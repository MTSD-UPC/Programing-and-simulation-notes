# LAMMPS 简易安装
### 前置要求：安装 mpi 和 fftw

按照 VASP 的安装教程安装 oneAPI （里面包含了 mpi 和 fftw）

配置 oneAPI 环境

```bash
source /opt/intel/oneapi/setvars.sh
```


### 编译安装 LAMMPS（能并行、可使用GPU）

假设我们已经配置好了相关环境，现在就开始正式编译安装 LAMMPS

LAMMPS 官网：https://www.lammps.org/

下载 LAMMPS 的安装包，并解压到当前目录下

```bash
# 下载
wget https://download.lammps.org/tars/lammps.tar.gz

# 解压
tar -zxvf lammps.tar.gz
```

编译 LAMMPS 源代码有 **make** 和 **CMake** 两种方法。相比于前者，后者更现代化且具有很多优点
如果读者缺乏编译软件的经验或者想修改 LAMMPS 源码，官方推荐选择后者

**编译方案一：make（以lammps-2Aug2023为例）**

```bash
# 进入lammps-2Aug2023/src目录
cd lammps-2Aug2023/src

# 选择要安装的包，这里我们把能安的都安上
make ps  # 显示LAMMPS包的状态
make yes-all  # 安装包全安装
make no-lib  # 有外链的不安装
```

添加 gpu 包，无 gpu 的服务器可跳过此步骤

确保已经有 NVIDIA 驱动和 CUDA 环境

```bash
# 检查NVIDIA驱动
nvidia-smi

# 检查CUDA环境
nvcc -V
```

接着进行如下操作

```bash
cd lammps-2Aug2023/lib/gpu
make -f Makefile.linux

cd lammps-2Aug2023/src
make yes-gpu  # 添加gpu包
```

最后编译 LAMMPS

```bash
make mpi -jN  # 编译并行版LAMMPS，N为需要使用的核数，比如把N设置为8
```

最后把编译好的 lmp_mpi 可执行文件复制到自己的目录下使用

**编译方案二：CMake（以lammps-29Aug2024为例）**

```bash
# 进入lammps-29Aug2024/src目录
cd lammps-29Aug2024

# 创建build目录并进入
mkdir build
cd build

# 配置LAMMPS安装特性
# -D PKG_GPU=yes表示安装gpu包，无gpu的服务器可不加这一项
# -D CMAKE_INSTALL_PREFIX指定安装位置
cmake ../cmake -C ../cmake/presets/most.cmake -C ../cmake/presets/oneapi.cmake -D BUILD_OMP=no -D PKG_GPU=yes -D CMAKE_INSTALL_PREFIX=/home/peter/software/lammps/29Aug2024/
cmake --build .

# 编译LAMMPS
make -jN  # 编译LAMMPS，N为需要使用的核数，比如把N设置为8

# 安装LAMMPS
make install 
```


### 运行 LAMMPS

进入 LAMMPS 的测试案例下，运行测试案例：

```bash
# 找到测试案例
cd lammps-2Aug2023/examples/shear

# 运行测试案例（使用gpu），需要提前设置好lammps环境变量
# lammps的可执行文件名称可能会变化，不一定是lmp_mpi
mpirun -np 2 lmp_mpi -sf gpu -pk gpu 1 -in in.shear
```

