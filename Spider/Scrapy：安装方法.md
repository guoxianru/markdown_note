# Scrapy：安装方法

## 导读

> 总结scrapy在不同平台的安装方法。

### 1、Ubuntu 16.04

1.安装Scrapy依赖库

```shell
sudo apt-get install python3.6-dev
sudo apt-get install libevent-dev
sudo apt-get install libssl-dev
```

2.scarpy需求lxml，OpenSSL，Twisted库一般系统自带，也可用以下方法安装：

```shell
sudo pip install lxml
sudo pip install pyOpenSSL
sudo pip install Twisted
```

3.安装Scrapy

```shell
sudo pip install scrapy
```

### 2、Windows

1.更新pip

```shell
python -m pip install --upgrade pip
```

2.安装wheel

```shell
pip install wheel
```

3.安装lxml

```shell
pip intsall lxml
```

4.安装Twisted

[下载地址](http://www.lfd.uci.edu/~gohlke/pythonlibs/#twisted)

- 下载Twisted-18.9.0-cp36-cp36m-win_amd64.whl(对应python和软件版本)

- 进入文件所在目录

- 安装

```shell
pip install Twisted-18.9.0-cp36-cp36m-win_amd64.whl
```

5.安装pywin32

方法一、离线包

[下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32)

- 下载pywin32-224-cp36-cp36m-win_amd64.whl(对应python和软件版本)

- 进入文件所在目录

- 安装

```shell
pip install pywin32-224-cp36-cp36m-win_amd64.whl
```

方法二、在线

通过PIP指向此软件包pypiwin32，从PYPI安装pywin32

```shell
python -m pip install pypiwin32
# python -m意思是将库中的python模块用作脚本去运行
```

6.安装scrapy

```shell
pip install scrapy
```
