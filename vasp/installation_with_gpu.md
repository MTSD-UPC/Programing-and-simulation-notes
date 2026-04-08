# 使用 GPU 的 VASP 编译
参考教程：https://blog.csdn.net/liouver/article/details/140653183

前置条件 1：下载并安装 Intel OneAPI
加载 intel OneAPI 环境变量

```bash
source /opt/intel/oneapi/setvars.sh
```

前置条件 2： 下载并安装 NVIDIA 驱动和 CUDA
配置 CUDA 环境

```bash
export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
```

前置条件 3：下载并安装NVIDIA HPC SDK
下载地址：https://developer.nvidia.com/hpc-sdk/releases
**根据 cuda 版本，选择合适的 hpc-sdk 版本**，本人 cuda 版本为 12.8，因此选择了 hpc sdk 25.3：

```bash
wget https://developer.download.nvidia.com/hpc-sdk/25.3/nvhpc_2025_253_Linux_x86_64_cuda_12.8.tar.gz
tar xpzf nvhpc_2025_253_Linux_x86_64_cuda_12.8.tar.gz
nvhpc_2025_253_Linux_x86_64_cuda_12.8/install
```

进入安装界面，选择“3 Auto install”，然后输入安装目录，默认安装在 `/opt/nvidia/hpc_sdk`，我安装在了个人用户目录里（/home/zenghui/software/nvidia/hpc_sdk）

安装完成后，在 ~/.bashrc 中设置环境变量：

```bash
export PATH=/home/zenghui/software/nvidia/hpc_sdk/Linux_x86_64/25.3/compilers/bin:$PATH
export MANPATH=/home/zenghui/software/nvidia/hpc_sdk/Linux_x86_64/25.3/compilers/man:$MANPATH
export PATH=/home/zenghui/software/nvidia/hpc_sdk/Linux_x86_64/25.3/comm_libs/mpi/bin:$PATH
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
cp arch/makefile.include.nvhpc_ompi_mkl_omp_acc makefile.include

# 修改编译配置文件
# 根据GPU版本修改-gpu后面的参数，本人的CUDA版本为12.8，显卡是RTX4080S，更改为cc89，cc90
CC          = mpicc  -acc -gpu=cc89,cc90,cuda12.8 -mp
FC          = mpif90 -acc -gpu=cc89,cc90,cuda12.8 -mp
FCL         = mpif90 -acc -gpu=cc89,cc90,cuda12.8 -mp -c++libs

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
