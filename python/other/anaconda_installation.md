# Anaconda 简易安装及虚拟环境搭建（Linux）
### 安装 Anaconda

> Anaconda 是一个用于数据科学，机器学习和深度学习的开源软件包管理系统，其中包括了许多流行的 Python 包和工具。

Anaconda 官网：https://www.anaconda.com

手动下载 Anaconda：https://www.anaconda.com/download/success

通过命令下载 Anaconda：

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
```

给安装包添加可执行权限：

```bash
chmod +x Anaconda3-2024.02-1-Linux-x86_64.sh
```

开始安装：

```bash
./Anaconda3-2024.02-1-Linux-x86_64.sh

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>  # 回车即可

Do you accept the license terms? [yes|no]
>>> yes  # 输入yes

Anaconda3 will now be installed into this location:
/home/peter/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/peter/anaconda3] >>> /home/peter/software/anaconda3  # 输入自己的安装路径

Do you wish to update your shell profile to automatically initialize conda?
This will activate conda on startup and change the command prompt when activated.
If you'd prefer that conda's base environment not be activated on startup,
   run the following command when conda is activated:

conda config --set auto_activate_base false

You can undo this by running `conda init --reverse $SHELL`? [yes|no]
[no] >>> no  # 输入yes会直接帮你配置好环境，为了展示更多细节，这里我选择no
```


### 配置 Anaconda 环境

> Conda 是 Anaconda 中的包管理器，用来安装和管理软件包，也可以部署和管理环境。

**如果上一步选择 yes，可直接跳过本节**；如果上一步选择 no，那么直接输入 conda 会显示找不到命令，这是因为我们还没有配置好环境：

```bash
[peter@DESKTOP-RI88H2O software]$ conda
conda: command not found
```

有两种方法配置环境：

**第一种**：vim 修改 ~/.bashrc 文件，在最后一行加上（记得改成自己的路径）：

```bash
source /home/peter/software/anaconda3/etc/profile.d/conda.sh
```

重新登陆或刷新配置：

```bash
source ~/.bashrc
```

测试效果：

```bash
[peter@DESKTOP-RI88H2O bin]$ conda
usage: conda [-h] [-v] [--no-plugins] [-V] COMMAND ...

conda is a tool for managing and deploying applications, environments and packages.
```


**第二种**：进入自己 anaconda 的 bin 目录，运行 conda init

```bash
[peter@DESKTOP-RI88H2O bin]$ ./conda init
no change     /home/peter/software/miniconda3/condabin/conda
no change     /home/peter/software/miniconda3/bin/conda
no change     /home/peter/software/miniconda3/bin/conda-env
no change     /home/peter/software/miniconda3/bin/activate
no change     /home/peter/software/miniconda3/bin/deactivate
no change     /home/peter/software/miniconda3/etc/profile.d/conda.sh
no change     /home/peter/software/miniconda3/etc/fish/conf.d/conda.fish
no change     /home/peter/software/miniconda3/shell/condabin/Conda.psm1
no change     /home/peter/software/miniconda3/shell/condabin/conda-hook.ps1
no change     /home/peter/software/miniconda3/lib/python3.12/site-packages/xontrib/conda.xsh
no change     /home/peter/software/miniconda3/etc/profile.d/conda.csh
modified      /home/peter/.bashrc

==> For changes to take effect, close and re-open your current shell. <==
```

重新登录测试效果：

```bash
(base) [peter@DESKTOP-RI88H2O ~]$  # 前面多了一个base，这是进入了base虚拟环境
(base) [peter@DESKTOP-RI88H2O ~]$ conda
usage: conda [-h] [-v] [--no-plugins] [-V] COMMAND ...

conda is a tool for managing and deploying applications, environments and packages.
```

其实 conda init 也是修改了 ~/.bashrc。


### 搭建虚拟环境

**base 虚拟环境**

通过第二个方法我们发现自己直接进入了一个名为 base 的虚拟环境，它是一个独立的 Python 环境，在里面安装的 Python 包不会影响到外面的 Python 环境。**Anaconda 安装后默认有一个 base 虚拟环境**。

退出base虚拟环境：

```bash
(base) [peter@DESKTOP-RI88H2O ~]$ conda deactivate
[peter@DESKTOP-RI88H2O ~]$
```

再次进入base虚拟环境：

```bash
[peter@DESKTOP-RI88H2O ~]$ conda activate
(base) [peter@DESKTOP-RI88H2O ~]$
```

**为什么需要虚拟环境**？

假设你有两个 Python 项目，一个需要安装 **pytorch1.x.x** 版本，而另一个需要安装 **pytorch2.x.x** 版本，这时候就体现虚拟环境的重要性了。

**创建自己的虚拟环境**

这里我创建一个名为 mytest 的虚拟环境，使用的 Python 版本为 3.10

```bash
conda create --name mytest python=3.10
```

激活 mytest 环境：

```bash
conda activate mytest
```

查看已创建的虚拟环境：

```bash
conda env list
```

查看 conda 虚拟环境中安装的 Python 包：

```bash
conda list
```

最后，用不到虚拟环境的时候，记得退出：

```bash
(mytest) [peter@DESKTOP-RI88H2O ~]$ conda deactivate
[peter@DESKTOP-RI88H2O ~]$
```

