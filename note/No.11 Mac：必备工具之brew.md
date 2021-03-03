# No.11 Mac：必备工具之brew

## 导读

> brew是Mac下的一个包管理工具，类似于centos下的yum，可以很方便地进行安装/卸载/更新各种软件包。

### 1、安装 brew

```shell
# 默认安装脚本
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 国内加速版
/bin/bash -c "$(curl -fsSL https://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"
```

### 2、基本用法

以 nodejs 为例，执行下面命令即可，安装目录在 /usr/local/Cellar。

```shell
# 安装
brew install nodejs
如果需要指定版本，，在@后面指定版本号
brew install nodejs@0.9

# 更新
brew upgrade nodejs

# 卸载
brew remove nodejs

# 列出当前安装的软件
brew list

# 查询与 nodejs 相关的可用软件
brew search nodejs

# 查询 nodejs 的安装信息
brew info nodejs
```

### 3、brew services

brew services 是一个非常强大的工具，可以用来管理各种服务的启停，有点像 linux 里面的 services，非常方便。

```shell
# 启动
brew services start nodejs

# 停止
brew services stop nodejs

# 重启
brew services restart nodejs

# 列出当前的状态
brew services list

# brew services 配置路径：/usr/local/etc/
# brew services 日志路径：/usr/local/var/log
```
