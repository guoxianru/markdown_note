# Docker：安装方法

## 导读

> Docker在不同平台下的安装方法。

### 1、CentOS7

- 安装前准备工作

卸载清理原有docker程序，如果之前没安装过docker可以不用执行。

```shell
yum remove docker docker-client docker-client-latest docker-common docker-latest docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
```

- 安装依赖软件，添加docker-ce安装源

yum-utils提供yum-config-manager包，用来管理yum配置文件。

lvm2和device-mapper-persistent-data为dockerdevicemapper存储设备的必须依赖。

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

- 添加docker-ce安装源

```shell
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

默认开启的是stable稳定版仓库，如果想要安装test测试版或者是边缘版本可使用如下命令开启相关模式，关闭的话只需要将--enable参数换成--disable。

```shell
yum-config-manager --enable docker-ce-edge
yum-config-manager --enable docker-ce-test
```

- 安装docker-CE

```shell
# 默认安装的是最新版本的稳定版
yum install -y docker-ce

# 如果要安装特定版本的docker-CE请使用如下命令格式
yum install -y docker-ce-<VERSION STRING>

# 查看版本列表请使用如下命令
yum list docker-ce --showduplicates | sort -r
```

- 常用命令

```shell
# 启动docker服务
systemctl start docker

# 重启docker服务
systemctl restart docker

# 查看docker状态
systemctl status docker

# 开机启动docker服务
systemctl enable docker
```

- 配置镜像加速器

```shell
# 编辑配置文件，没有请新建
vim /etc/docker/daemon.json

# 添加以下内容：
{
"registry-mirrors": ["https://bwzbq3db.mirror.aliyuncs.com"]
}

# 重载配置
systemctl daemon-reload

# 重启服务
systemctl restart docker
```

- 问题整理，报错“WARNING: IPv4 forwarding is disabled. Networking will not work”

```shell
# 原因：未开启ipv4转发，解决办法如下

# 编辑文件
vim /etc/sysctl.conf

# 配置转发
net.ipv4.ip_forward=1

# 重启服务
systemctl restart network

# 查看是否成功,如果返回为“net.ipv4.ip_forward = 1”则表示成功
sysctl net.ipv4.ip_forward
```

### 2、Ubuntu16.04

- 获取最新版本的 Docker 安装包，并且安装Docker及依赖包。

```shell
wget -qO- https://get.docker.com/ | sh
```

安装完成后有个提示：

```shell
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:
sudo usermod -aG docker runoob
Remember that you will have to log out and back in for this to take effect!
```

意思是，当要以非root用户可以直接运行docker时，需要执行以下命令，然后重新登陆，否则会报错。

```shell
sudo usermod -aG docker runoob
```

- 常用命令

```shell
# 启动docker服务
sudo service docker start

# 重启docker服务
sudo service docker restart
```

- 配置镜像加速器

```shell
# 编辑配置文件，没有请新建
vim /etc/docker/daemon.json

# 添加以下内容：
{
"registry-mirrors": ["https://bwzbq3db.mirror.aliyuncs.com"]
}

# 重启服务
sudo service docker restart
```
