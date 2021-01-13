# No.19 Python：Pyinstaller常见问题汇总

## 导读

> 记录使用Pyinstaller时遇到的坑。

### 1、ModuleNotFoundError: No module named 'distutils'

pyinstaller打包好后运行的时候提示:ModuleNotFoundError: No module named ‘distutils’。

- 出现原因

virtualenv版本不兼容，创建的虚拟环境问题。

- 解决方法

重新安装版本为16.1的virtualenv。

```shell
pip install virtualenv==16.1
```
