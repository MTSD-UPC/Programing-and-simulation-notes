# 作业调度系统常用命令
### 一、PBS作业调度系统
**01/ 常用命令**
```bash
# 查看所有节点信息
[zenghui@dimmy ~]$ qnodes
node01
     state = job-exclusive
     power_state = Running
     np = 48
     ntype = cluster
     jobs = 0-47/569359.dimmy
......

# 查看某一节点信息
[zenghui@dimmy ~]$ qnodes node02

# 查看所有节点状态
[zenghui@dimmy ~]$ qnodes -l all
node01               job-exclusive
node02               free
node03               free
node04               job-exclusive
......

# 查看空闲节点
[zenghui@dimmy ~]$ qnodes -l free

# 提交作业
[zenghui@dimmy ~]$ qsub run_vasp6.4.pbs 
569362.dimmy

# 列出作业
[zenghui@dimmy ~]$ qstat
Job ID                    Name             User            Time Use S Queue
------------------------- ---------------- --------------- -------- - -----
569362.dimmy               vasp-6.4         zenghui                0 Q brief 

# 显示某用户的所有任务状态
[zenghui@dimmy ~]$ qstat -u zenghui

# 显示作业号为569362的作业信息
[zenghui@dimmy ~]$ qstat -f 569362
Job Id: 569362.dimmy
    Job_Name = vasp-6.4
    Job_Owner = zenghui@dimmy
    job_state = C
    queue = brief
    server = dimmy
......

# 查看所有队列作业状态
[zenghui@dimmy ~]$ qstat -q

# 删除作业
[zenghui@dimmy ~]$ qdel 569362
```
**02/ 提交任务脚本文件样例**
```bash
#!/bin/bash
#PBS -N vasp-6.4          # 任务名称
#PBS -l nodes=1:ppn=48    # 使用1个节点，每个节点48核
#PBS -q brief             # 分区名称
#PBS -j oe
##################################
source /opt/intel/oneapi/setvars.sh

cd $PBS_O_WORKDIR
ulimit -s 102400
NP=`cat $PBS_NODEFILE | wc -l`
echo "The job id is $PBS_JOBID" > out.log
echo "Work directory is $PBS_O_WORKDIR" >> out.log
echo "Number of processes is $NP" >> out.log
echo "Begin time is `date`" >> out.log
##################################
mpirun -np $NP /opt/vasp/6.4.1/intel21/vasp_std

echo "End time is `date`" >> out.log
```

### 二、Slurm作业调度系统
**01/ 常用命令**
```bash
# 查看集群状态，类似 qnodes
[wangzj@lucky ~]$ sinfo

# 查看节点级信息，类似qnodes -l all、 bhosts等
[wangzj@lucky ~]$ sinfo -N
node13 1 guowy alloc #已占用
node14 1 guowy idle #空闲
node15 1 guowy down #故障
node16 1 guowy unk #未知
.....

# 提交作业，类似qsub、bsub等
[wangzj@lucky ~]$ sbatch run_vasp.sh
Submitted batch job 152

# 列出作业，类似qstat、bjobs等
[wangzj@lucky ~]$ squeue
JOBID PARTITION     NAME     USER ST       TIME NODES NODELIST(REASON)
152     guowy     test   wangzj R       0:29      1 node13

# 显示某用户的所有任务状态
[wangzj@lucky ~]$ squeue -u XXXXX(用户名)

# 删除作业，类似qdel、bkill
[wangzj@lucky ~]$ scancel 152

# 显示作业号为152的作业信息，类似于qstat -f 152,
[wangzj@lucky ~]$ scontrol show job 152

# 显示所有node节点的硬件信息
[wangzj@lucky ~]$ scontrol show node

# 查看名字为node02的节点的硬件信息
[wangzj@lucky ~]$ scontrol show node node02
```
**02/ 提交任务脚本文件样例**
```bash
#!/bin/bash
#SBATCH --job-name="vasp"       # 任务名称
#SBATCH -N 1                    # 节点数
#SBATCH --partition=guowy       # 分区名称
#SBATCH --cpus-per-task=32      # 每个节点32核
#SBATCH --output=logs_%j.out
#SBATCH --error=logs_%j.err
#SBATCH --time=120:30:00
#################################
NP=32        # NP=32*节点数
source /home/software/intel/oneapi/setvars.sh intel64

echo Begin time is `date`
echo "The job id is : $SLURM_JOBID "
echo -n 'Nodelist is : '
echo $SLURM_JOB_NODELIST
echo -n 'We work on: '
echo $SLURM_SUBMIT_DIR
##################################
VASP=/home/software/vasp/6.4.1/intel21/vasp_std
mpirun -np $NP $VASP >& output

echo End time is `date`
```

### 附录：PBS和Slurm的一些比较

| **功能**      | **PBS**                 | **Slurm**                    |
| ------------- | ----------------------- | ---------------------------- |
| 任务名称      | `#PBS -N name`          | `#SBATCH -J name`            |
| 指定队列/分区 | `#PBS -q cpu`           | `#SBATCH -p cpu`             |
| 最长运行时间  | `#PBS -l walltime=5:00` | `#SBATCH -t 5:00`            |
| 指定节点数量  | `#PBS -l nodes=1`       | `#SBATCH -N 1`               |
| 指定特定节点  | `qsub -l nodes=comput1` | `#SBATCH --nodelist=comput1` |
| 指定 CPU 核心 | `#PBS -l ppn=4`         | `#SBATCH --cpus-per-task=4`  |
| 指定 GPU 卡   | 不支持                  | `#SBATCH --gres=gpu:1`       |
| 输出文件      | `#PBS -o test.out`      | `#SBATCH -o test.out`        |
| 提交任务脚本  | `qsub run.pbs`          | `sbatch run.slurm`           |
| 查看任务状态  | `qstat`                 | `squeue`                     |
| 取消任务      | `qdel 1234`             | `scancel 1234`               |
