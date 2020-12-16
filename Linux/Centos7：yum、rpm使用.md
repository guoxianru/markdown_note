# Centos7：yum、rpm使用

## 导读

> yum与rpm的区别：rpm适用于所有环境，而yum要搭建本地yum源才可以使用。yum是上层管理工具，自动解决依赖性，而rpm是底层管理工具。

### 1、yum 安装、卸载软件

- yum 简介

yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。

- 更换yum镜像源

```shell
# 备份源文件
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup

# 下载新的CentOS-Base.repo (阿里云)到/etc/yum.repos.d/
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

- 常用的 yum 命令

```shell
# 显示已经安装的软件包
yum list installed

# 查找可以安装的软件包 （以 tomcat 为例）
yum list tomcat

# 安装软件包（以 tomcat 为例）
yum install tomcat

# 卸载软件包（以 tomcat 为例）
yum remove tomcat

# 列出软件包的依赖（以 tomcat 为例）
yum deplist tomcat

# -y 自动应答yes
yum -y install tomcat

# info 显示软件包的描述信息和概要信息（以 tomcat 为例）
yum info tomcat

# 生成缓存
yum makecache

# 升级所有的软件包
yum -y update

# 升级某一个软件包（以 tomcat 为例）
yum update tomcat

# 检查可更新的程序
yum check-update

# 清理缓存
yum clean all
```

- yum 可视化图形界面 Yumex

yum Extender (简称 yumex ) , 是 yum 的图形化操作界面。可以通过 yumex 方便的查看软件包，安装、卸载软件包。对于对命令行不熟的人简直就是神器，管理软件包很方便。

```shell
yum install yumex
```

### 2、Rpm 彻底完全删除已安装软件

- 查询是否安装了软件

```shell
rpm -qa | grep -i 软件名
rpm -qa | grep 软件名
```

- 删除已安装的软件包

```shell
# 普通删除模式，根据第一步显示的软件包名，一个个删除
sudo rpm -e  -- 包名

# 强力删除模式，如果用上面命令删除时，提示有依赖的其他文件
# 则用该命令可以对其进行强力删除
sudo rpm -e --nodeps 包名
```
