# xTB 简单使用
在线手册：[User Guide to Semiempirical Tight Binding — xtb doc 2023 documentation](https://xtb-docs.readthedocs.io/en/latest/index.html)

### 安装

前往 GitHub 寻找预编译版本：https://github.com/grimme-lab/xtb/releases/latest
下载 xtb-6.7.1-linux-x86_64.tar.xz 压缩包即可（目前最新版是 6.7.1）

使用 `tar` 命令解压到当前目录下：

```bash
tar -xvJf xtb-6.7.1-linux-x86_64.tar.xz
```

进入 xtb 目录内会发现一个 `config_env.bash` 文件

```bash
cd xtb-dist/share/xtb
```

每次登录账号后使用 `source` 命令运行该文件即可激活 xtb 环境

```
source config_env.bash
```

激活环境后可直接输入 `xtb` 命令，会发现 xtb 能够运行但会报错，这是因为需要输入文件

```
########################################################################
[ERROR] Program stopped due to fatal error
-2- Command line argument parsing failed
-1- prog_main: No input file given, so there is nothing to do
########################################################################
abnormal termination of xtb
```

### 结构文件

目前 xtb 支持的结构文件格式有：

| Format                | Basename        | Suffix                | molecular | periodic |
| --------------------- | --------------- | --------------------- | --------- | -------- |
| Turbomole             | coord           | coord, tmol           | x         | 3D       |
| xyz file              |                 | xyz                   | x         |          |
| mol file              |                 | mol                   | x         |          |
| Structure-Data file   |                 | sdf                   | x         |          |
| Protein Database file |                 | pdb                   | x         |          |
| Vasp’s POSCAR/CONTCAR | poscar, contcar | vasp, poscar, contcar |           | 3D       |
| genFormat             |                 | gen                   | x         | 3D       |
| Gaussian external     |                 | ein                   | x         |          |

**coord 文件格式如下：**

h2o.coord
```
$coord
 0.00000000000000      0.00000000000000     -0.73578586109551      o
 1.44183152868459      0.00000000000000      0.36789293054775      h
-1.44183152868459      0.00000000000000      0.36789293054775      h
$end
```

mgo.coord
```
$coord frac
    0.00      0.00      0.00      mg
    0.50      0.50      0.50      o
$periodic 3
$cell
 5.798338236 5.798338236 5.798338236 60. 60. 60.
$end
```

**xyz 文件格式如下：**

```
24

C          1.07317        0.04885       -0.07573
N          2.51365        0.01256       -0.07580
C          3.35199        1.09592       -0.07533
N          4.61898        0.73028       -0.07549
C          4.57907       -0.63144       -0.07531
C          3.30131       -1.10256       -0.07524
...
```

**pdb 文件格式如下：**

```
ATOM      1  N   GLY Z   1      -0.821  -2.072  16.609  1.00  9.93           N
ATOM      2  CA  GLY Z   1      -1.705  -2.345  15.487  1.00  7.38           C
ATOM      3  C   GLY Z   1      -0.968  -3.008  14.344  1.00  4.89           C
ATOM      4  O   GLY Z   1       0.258  -2.982  14.292  1.00  5.05           O
ATOM      5  HA2 GLY Z   1      -2.130  -1.405  15.135  1.00  0.00           H
ATOM      6  HA3 GLY Z   1      -2.511  -2.999  15.819  1.00  0.00           H
ATOM      7  H1  GLY Z   1      -1.364  -1.742  17.394  1.00  0.00           H
ATOM      8  H2  GLY Z   1      -0.150  -1.365  16.344  1.00  0.00           H
ATOM      9  H3  GLY Z   1      -0.334  -2.918  16.868  1.00  0.00           H
ATOM     10  N   ASN Z   2      -1.721  -3.603  13.425  1.00  3.53           N
ATOM     11  CA  ASN Z   2      -1.141  -4.323  12.291  1.00  1.85           C
...
```

### 单点能计算

准备一个水分子的结构文件：

h2o.coord
```
$coord
 0.00000000000000      0.00000000000000     -0.73578586109551      o
 1.44183152868459      0.00000000000000      0.36789293054775      h
-1.44183152868459      0.00000000000000      0.36789293054775      h
$end
```

运行 xtb 即可计算单点能：

```
xtb h2o.coord
```

### 结构优化

只需要加上一个 `--opt` 即可使用默认参数进行结构优化

```
xtb 结构文件 --opt
```

### 分子动力学

只需要加上一个 `--omd` 或者 `--md` 即可使用默认参数进行分子动力学模拟

```
xtb 结构文件 --omd
```

如果需要修改参数，需要编写一个参数文件，参数说明在手册上可以找到

md.inp
```
$md
   temp=298.15 # in K
   time= 50.0  # in ps
   dump= 50.0  # in fs
   step=  4.0  # in fs
   velo=false
   nvt =true
   hmass=4
   shake=2
   sccacc=2.0
$end
```

输入命令时加上 `--input 参数文件` 即可

```
xtb 结构文件 --input md.inp --omd
```

