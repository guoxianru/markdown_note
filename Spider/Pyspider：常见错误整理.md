# Pyspider：常见错误整理

## 导读

> 使用pyspider时遇到的问题，记录下来。

### 1、无法启动

- 因为pyspider是对pip有版本要求的，所以升级pip。

```shell
pip install –upgrade pip
```

- 一切配置好之后，就在CMD中运行命令来看能否跑起来。

```shell
 pyspider all
```

- python3.7不兼容pyspider问题(出现占用关键字的问题)

Python 3.5中引入了async和await，它们在Python 3.7中成为关键字。所以需要替换一下关键字。

```shell
分别在run.py、tornado_fetcher.py、webui>app.py，ctrl+f查找async替换掉就可以了。
```

- Deprecated option ‘domaincontroller’: use ‘domain_controller’ instead.

这是WsgiDAV发布了版本 pre-release 3.x导致的，所以只要把版本降下来就好了。将wsgidav替换为2.4.1。

```shell
python -m pip install wsgidav==2.4.1
```

然后运行 pyspider all 。打开浏览器输入：localhost：5000
