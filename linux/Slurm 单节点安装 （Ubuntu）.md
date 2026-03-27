注1：本文档参考[1. Slurm简介 — Slurm资源管理与作业调度系统安装配置 2021-12 文档](https://scc.ustc.edu.cn/hmli/doc/linux/slurm-install/slurm-install.html)
注2：本文档仅适用于单台服务器，采用的发行版为 Ubuntu24.04，所有命令均使用 root 用户在命令行下执行。

> **Slurm** (**S**imple **L**inux **U**tility for **R**esource **M**anagement) 是开源的、具有容错性和高度可扩展的 Linux集群资源管理和作业调度系统。Linux 操作系统可利用 Slurm 对资源和作业进行管理，以避免相互干扰，提高运行效率。

### 编译安装 Slurm

**安装编译 Slurm 所需软件包**

```bash
apt-get install libmunge-dev libmariadb-dev libpam0g-dev libcgroup-dev libhwloc-dev build-essential fakeroot devscripts equivs
```

**下载 Slurm 源码包**

```bash
wget https://download.schedmd.com/slurm/slurm-24.05.7.tar.bz2
```

**编译成 DEB 包**

+ 解压缩
```bash
tar -xaf slurm-24.05.7.tar.bz2
```

+ 进入解压后的目录
```bash
cd slurm-24.05.7
```

+ 修改 debian/control 增加或删除所需要包，然后执行命令安装所依赖的包：
```bash
mk-build-deps -i debian/control
```

+ 生成 Slurm 安装包：
```bash
debuild -b -us -us
```

正常结束后，会在上级目录生成DEB包及相关文件

### 管理节点操作

**设置 Slurm 的 DEB 软件仓库**

+ 建立 APT 仓库目录：
```bash
mkdir -p /opt/slurm/debs
```

+ 复制前面生成的 APT 文件到 /opt/slurm/debs/ 目录下：
```bash
cp slurm*.deb /opt/slurm/debs
```

+ 安装 APT 包索引生成工具：
```bash
apt-get install dpkg-dev
```

+ 生成本地库索引，在 /opt/slurm/ 目录下执行：
```bash
cd /opt/slurm
dpkg-scanpackages debs /dev/null | gzip > debs/Packages.gz
```

+ 生成源配置文件：
```bash
echo "deb [trusted=yes] file:/opt/slurm debs/" >> /etc/apt/sources.list.d/slurm.list
```

+ 更新一下源：
```bash
apt-get update
```

**安装所需 slurm 包**

```bash
apt -y install libswitch-perl munge slurm-smd slurm-smd-slurmctld slurm-smd-client slurm-smd-slurmdbd slurm-smd-slurmd
```

**生成 slurm 用户**

```bash
useradd slurm
```

**设置主配置文件**

主配置文件：/etc/slurm/slurm.conf 
内容模板可访问 https://slurm.schedmd.com/congurator.html
以下是我的一些设置：
```
# 根据实际情况修改
ClusterName=tensor
SlurmctldHost=tensor
GresTypes=gpu

ProctrackType=proctrack/cgroup
ReturnToService=1
SlurmctldPidFile=/var/run/slurmctld.pid
SlurmctldPort=6817
SlurmdPidFile=/var/run/slurmd.pid
SlurmdPort=6818
SlurmdSpoolDir=/var/spool/slurmd
SlurmUser=slurm
StateSaveLocation=/var/spool/slurmctld
TaskPlugin=task/affinity,task/cgroup
InactiveLimit=0
KillWait=30
MinJobAge=300
SlurmctldTimeout=120
SlurmdTimeout=300
Waittime=0
SchedulerType=sched/backfill
SelectType=select/cons_tres
SelectTypeParameters=CR_Core
JobCompType=jobcomp/none
JobAcctGatherFrequency=30
SlurmctldDebug=info
SlurmctldLogFile=/var/log/slurmctld.log
SlurmdDebug=info
SlurmdLogFile=/var/log/slurmd.log

# 根据实际情况修改
NodeName=tensor NodeAddr=127.0.0.1 CPUs=32 RealMemory=60000 Sockets=1 CoresPerSocket=16 ThreadsPerCore=2 Gres=gpu:1 State=UNKNOWN
PartitionName=normal Nodes=ALL Default=YES MaxTime=INFINITE State=UP
```

**设置 cgroup 限制**

cgroup 配置文件：/etc/slurm/cgroup.conf

```
ConstrainCores=yes
ConstrainDevices=yes
ConstrainRAMSpace=yes
```

启用 cgroup 资源限制，可以防止用户实际使用的资源超过用户为该作业通过作业调度系统申请到的资源。如不需限制，不要在 /etc/slurm/slurm.conf 中设定 ProctrackType=proctrack/cgroup 及 TaskPlugin=task/cgroup 参数

**GPU 等资源配置**

/etc/slurm/gres.conf 文件内容：
```
NodeName=ecm-ab83 Name=gpu File=/dev/nvidia[0-1]
```

**设置 Slurm 文件权限**

+ /etc/slurm/slurm.conf 文件所有者须为 root 用户：
```bash
chown root /etc/slurm/slurm.conf
```

+ 建立 slurmctld 服务存储其状态等的目录，由 slurm.conf 中 StateSaveLocation 参数定义：
```bash
mkdir /var/spool/slurmctld
```

+ 设置 /var/spool/slurmctld 目录所有者为 slurm 用户：
```bash
chown slurm /var/spool/slurmctld
```

**计算节点设置slurmd服务**

在 /usr/lib/systemd/system/ 下建立 slurmd.service 文件，其内容为：
```
[Unit]
Description=Slurm node daemon
After=munge.service network-online.target remote-fs.target sssd.service
Wants=network-online.target
#ConditionPathExists=/etc/slurm/slurm.conf

[Service]
Type=notify
EnvironmentFile=-/etc/sysconfig/slurmd
EnvironmentFile=-/etc/default/slurmd
RuntimeDirectory=slurm
RuntimeDirectoryMode=0755
ExecStart=/usr/sbin/slurmd --systemd $SLURMD_OPTIONS
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
LimitNOFILE=131072
LimitMEMLOCK=infinity
LimitSTACK=infinity
Delegate=yes
TasksMax=infinity

# Uncomment the following lines to disable logging through journald.
# NOTE: It may be preferable to set these through an override file instead.
#StandardOutput=null
#StandardError=null

[Install]
WantedBy=multi-user.target
```

**重启 slurmctld 服务和 slurmd 服务**

```bash
systemctl restart slurmctld
systemctl restart slurmd
```
