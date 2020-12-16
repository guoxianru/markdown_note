# Python：虚拟环境 - Ubuntu16.04

## 导读

> Ubuntu16.04系统下Python虚拟环境的安装使用。

### 1、系统环境

Ubuntu16.04

### 2、安装虚拟环境

- 升级pip3

```shell
pip3 install --upgrade pip
```

- 安装虚拟环境

```shell
sudo pip install virtualenv
sudo pip install virtualenvwrapper
```

virtualenvwrapper 是virtualenv的扩展管理包，可以将所有的虚拟环境整合在一个目录下。

### 3、配置虚拟环境

- 创建虚拟环境管理目录

```shell
sudo mkdir ~/.envs
```

- 打开.bashrc

```shell
sudo vim ~/.bashrc
```

- 在.bashrc的末尾增加下面内容

```shell
export WORKON_HOME=$HOME/.envs  # 所有虚拟环境存储的目录
source /usr/local/bin/virtualenvwrapper.sh
```

- 使配置生效

```shell
source ~/.bashrc
```

- 更新配置文件报错：No module named virtualenvwrapper

解决方法，重新添加python有关的环境变量，先确认virtualenvwrapper依赖的Python版本

```shell
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python
export VIRTUALENVWRAPPER_VIRTUALENV=/usr/local/bin/virtualenv
```

### 4、虚拟环境操作

- 创建虚拟环境

```shell
mkvirtualenv env_name
# env_name为你要创建的虚拟环境的名字，需要联网
```

- 创建指定python版本的虚拟环境

```shell
mkvirtualenv -p /usr/bin/python3.6 python36_
mkvirtualenv -p /usr/bin/python2.7 python27_
```

- 查看安装的所有虚拟环境

```shell
workon
```

- 进入虚拟环境

```shell
workon env_nam
```

- 退出虚拟环境

```shell
deactivate
```

- 删除虚拟环境

```shell
rmvirtualenv env_nam
```
