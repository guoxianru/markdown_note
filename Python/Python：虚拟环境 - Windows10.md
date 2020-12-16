# Python：虚拟环境 - Windows10

## 导读

> Windows10系统下Python虚拟环境的安装使用。

### 1、系统环境

Windows10

### 2、安装虚拟环境

- 升级pip

```shell
python -m pip install --upgrade pip
```

- 安装虚拟环境

```shell
pip install virtualenv==16.1
pip install virtualenvwrapper-win==1.2.5
```

virtualenvwrapper 是virtualenv的扩展管理包，可以将所有的虚拟环境整合在一个目录下。

### 3、配置虚拟环境

默认创建的虚拟环境的路径在 C:\Users\Administrator\Envs

### 4、虚拟环境操作

```shell
mkvirtualenv env_name
# env_name为你要创建的虚拟环境的名字，需要联网
```

- 创建指定python版本的虚拟环境

```shell
mkvirtualenv --python=python3安装路径  env_name
mkvirtualenv --python=C:\Users\Administrator\AppData\Local\Programs\Python\Python36\python.exe python36_
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
