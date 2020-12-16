# MongoDB：安装方法

## 导读

> MongoDB在不同平台下的安装方法。

### 1、CentOS7

- 配置yum源

```shell
vim /etc/yum.repos.d/mongodb-org-4.0.repo

# 添加以下内容：
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/4.0/x86_64/
# 这里可以修改 gpgcheck=0, 省去gpg验证
gpgcheck=0
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
```

- 生成缓存

```shell
yum makecache
```

- 安装

```shell
yum -y install mongodb-org
```

- 启动服务并设置开机启动

```shell
# 命令行启动
/usr/bin/mongod --quiet --config /etc/mongod.conf

# 启动服务
systemctl start mongod

# 停止服务
systemctl stop mongod

# 重启服务
systemctl restart mongod

# 服务状态
systemctl status mongod

# 开机启动
systemctl enable mongod

# 重载服务配置
systemctl daemon-reload
```

- 查看安装位置

```shell
whereis mongod
```

默认配置文件路径：

配置文件：/etc/mongod.conf

数据文件：/var/lib/mongo

日志文件：/var/log/mongodb

服务启动脚本：/usr/lib/systemd/system/mongod.service

socket文件：/run/mongodb/mongod.pid

- 查看修改配置文件

```shell
vim /etc/mongod.conf

# 注释bindIp或者设置为0.0.0.0
```

- 卸载

```shell
# 停止服务
systemctl stop mongod

# 删除安装的包
yum erase $(rpm -qa | grep mongodb-org)

# 删除配置文件
rm -rf /etc/mongod.conf

# 删除数据文件
rm -rf /var/lib/mongo

# 删除日志文件
rm -rf /var/log/mongodb
```

### 2、Ubuntu16.04

- 导入包管理系统使用的公钥

```shell
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

- 为MongoDB创建一个列表文件

```shell
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

- 更新本地包数据库

```shell
sudo apt-get update
```

- 安装最新版本

```shell
sudo apt-get install mongodb-org
```

- 安装指定版本

```shell
sudo apt-get install -y mongodb-org=4.0.0 mongodb-org-server=4.0.0 mongodb-org-shell=4.0.0 mongodb-org-mongos=4.0.0 mongodb-org-tools=4.0.0
```

- 服务管理

```shell
# 启动
sudo service mongod start

# 停止
sudo service mongod stop

# 服务状态
sudo service mongod status

# 开机启动mongodb服务
sudo systemctl enable mongod
```

- 修改配置文件

```shell
sudo vim /etc/mongod.conf

# 数据库存储路径
dbPath: /var/lib/mongodb
# 以追加的方式写入日志
logAppend: true
# 日志文件路径
path: /var/log/mongodb/mongod.log
# 数据库端口
port: 27017
# 绑定监听的ip，127.0.0.1只能监听本地的连接，可以改为0.0.0.0
bindIp: 127.0.0.1
```

- 查看守护进程运行状态

```shell
ps aux | grep mongod
```

- 卸载

```shell
# 关闭守护进程mongod
sudo service mongod stop

# 卸载安装的软件包
sudo apt-get remove --purge mongodb-org*

# 数据库和日志文件的路径取决于/etc/mongod.conf文件中的配置
sudo rm -r /var/log/mongodb
sudo rm -r /var/lib/mongodb
```
