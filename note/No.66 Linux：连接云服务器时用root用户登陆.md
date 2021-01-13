# No.66 Linux：连接云服务器时用root用户登陆

## 导读

> 有些云服务器默认不允许root用户登录（比如谷歌云），需要修改SSH配置。

- 切换到root用户

```shell
sudo -i
```

- 修改SSH配置文件

```shell
vi /etc/ssh/sshd_config

# 默认为no，需要root用户登陆改为yes
PermitRootLogin yes

# 默认为no，需要root用户密码登陆改为yes
PasswordAuthentication yes
```

- 给root用户设置密码

```shell
passwd root
# 输入两遍新密码
```

- 重启SSH服务使修改生效

```shell
# Centos7
systemctl restart sshd.service

# Ubuntu
sudo service ssh restart
```

- SSH服务基本操作

```shell
# CentOS
# 查看服务
systemctl status sshd.service
# 启动服务
systemctl start sshd.service
# 关闭服务
systemctl stop sshd.service
# 重启服务
systemctl restart sshd.service
# 开机自启
systemctl enable sshd.service

# Ubuntu
# 查看服务
sudo service ssh status
# 启动服务
sudo service ssh start
# 关闭服务
sudo service ssh stop
# 重启服务
sudo service ssh restart
```
