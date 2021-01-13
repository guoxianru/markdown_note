# No.74 Python：pip的基本使用

## 导读

> 会Python的前提是会pip。

### 1、pip安装包下载

进入[下载地址](https://pypi.python.org/pypi/pip)下载 .tar.gz压缩包。

### 2、安装pip

- centos7：

```shell
yum install -y pip
yum install -y pip3
```

- Ubuntu：

```shell
sudo apt-get install -y pip
sudo apt-get install -y pip3
```

- 压缩包安装

```shell
# 解压压缩包
tar -xzvf pip-1.5.4.tar.gz

# 进入解压文件
cd pip-1.5.4

# 安装
python setup.py install
```

### 3、升级pip

- Windows

```shell
python -m pip install --upgrade pip
```

- Linux

```shell
pip install --upgrade pip
```

### 4、pip安装第三方包

```shell
pip install 安装包名
```

### 5、pip查看是否已安装

```shell
pip show --files 安装包名

# 输出
Name:SomePackage                                # 包名
Version:1.0                                     # 版本号
Location:/my/env/lib/pythonx.x/site-packages    # 安装位置
Files:
../somepackage/__init__.py [...]                # 包含文件等等
```

### 6、pip检查哪些包需要更新

```shell
pip list --outdated
```

### 7、pip升级包

```shell
pip install --upgrade 要升级的包名
```

### 8、pip卸载包

```shell
pip uninstall 要卸载的包名
```

### 9、导出安装的库到list.txt

```shell
pip freeze > list.txt
```

### 10、导入list.txt中列出的库到系统

```shell
pip install -r list.txt
```

### 11、下载离线安装包

```shell
pip download -d 路径 -r requirments.txt
```

### 12、利用离线包安装，首先切换到离线包所在路径

```shell
pip install --no-index --find-links=路径 -r requirments.txt
```

### 13、更换pip镜像源

- Linux

在用户目录下创建一个命名为<.pip>的文件夹（如：~/.pip/pip.conf），在该文件夹下创建一个命名为<pip.conf>的文件，在该文件中写入以下内容：

```shell
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

- Windows

在用户目录下创建一个命名为“pip”的文件夹（如：C:\Users\用户名\pip\pip.ini），在该文件夹下创建一个命名为“pip.ini”的文件，在该文件中写入以下内容：

```shell
[global]
trusted-host=mirrors.aliyun.com
index-url=http://mirrors.aliyun.com/pypi/simple/
```

### 14、pip参数解释

```shell
pip --help

Usage:  pip<command>[options]
Commands:
install                   安装包.
uninstall                 卸载包.
freeze                    按着一定格式输出已安装包列表
list                      列出已安装包.
show                      显示包详细信息.
search                    搜索包，类似yum里的search.
wheel                     Buildwheelsfromyourrequirements.
zip                       不推荐.Zipindividualpackages.
unzip                     不推荐.Unzipindividualpackages.
bundle                    不推荐.Createpybundles.
help                      当前帮助.

GeneralOptions:
-h,--help                 显示帮助.
-v,--verbose              更多的输出，最多可以使用3次
-V,--version              现实版本信息然后退出.
-q,--quiet                最少的输出.
--log-file<path>          覆盖的方式记录verbose错误日志，默认文件：/root/.pip/pip.log
--log<path>               不覆盖记录verbose输出的日志.
--proxy<proxy>            Specifyaproxyintheform[user:passwd@]proxy.server:port.
--timeout<sec>            连接超时时间(默认15秒).
--exists-action<action>   Defaultactionwhenapathalreadyexists:(s)witch,(i)gnore,(w)ipe,(b)ackup.
--cert<path>              证书.
```
