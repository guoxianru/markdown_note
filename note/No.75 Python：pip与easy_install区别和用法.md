# No.75 Python：pip与easy_install区别和用法

## 导读

> 在pip不方便的时候考虑使用easy_install。

### 1、区别

easy_insall的作用和perl中的cpan，ruby中的gem类似，都提供了在线一键安装模块的傻瓜方便方式，而pip是easy_install的改进版，提供更好的提示信息，删除package等功能。老版本的python中只有easy_install，没有pip。

### 2、easy_install的用法

```shell
# 安装一个包
easy_install <package_name>
easy_install "<package_name>==<version>"

# 升级一个包
easy_install -U "<package_name>>=<version>"
```

### 3、pip的用法

```shell
# 安装一个包
pip install <package_name>
pip install <package_name>==<version>

# 升级一个包 (如果不提供version号，升级到最新版本）
pip install --upgrade <package_name>>=<version>

# 删除一个包
pip uninstall <package_name>
```
