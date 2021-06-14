# No.32 MySQL：安装方法

## 导读

> MySQL在不同平台下的安装方法。

### 1、CentOS7

- 下载源安装包

```shell
wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm
```

- 安装源

```shell
yum localinstall -y mysql57-community-release-el7-8.noarch.rpm
```

- 安装MySQL

```shell
yum install -y mysql-community-server
```

- 启动服务并设置开机启动

```shell
# 启动服务
systemctl start mysqld

# 停止服务
systemctl stop mysqld

# 重启服务
systemctl restart mysqld

# 服务状态
systemctl status mysqld

# 开机启动
systemctl enable mysqld

# 重载服务配置
systemctl daemon-reload
```

- 修改root本地登录密码

```shell
# 查看mysql初始密码
grep 'temporary password' /var/log/mysqld.log

# 连接mysql
mysql -uroot -p密码

# 修改密码【注意：后面的分号一定要跟上】
ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';
# 或
set password for 'root'@'localhost'=password('密码');
```

- 设置简单密码

```shell
# 查看 mysql 初始的密码策略（已连接MySQL服务）
SHOW VARIABLES LIKE 'validate_password%';

# 设置密码策略为只验证密码长度（不验证特殊字符）
set global validate_password_policy=LOW;

# 可设置为4位的密码
set global validate_password_length=4;
```

- 查看安装位置

```shell
whereis mysqld
```

默认配置文件路径：

配置文件：/etc/my.cnf

数据文件：/var/lib/mysql

日志文件：/var/log/mysqld.log

服务启动脚本：/usr/lib/systemd/system/mysqld.service

socket文件：/var/run/mysqld/mysqld.pid

- 查看修改配置文件

```shell
vim /etc/my.cnf

# 配置默认编码为utf8，在[mysqld]下添加以下内容：
character_set_server=utf8
init_connect='SET NAMES utf8'
```

- 卸载

```shell
# 停止服务
systemctl stop mysqld

# 删除安装的包
yum erase $(rpm -qa | grep mysql-community-server)

# 删除配置文件
rm -rf /etc/my.cnf

# 删除数据文件
rm -rf /var/lib/mysql

# 删除日志文件
rm -rf /var/log/mysqld.log
```

### 2、Ubuntu16.04

- 安装mysql服务，默认5.7

```shell
sudo apt-get install mysql-server
```

- 启动服务并设置开机启动

```shell
# 启动
sudo service mysql start

# 停止
sudo service mysql stop

# 服务状态
sudo service mysql status
```

- 修改配置文件
  
```shell
sudo vim/etc/mysql/mysql.conf.d/mysqld.cnf
# 添加该句：
character-set-server=utf8

sudo vim /etc/mysql/conf.d/mysql.cnf
# 添加以下：
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
collation-server = utf8_general_ci
init-connect='SET NAMES utf8'
character-set-server = utf8

# 修改完成后，重启mysql
sudo service mysql restart
```

- 查看守护进程运行状态

```shell
ps aux | grep mysql
```

- 卸载

```shell
# 关闭守护进程mysql
sudo service mysql stop

# 卸载安装的软件包
sudo apt-get remove --purge mysql-server*
```

### 3、Windows10

- 环境变量

```shell
# 安装目录
C:\PortableFiles\mysql-5.7.33-winx64\bin
```

- 配置文件

```shell
[mysql]

default-character-set=utf8

[mysqld]

port = 3306

basedir=C:\PortableFiles\mysql-5.7.33-winx64

datadir=C:\PortableFiles\mysql-5.7.33-winx64\data

max_connections=1000

character-set-server=utf8

default-storage-engine=INNODB

```

- 安装MySQL

```shell
# 安装
mysqld install
# 初始化
mysqld --initialize
```

- 设置root密码

```shell
# 安全模式启动
mysqld --defaults-file="C:\PortableFiles\mysql-5.7.33-winx64\my.ini" --console --skip-grant-tables
# 另起终端
mysql -uroot -p
# 直接敲Enter键，成功进入mysql

```

```sql
use mysql;

update user set authentication_string=password("1111") where user="root";

quit
-- 最后关闭安全模式启动的服务
```

- 设置root权限

重启MySQL服务

```shell
# 重启服务
net start mysql

mysql -uroot -p1111
```

```sql
-- 登录后，需要在正常模式下，再设一次root用户的密码
set password=password('1111');

use mysql;

select host,user from user;

update user set host='%' where user='root';

quit
```

- 操作服务

```shell
# 启动服务
net start mysql
# 停止服务
net stop mysql
```
