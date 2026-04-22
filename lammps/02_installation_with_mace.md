# 带 MACE 的 LAMMPS 安装
参考链接 1：[Installation — mace 0.3.13 documentation](https://mace-docs.readthedocs.io/en/latest/guide/installation.html)  
参考链接 2：[MACE in LAMMPS with ML-IAP — mace 0.3.13 documentation](https://mace-docs.readthedocs.io/en/latest/guide/lammps_mliap.html)

### MACE 安装

使用 Anaconda 创建 mace 虚拟环境，选择合适的 Python 版本

```bash
conda create -n mace_env python=3.10
```

安装 pytorch（选择版本：[Previous PyTorch Versions](https://pytorch.org/get-started/previous-versions/)）

安装 mace

```bash
pip install --upgrade pip
pip install mace-torch
```

### LAMMPS 安装

激活 Intel OneAPI 环境，如果没有，也可以安装 mpich 或 mpi4py

```bash
pip install mpich
```

安装 Python 依赖

```bash
pip install cuequivariance-torch
pip install cuequivariance
pip install cuequivariance-ops-torch-cu12
pip install cupy-cuda12x
```

下载 LAMMPS

```bash
git clone https://github.com/lammps/lammps.git
cd lammps
```

创建 build 目录

```
mkdir build-mliap
cd build-mliap
```

复制并指定你的 GPU 架构

```bash
cp ../cmake/presets/kokkos-cuda.cmake ./
# Edit kokkos-cuda.cmake to set the correct architecture
# Find your architecture in: https://docs.lammps.org/Build_extras.html#kokkos
```

配置 CMake （确保 mace 虚拟环境已经激活）

```
cmake -C kokkos-cuda.cmake \
  -D CMAKE_BUILD_TYPE=Release \
  -D CMAKE_INSTALL_PREFIX=$(pwd) \
  -D BUILD_MPI=ON \
  -D PKG_ML-IAP=ON \
  -D PKG_ML-SNAP=ON \
  -D MLIAP_ENABLE_PYTHON=ON \
  -D PKG_PYTHON=ON \
  -D BUILD_SHARED_LIBS=ON \
  ../cmake
```

注：如果没有 cython 就 pip 安装 cython

编译 LAMMPS

```bash
make -j 8
```

创建并安装 LAMMPS 的 Python 包

```bash
make install-python
```

配置好环境后运行 LAMMPS

```
lmp -k on g 1 -sf kk -pk kokkos newton on neigh half -in your_input.in
```
