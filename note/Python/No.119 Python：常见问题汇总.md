# No.119 Python：常见问题汇总

## 导读

> Python编程时遇到过一些问题，整理了以下内容，含解决办法。

### 1、SyntaxError: Non-ASCII character '\xe4' in file

文件中出现了中文，且没有编码声明，Python2将默认以ASCII作为标准编码，而Python2支持的ASCII码无中文。

解决方法：

必须在文件中第一行声明文件编码

```python
# -*- coding: utf-8 -*-

```

### 2、UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range

此问题常见于Python2环境中。

解决方法：

```python
import sys

# Python2.x
reload(sys)
sys.setdefaultencoding("utf8")

```

### 3、Python2写文件中文乱码

Python2中open方法是没有encoding这个参数的，如果像python3一样的写法会报异常：

```shell
TypeError: ‘encoding’ is an invalid keyword argument for this function
```

解决方法：

```python
# -*- coding: utf-8 -*-

import io

test_1 = "中文"
with io.open("test.txt", "w", encoding="utf-8") as f:
    f.write(unicode(test_1, "utf-8"))

with open("test.txt", "r") as f:
    test_2 = unicode(f.read(), "utf-8")
    print test_2

```

### 4、Mac上PyCharm运行多进程报错的解决方案

运行时报错运行时报错

```shell
may have been in progress in another thread when fork() was called. We cannot safely call it or ignore it in the fork() child process. Crashing instead. Set a breakpoint on objc_initializeAfterForkError to debug.
```

解决方案

添加环境变量:

点击窗口上的Run->Edit Configurations...->Environment variables->点击输入栏后的文件夹图标

添加内容：

key: OBJC_DISABLE_INITIALIZE_FORK_SAFETY, value: YES

完整示例：

OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
