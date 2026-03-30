# 带 DFT-D4 的 VASP 编译
前置条件：下载并安装 Intel OneAPI
加载 intel OneAPI 环境变量

```bash
source /opt/intel/oneapi/setvars.sh
```

### dftd4 编译

pip 安装 meson 以及 ninja 或者前往 GitHub 下载其安装包，如果有管理员权限，也可以使用 yum/apt 安装

```bash
pip install meson
pip install ninja
```

前往 GitHub 下载 dftd4、json-fortran、mctc-lib、mstore 和 multicharge 的源码包，解压所有的源码包（本文的 dftd4 版本为 3.7.0，json-fortran 版本为 8.2.5，mctc-lib 版本为 0.3.2，mstore 版本为 0.3.0，multicharge 版本为 0.3.0）

```bash
unzip *.zip  # zip格式使用这个
tar -zxvf *.tar.gz  # tar.gz格式使用这个
```

进入 dftd4 的源码包内，将其他插件拷贝到 subprojects 目录内

```bash
cd dftd4-3.7.0
cp -r ../json-fortran-8.2.5 subprojects/json-fortran-8.2.5
cp -r ../mctc-lib-0.3.2 subprojects/mctc-lib
cp -r ../mstore-0.3.0 subprojects/mstore
cp -r ../multicharge-0.3.0 subprojects/multicharge
```

使用 meson 命令编译 dftd 到 build 目录下

```bash
FC=ifort CC=icc CXX=icpc meson setup build -Dfortran_link_args=-qopenmp
```

可能会出现找到 json-fortran 但没有 meson.build 文件，这时候需要去 mctc-lib 的 subprojects 里面复制一份 meson.build 到 json-fortran 目录中

测试

```bash
meson test -C build --print-errorlogs
```

设置安装位置并安装

```bash
meson configure build --prefix=/path/to/your/install
meson install -C build
```

### VASP 编译

与常规编译的区别在于：需要在 makefile.include 的末尾加上三行

```bash
CPP_OPTIONS += -DDFTD4
LLIBS       += -L/xxx/xxx/lib64 -ldftd4
INCS        += -I/xxx/xxx/include -I/xxx/xxx/include/dftd4/intel-2021.4.0  # intel版本根据实际情况而定
```

编译 VASP

```bash
make std # 单核编译
make gam
make ncl
```

在运行 VASP 前需要设置 dftd4 的环境变量

```bash
export LD_LIBRARY_PATH=/xxx/xxx/lib64:$LD_LIBRARY_PATH
```
