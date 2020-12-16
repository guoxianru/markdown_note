# Python：安装方法

## 导读

> Python在不同平台下的安装方法。

### 1、CentOS7

- 安装Python3.6.6

```shell
# 准备编译环境
yum groupinstall -y 'Development Tools'
yum install -y zlib-devel bzip2-devel  openssl-devel ncurses-devel

# 下载Python3.6.6压缩包
wget --no-check-certificate https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz

# 创建安装目录
mkdir /usr/local/python3

# 解压
tar -zxvf Python-3.6.6.tgz

# 切换到解压后的根目录
cd Python-3.6.6/

# 编译安装
./configure --prefix=/usr/local/python3
make
make install
```

- 创建Python3链接

Linux里原来的python命令还是指向Python2，这里创建python3的软链接指向Python3，这样Python2和Python3就都可以用了。

```shell
ln -s /usr/local/python3/bin/python3.6 /usr/bin/python3
```

- 创建Pip3链接

保留pip指向Pip2，创建pip3的软链接指向Pip3。

```shell
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
```

- 问题

如果你遇到ModuleNotFoundError: No module named '_sqlite3'

```shell
yum install sqlite*
```

然后重新执行第五步。

### 2、Ubuntu16.04

- 安装Python3.6

```shell
# 添加源仓库
sudo add-apt-repository ppa:jonathonf/python-3.6

# 先更新，再安装
sudo apt-get update
sudo apt-get install python3.6

# 安装pip
sudo apt-get install python-pip
sudo apt-get install python3-pip

# 升级pip
pip install --upgrade pip
```

- 使用pip出现以下错误：

```python
Traceback (most recent call last):
    File “/usr/bin/pip3”, line 9, in from pip import mainsudo rm system.reg
```

解决办法：

```shell
修改 /usr/bin/pip 文件：
sudo vim /usr/bin/pip

# from pip import main
# 改为
from pip import __main__

# sys.exit(main())
# 改为
sys.exit(__main__._main())
```
