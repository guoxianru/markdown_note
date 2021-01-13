# No.76 Python：PyQt5的安装依赖版本

## 导读

> PyQt5在Windows10下安装需要很多依赖库，但是这些依赖库又有版本限制，试了好几次，找到相对均衡的安装版本。

```shell
pip uninstall pyqt5 PyQt5-stubs pyqt5-tools
pip install pyqt5==5.13.0 PyQt5-stubs==5.13.0.1 pyqt5-tools==5.13.0.1.5
```
