# No.47 Redis：数据导出与导入

## 导读

> redis-dump工具用于Redis数据库的数据导出与导入。

- 安装redis-dump

```shell
# 安装Ruby环境
yum -y install ruby ruby-devel install rubygems
# 更改gem源
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
# 查看现有源
gem sources -l
# 安装curl
yum install -y curl
# 添加RVM官网地址
vim /etc/hosts
# 添加
199.232.28.133 raw.githubusercontent.com
# 导入公钥
gpg2 --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
# 安装RVM
curl -L get.rvm.io | bash -s stable
# 添加环境变量
source /usr/local/rvm/scripts/rvm
# 查看rvm库中已知的ruby版本
rvm list known
# 安装一个ruby版本
rvm install 2.6.3
# 使用一个ruby版本
rvm use 2.6.3
# 设置默认版本
ruby --version
# 卸载一个已知版本
rvm remove 2.0.0
# 安装redis-dump
gem install redis-dump
```

- 导出

```shell
redis-dump -u :password@127.0.0.1:6379 > filename.json
```

- 导入

```shell
cat filename.json | redis-load -u :password@127.0.0.1:6379
```
