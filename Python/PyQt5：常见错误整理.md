# PyQt5：常见错误整理

## 导读

> 使用 PyQt5 时遇到的问题，记录下来。

### 1、安装遇到的坑

- 问题描述

PyQt5在Windows10下安装需要很多依赖库，但是这些依赖库又有版本限制，试了好几次，找到相对均衡的安装版本。

- 解决方法

```shell
pip uninstall pyqt5 PyQt5-stubs pyqt5-tools
pip install pyqt5==5.13.0 PyQt5-stubs==5.13.0.1 pyqt5-tools==5.13.0.1.5
```
