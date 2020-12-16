# Docker：搭建Ubuntu-Python环境

## 导读

> 自定义Ubuntu--Python镜像。

### 1、拉取ubuntu镜像

```shell
docker image pull ubuntu
```

### 2、创建容器

```shell
docker run -itd --name ubuntu_python -p 9977:22 镜像ID /bin/bash
```

参数说明

- -itd为守护进程和交互模式；

- --name + 要给容器起的名字(此处本人起名为ubuntu_python)；

- -p +系统端口:容器端口;/bin/bash 为进入后的第一个命令。

### 3、进入容器

```shell
docker exec -it 容器ID /bin/bash
```

### 4、安装sudo命令

```shell
apt-get update
apt-get install sudo
激活root用户，设置root密码
sudo passwd root
```

### 5、安装python、virtualenv、vim

```shell
apt-get install python
apt-get install python3
apt-get install virtualenv
apt-get install virtualenvwrapper
apt-get install vim
```

### 6、配置虚拟环境

```shell
# 创建虚拟环境存放目录
mkdir ~/.virtualenvs

# 编辑环境配置文件
vi ~/.bashrc

# 添加如下
export WORKON_HOME=$HOME/.virtualenvs
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh

# 保存退出后执行如下
source .bashrc
```

注意:source行后面的地址是安装virtualenvwrapper的地址,如果忘记了使用如下命令查找,将查找到的地址替换刚刚的source行.

```shell
find / -name virtualenvwrapper.sh
```

### 7、创建虚拟环境,创建的虚拟环境默认的解释器为先安装的python版本

```shell
# 创建默认解释器的虚拟环境
mkvirtualenv py_2

# 创建指定版本解释器的虚拟环境
mkvirtualenv -p /usr/bin/python3 py_3
```

### 8、开启ssh服务用于远程连接

```shell
# 安装ssh服务
apt-get install openssh-server

# 启动ssh服务
service ssh start

# 查看ssh服务是否成功开启
ps -e |grep ssh

# 修改ssh配置文件
vi /etc/ssh/sshd_config

# 修改部分如下
PermitRootLogin yes
```

xshell使用本机ip;端口为第二步中设置的端口(此处为9977),使用root用户及之前设置的密码即可登录.

### 9、安装ftp服务

```shell
# 安装ftp服务
apt-get install vsftpd

# 启动ftp服务
service vsftpd start

# 查看ssh服务是否成功开启
ps -e | grep ftp

# 配置ftp文件,修改部分如下
# listen=NO
listen=YES

# 允许本地用户访问
local_enable=YES
#write_enable=YES
write_enable=YES

# 不允许匿名用户访问  
anonymous_enable=NO

# 重启ftp服务
service vsftpd restart
```

使用xshell自带的xftp进行连接,ip为本机ip;端口为第二步中设置的端口(此处为9977),使用root用户及之前设置的密码即可登录.

### 10、将当前容器保存为文件

```shell
# 将当前容器保存为镜像
docker commit 容器名 镜像名

# 将镜像保存为文件,镜像文件放在最下方
docker save -o 保存的文件名 镜像名

# 使用方法
docker load -i 镜像文件名
```

接着执行第2步的命令创建容器,默认设置的root密码为123456,进去容器后自行修改即可.

注意:链接ssh时ip为容器外面ifconfig得到的ip.
