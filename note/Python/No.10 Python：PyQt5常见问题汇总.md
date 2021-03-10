# No.10 Python：PyQt5常见问题汇总

## 导读

> 记录使用PyQt5时遇到的坑。

### 1、安装时依赖问题

PyQt5在Windows10下安装需要很多依赖库，但是这些依赖库又有版本限制，试了好几次，找到相对均衡的安装版本。

```shell
pip uninstall pyqt5 PyQt5-stubs pyqt5-tools
pip install pyqt5==5.13.0 PyQt5-stubs==5.13.0.1 pyqt5-tools==5.13.0.1.5
```

### 2、ImportError: unable to find Qt5Core.dll on PATH

- 问题报错

```shell
Traceback (most recent call last):
  File "demo.py", line 3, in <module>
  File "d:\python project\demo\venv\lib\site-packages\PyInstaller\loader\pyimod03_importers.py", line 627, in exec_module
    exec(bytecode, module.__dict__)
  File "lib\site-packages\PyQt5\__init__.py", line 41, in <module>
  File "lib\site-packages\PyQt5\__init__.py", line 33, in find_qt
ImportError: unable to find Qt5Core.dll on PATH
[11632] Failed to execute script demo
```

- 出现原因

PyQt5对系统变量的加载存在bug。

- 解决方法

在主程序中PyQt5库import之前就对系统变量进行手动设置。

```python
import sys, os

if hasattr(sys, "frozen"):
    os.environ["PATH"] = sys._MEIPASS + ";" + os.environ["PATH"]
from PyQt5 import QtCore, QtWidgets, QtGui
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *

```
