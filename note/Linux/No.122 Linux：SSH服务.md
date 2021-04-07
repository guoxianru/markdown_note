# No.122 Linux：SSH服务

## 导读

> SSH是标准的网络协议，可用于大多数UNIX操作系统，能够实现字符界面的远程登录管理。

### 1、介绍

SSH提供了口令和密钥两种用户验证方式，这两者都是通过密文传输数据的。

不同的是，口令用户验证方式传输的是用户的账户名和密码，这要求输入的密码具有足够的复杂度才能具有更高的安全性。

而基于密钥的安全验证必须为用户自己创建一对密钥，并把共有的密钥放在需要访问的服务器上。当需要连接到SSH服务器上时，客户端软件就会向服务器发出请求，请求使用客户端的密钥进行安全验证。

服务器收到请求之后，先在该用户的根目录下寻找共有密钥，然后把它和发送过来的公有密钥进行比较。如果两个密钥一致，服务器就用公有的密钥加密“质询”，并把它发送给客户端软件。

客户端收到质询之后，就可以用本地的私人密钥解密再把它发送给服务器。这种方式是相当安全的。

### 2、安装SSH

ssh软件由两部分组成：ssh服务端和ssh客户端。

ssh的配置文件在/etc/ssh/目录下，其中服务端的配置文件是sshd_config，客户端的配置文件是ssh_config。

```shell
yum install -y openssh-*

```

### 3、配置ssh服务器

```shell
vim /etc/ssh/sshd_config

# 默认使用22端口，也可以自行修改为其他端口，但登录时要打上端口号
Port 22

# 指定提供ssh服务的IP
# ListenAddress

# 禁止以root远程登录
# PermitRootLogin

# 启用口令验证方式
# PasswordAuthentication

# 禁止使用空密码登录
# PermitEmptyPassword

# 重复验证时间为1分钟
# LoginGraceTime 1m

# 最大重试验证次数
# MaxAuthTries 6

```

- 修改默认端口，需要关闭SELinux

```shell
vim /etc/selinux/config

SELINUX=disabled

```

### 4、重启sshd服务

```shell
systemctl restart sshd.service
```
