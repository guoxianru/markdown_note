# No.49 Redis：安装方法

## 导读

> Redis在不同平台下的安装方法。

### 1、CentOS7

- 安装

```shell
yum install -y redis
```

- 下载安装fedora的epel仓库

```shell
yum install -y epel-release
```

- 启动服务并设置开机启动

```shell
# 启动服务
systemctl start redis

# 停止服务
systemctl stop redis

# 重启服务
systemctl restart redis

# 服务状态
systemctl status redis

# 开机启动
systemctl enable redis

# 重载服务配置
systemctl daemon-reload
```

- 查看安装位置

```shell
whereis redis
```

默认配置文件路径：

配置文件：/etc/redis.conf

数据文件：/var/lib/redis

日志文件：/var/log/redis

服务启动脚本：/usr/lib/systemd/system/redis.service

socket文件：/var/run/redis/

- 查看修改配置文件

```shell
vim /etc/redis.conf

# 启用守护进程
daemonize yes

# 开启远程连接
# bind 127.0.0.1

# 指定端口
port 6379

# 设置密码
requirepass 1111
```

- 卸载

```shell
# 停止服务
systemctl stop redis

# 删除安装的包
yum erase $(rpm -qa | grep redis)

# 删除配置文件
rm -rf /etc/redis.conf

# 删除数据文件
rm -rf /var/lib/redis

# 删除日志文件
rm -rf /var/log/redis
```

### 2、Ubuntu16.04

- 安装Redis服务

```shell
sudo apt-get install redis-server
```

- 服务管理

```shell
# 启动
sudo service redis start
# 停止
sudo service redis stop

# 服务状态
sudo service redis status
```

- 修改配置文件

```shell
sudo vim /etc/redis/redis.conf

# 开启远程连接
# bind 127.0.0.1

# 设置密码
requirepass 111111
```

- 测试服务

```shell
# 本地登录
redis-cli

# 远程登录
redis-cli -h ip -p port

# auth验证密码
auth passwo
```

- 查看守护进程运行状态

```shell
ps aux | grep redis
```

- 卸载

```shell
# 关闭守护进程redis
sudo service redis stop

# 卸载安装的软件包
sudo apt-get remove --purge redis-server*
```

### 3、Windows10

- 环境变量

```shell
# 安装目录
C:\PortableFiles\Redis-x64-5.0.10
```

- 注册服务

```shell
# 安装目录
redis-server --service-install redis.windows.conf --loglevel verbose
```

- 操作服务

```shell
# 启动服务
net start redis
# 停止服务
net stop redis
```
