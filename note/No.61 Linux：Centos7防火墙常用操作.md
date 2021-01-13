# No.61 Linux：Centos7防火墙常用操作

## 导读

> Centos7默认使用的是firewall作为防火墙。

### 一、防火墙服务

```shell
# 开启防火墙服务
systemctl start firewalld.service

# 关闭防火墙服务
systemctl stop firewalld.service

# 重启防火墙服务
systemctl restart firewalld.service

# 防火墙服务状态
systemctl status firewalld.service

# 禁止防火墙服务开机启动
systemctl disable firewalld.service
```

### 二、防火墙操作

- 查询有哪些端口是开启的

```shell
firewall-cmd --list-port
```

- 查询端口号80是否开启

```shell
firewall-cmd --query-port=80/tcp
```

- 开放端口

```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
# 命令含义：
--zone # 作用域
--add-port=80/tcp # 添加端口，格式为：端口/通讯协议
--permanent # 永久生效，没有此参数重启后失效
```

- 重载防火墙配置

```shell
firewall-cmd --reload
```

- 常用命令介绍

```shell
# 查看防火墙状态，是否是running
firewall-cmd --state

# 列出支持的zone
firewall-cmd --get-zones

# 列出支持的服务，在列表中的服务是放行的
firewall-cmd --get-services

# 查看ftp服务是否支持，返回yes或者no
firewall-cmd --query-service ftp

# 临时开放ftp服务
firewall-cmd --add-service=ftp

# 永久开放ftp服务
firewall-cmd --add-service=ftp --permanent

# 永久移除ftp服务
firewall-cmd --remove-service=ftp --permanent

# 永久添加80端口
firewall-cmd --add-port=80/tcp --permanent

# 查看规则，这个命令是和iptables的相同的
iptables -L -n

# 查看帮助
man firewall-cmd
firewall-cmd --help
```

### 三、使用iptables替换firewall

- 直接关闭防火墙

```shell
# 停止firewall
systemctl stop firewalld.service

# 禁止firewall开机启动
systemctl disable firewalld.service
```

- 安装 iptables service

```shell
yum -y install iptables-services
```

如果要修改防火墙配置，如增加防火墙端口3306

```shell
# 编辑配置文件
vim /etc/sysconfig/iptables

# 增加规则
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT


# 保存退出
```

- 开机启动iptables

```shell
# 重启防火墙使配置生效
systemctl restart iptables.service

# 设置防火墙开机启动
systemctl enable iptables.service
```

- 重启系统

```shell
reboot
```
