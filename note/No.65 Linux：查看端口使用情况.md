# No.65 Linux：查看端口使用情况

## 导读

> 使用Linux的过程中是不是经常碰到端口问题，开启、占用、ping不通等等，命令又经常忘记，所以整理一下，边用边看。

### 1、查看到进程占用的端口号

```shell
netstat -lnp | grep 5000
netstat -anp | grep pid
pgrep python3 | xargs kill -s 9
```

### 2、查看8000端口的使用情况

```shell
lsof -i:8000
```

### 3、netstat命令各个参数说明如下

```shell
-t：指明显示TCP端口
-u：指明显示UDP端口
-n：不进行DNS轮询，显示IP(可以加速操作)
-p：显示进程标识符和程序名称，每一个套接字/端口都属于一个程序
-l：仅显示监听套接字(所谓套接字就是使应用程序能够读写与收发通讯协议(protocol)与资料的程序)
```

### 4、其他命令

```shell
# 查看当前所有tcp端口
netstat -ntlp

# 查看所有80端口使用情况
netstat -ntulp |grep 80

# 查看所有3306端口使用情况
netstat -an | grep 3306

# 查看一台服务器上面哪些服务及端口
netstat  -lanp

# 查看一个服务有几个端口。比如要查看mysqld
ps -ef |grep mysqld

# 查看某一端口的连接数量,比如3306端口
netstat -pnt |grep :3306 |wc

# 查看某一端口的连接客户端IP，比如3306端口
netstat -anp |grep 3306

# 查看网络端口
netstat -an

# 端口扫描
nmap

# UDP类型的端口
netstat -nupl

# TCP类型的端口
netstat -ntpl

# 显示系统端口使用情况
netstat -anp
```
