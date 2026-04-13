# 带 VTST 的 VASP 编译
参考链接 1：https://zhuanlan.zhihu.com/p/8591076280  
参考链接 2：https://theory.cm.utexas.edu/vtsttools/installation.html

前置条件：下载并安装 Intel OneAPI
加载 intel OneAPI 环境变量

```bash
source /opt/intel/oneapi/setvars.sh
```

### 编译 VASP（以 VASP 6.4.3 为例）

将准备好的 VASP 压缩包解压:

```bash
# 解压第一层
unzip 'VASP 6.4.3.zip'
cd 'VASP 6.4.3'

# 解压第二层
tar -zxvf vasp.6.4.3.tgz
cd vasp.6.4.3
```

将 src 目录下的 main.F 和 .objects 复制一份防止出错：

```bash
cd src
cp main.F main.F.bk
cp .objects .objects.bk
```

修改 main.F：
将

```fortran
CALL CHAIN_FORCE(T_INFO%NIONS,DYN%POSION,TOTEN,TIFOR, &
     LATT_CUR%A,LATT_CUR%B,IO%IU6)
```

替换为

```fortran
CALL CHAIN_FORCE(T_INFO%NIONS,DYN%POSION,TOTEN,TIFOR, &
     TSIF,LATT_CUR%A,LATT_CUR%B,IO%IU6)
```

将

```fortran
IF (LCHAIN) CALL chain_init( T_INFO, IO)
```

替换为

```fortran
CALL chain_init( T_INFO, IO)
```

下载 VTST 插件：https://theory.cm.utexas.edu/vtsttools/download.html  
解压：

```bash
tar -zxvf vtstcode-213.tgz
```

选取合适的版本（我使用的是 6.4.3），将里面的内容复制到 vasp 的 src 目录下，修改 .objects 文件，根据 vtst 的版本在 chain.o 文本前面加上官网给的内容 https://theory.cm.utexas.edu/vtsttools/installation.html  注意 “\” 符号 

如果是vtstcode6.3/4/5，还需修改 vasp/src 的 makefile 文件，找到变量 LIB，修改为：

```bash
LIB= lib parser pyamff_fortran
```

如果是并行编译的，你需要在 makefile 中添加 libs 到你的依赖项中：

```bash
dependencies: sources libs
```

**其他内容与常规 VASP 编译一致，不要忘记修改**

编译 VASP

```bash
make std # 单核编译
make gam
make ncl
```

