# No.120 Python：四种执行系统命令的方法

## 导读

> Python中执行系统命令常见的几种方法。

### 1、os.system(cmd)

在子终端运行系统命令，可以获取命令执行后的返回信息以及执行返回的状态

执行后返回两行结果，第一行是结果， 第二行是执行状态信息

```python
import os

os.system("date")
# 2018年 4月 8日 星期日 19时29分13秒 CST
# 运行状态号，0表示正确
# 0

```

### 2、os.popen(cmd)

不仅执行命令而且返回执行后的信息对象(常用于需要获取执行命令后的返回信息)，是通过一个管道文件将结果返回

```python
import os

nowtime = os.popen("date")
print(nowtime.read())
# 2018年 4月 8日 星期日 19时30分35秒 CST

```

### 3、commands模块(被subprocess取代)

| 方法 | 说明 |
| --- | --- |
| getoutput | 获取执行命令后的返回信息 |
| getstatus | 获取执行命令的状态值(执行命令成功返回数值0，否则返回非0) |
| getstatusoutput | 获取执行命令的状态值以及返回信息 |

```python
import commonds

status, output = commands.getstatusoutput("date")
print(status)
print(output)
# 0
# 2018年 4月 8日 星期日 19时31分45秒 CST

```

### 4、subprocess模块

运用对线程的控制和监控，将返回的结果赋于一变量，便于程序的处理。

有丰富的参数可以进行配置，可供我们自定义的选项多，灵活性高。

```python
# -*- coding: utf-8 -*-
import subprocess

nowtime = subprocess.Popen(
    "date", shell=True, stdout=subprocess.PIPE, stderr=subprocess.STDOUT
)
print(nowtime.stdout.read())
# 2018年 4月 8日 星期日 19时32分41秒 CST

```
