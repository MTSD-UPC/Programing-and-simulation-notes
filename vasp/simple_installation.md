# VASP 简易安装（不含插件）
### 前置条件：安装 intel 的 oneAPI 工具

下载地址：https://www.intel.cn/content/www/cn/zh/developer/tools/oneapi/toolkits.html

只需要下载并安装 [**Intel® oneAPI Base Toolkit**](https://www.intel.cn/content/www/cn/zh/developer/tools/oneapi/toolkits.html#base-kit) 和 [**Intel® oneAPI HPC Toolkit**](https://www.intel.cn/content/www/cn/zh/developer/tools/oneapi/toolkits.html#hpc-kit) 两种

安装 oneAPI 工具包：

```bash
sh l_BaseKit_p_2024.1.0.596_offline.sh
sh l_HPCKit_p_2024.1.0.560_offline.sh
```

配置 intel 的 oneAPI 环境：

```bash
source ~/intel/oneapi/setvars.sh
```

如果管理员已经在 /opt 下安装好了 oneAPI 工具包的话就直接：

```bash
source /opt/intel/oneapi/setvars.sh
```

### 编译 VASP（以 VASP 6.4.3 为例）

将准备好的VASP压缩包解压:

```bash
# 解压第一层
unzip 'VASP 6.4.3.zip'
cd 'VASP 6.4.3'

# 解压第二层
tar -zxvf vasp.6.4.3.tgz
cd vasp.6.4.3
```

准备配置文件并进行编译：

```bash
# 复制编译配置文件
cp arch/makefile.include.intel makefile.include

# 修改编译配置文件
vim makefile.include
FC          = mpiifx
FCL         = mpiifx
CC_LIB      = icx
CXX_PARS    = icpx
# (Note: for Intel Parallel Studio's MKL use -mkl instead of -qmkl)
FCL        += -qmkl=sequential

# 编译
make std 
```



### 配置和运行 VASP

编译完成后在 vasp6.4.3/bin 目录下会生成一个 vasp_std 可执行文件，记住这个目录，接着配置 vasp 环境，编辑 ~/.bashrc 文件，在末尾加入：

```bash
export PATH=/path/to/your/bin:$PATH
```

重新激活环境：

```bash
source ~/.bashrc
```

配置环境后可以输入如下命令运行 vasp 程序，需要提前准备好 VASP 的输入文件

```bash
mpirun -np N vasp_std  # N表示核数，比如设置为4
```
