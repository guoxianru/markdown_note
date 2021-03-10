# No.20 Git：安装方法

## 导读

> Git在不同平台下的安装方法。

### 1、CentOS7

- 安装

```shell
yum install -y git
```

- 配置

```shell
git config --global user.name "guoxianru"
git config --global user.email "admin@addcoder.com"
```

- 创建公钥

```shell
ssh-keygen -C 'admin@addcoder.com' -t rsa
cd ~/.ssh
vim id_rsa.pub
```

### 2、Ubuntu16.04

- 安装git

```shell
sudo apt-get install git
```
