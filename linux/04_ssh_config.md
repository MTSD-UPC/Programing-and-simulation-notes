# SSH 别名

参考链接 1：https://blog.csdn.net/qq_51447496/article/details/132090066
参考链接 2：https://blog.csdn.net/qq_45141261/article/details/154385784

新手使用 ssh 命令时往往需要输入用户、IP地址等：

```bash
ssh -p 2222 alice@203.0.113.10
```

那有没有办法可以省略这些内容？

答案是可以的，我们可以修改 SSH 配置文件来实现这一目的

如果你的个人电脑是 Windows 系统，配置文件的路径为 `$HOME\.ssh\config` ，可以使用文本编辑器修改，如果没有则新建一个；如果你使用了 WSL，配置文件的路径为 `~/.ssh/config`，可以使用 Vim 修改，如果没有则新建一个。

在配置文件中通常加入如下内容：

```
Host dev
    HostName 203.0.113.10
    User alice
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
```

其中，`dev` 为服务器别名，`203.0.113.10` 为服务器 IP，`alice` 为用户名，`2222` 为端口号，默认为 22，`~/.ssh/id_ed25519` 为私钥路径，没有则不加。

保存你的配置便可直接用别名登录服务器了：

```bash
ssh dev
```

# 免密登录

生成 SSH 密钥对：

```bash
ssh-keygen -t rsa
```

将公钥添加到远程服务器：

```bash
ssh-copy-id user@server_ip
```

# 跳板机

```
Host my-jump-box
    HostName 192.168.1.50
    Port 22002
    User jump_admin
    # IdentityFile ~/.ssh/id_rsa_jump

Host my-target-server
    HostName 10.0.1.100
    Port 22001
    User dev_user
    # IdentityFile ~/.ssh/id_rsa_target
    ProxyJump my-jump-box
```
