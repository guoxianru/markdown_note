# No.115 Node：安装方法

## 导读

> Node在不同平台下的安装方法。

### 1、CentOS7

- 安装Node12.16.2

```shell
# 准备编译环境
yum -y install gcc gcc-c++ openssl-devel

# 切换目录
cd /usr/local

# 下载Node12.16.2压缩包
wget http://nodejs.org/dist/v12.16.2/node-v12.16.2-linux-x64.tar.gz

# 解压
tar -zxvf node-v12.16.2-linux-x64.tar.gz

# 重命名文件夹
mv node-v12.16.2-linux-x64 node
```

- 添加环境变量

```shell
# 编辑文件
vim ~/.bashrc

# 添加
export PATH=$PATH:/usr/local/node/bin

# 更新生效
source ~/.bashrc

# 验证环境
node -v
```

- 设置淘宝npm源

```shell
# 设置源
npm config set registry https://registry.npm.taobao.org
# 验证源
npm config get registry
# 替换npm，npm变为cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
