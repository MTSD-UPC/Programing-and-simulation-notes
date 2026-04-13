### 远程访问 Jupyter Notebook

我们尝试在服务器上开启 Jupyter Notebook，并通过自己电脑上的浏览器远程访问它。

**确保在服务器上激活了 Anaconda 环境并存在 Jupyter Notebook**，使用下列命令生成 Jupyter Notebook 的配置文件：

```bash
(mytest) [peter@DESKTOP-RI88H2O ~]$ jupyter notebook --generate-config
Writing default config to: /home/peter/.jupyter/jupyter_notebook_config.py
```

接下来修改配置文件，旧版本和新版本的 Jupyter Notebook在配置上有一些区别：

**对于旧版本 Jupyter Notebook：**

```bash
# 使用vim修改配置文件
vim ~/.jupyter/jupyter_notebook_config.py

# 修改以下地方
c.NotebookApp.ip = "*"  # 表示所有IP可以访问
c.NotebookApp.open_browser = False  # 启动Jupyter Notebook后不会弹出网页
c.NotebookApp.port = 8888  # 可以自行修改端口

## Hashed password to use for web authentication.
#
#  To generate, type in a python/IPython shell:
#
#    from notebook.auth import passwd; passwd()
#
#  The string should be of the form type:salt:hashed-password.
c.NotebookApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$zZd5MfX2gYqcHbvbk7Dx6Q$ZiP5UGotAZ1t97YBjU9E0A'  # 密码需要通过上述方式生成，可以生成自己喜欢的密码，这里是123456

```

**对于新版本 Jupyter Notebook（7.0版本开始）：**

```bash
# 使用vim修改配置文件
vim ~/.jupyter/jupyter_notebook_config.py

# 修改以下地方
c.ServerApp.ip = '0.0.0.0'  # 表示所有IP可以访问
c.ServerApp.open_browser = False  # 启动Jupyter Notebook后不会弹出网页
c.ServerApp.port = 8899  # 可以自行修改端口

#  To generate hashed password, type in a python/IPython shell:
#
#    from jupyter_server.auth import passwd; passwd()
c.ServerApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$T9oCNCWqg9Spkovr2JO7fg$uDjFPZSZJjhMgVLACEnb3ZVM5cLoTNCZfuoZ2Z1f4/U'  # 密码需要通过上述方式生成，可以生成自己喜欢的密码，这里是123456

```

输入 `jupyter notebook` 命令即可在服务器端开启 Jupyter Notebook，在自己电脑的浏览器上输入服务器的 `http://IP:port` 便可远程访问。
