# No.29 Nginx：安装方法

## 导读

> NGINX在不同平台下的安装方法。

### 1、CentOS7

- 添加Nginx源

```shell
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

- 安装Nginx

```shell
yum install -y nginx
```

- 启动Nginx并设置开机自动运行

```shell
# 启动服务
systemctl start nginx

# 停止服务
systemctl stop nginx

# 重启服务
systemctl restart nginx

# 服务状态
systemctl status nginx

# 开机启动
systemctl enable nginx

# 重载服务配置
systemctl daemon-reload
```

- 查看安装位置

```shell
whereis  nginx
```

默认配置文件路径：

配置文件：/etc/nginx/

日志文件：/var/log/nginx

服务启动脚本：/usr/lib/systemd/system/nginx.service

- 查看修改配置文件

```shel
vim /etc/nginx/nginx.conf
```

- 卸载

```shell
# 停止服务
systemctl stop nginx

# 删除安装的包
yum erase $(rpm -qa | grep nginx)

# 删除配置文件
rm -rf /etc/nginx/

# 删除日志文件
rm -rf /var/log/nginx
```

### 2、Ubuntu16.04

- 安装

```shell
sudo apt-get install nginx
```

- 启动

```shell
/etc/init.d/nginx start
```
