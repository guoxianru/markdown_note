# Python：常见错误整理

## 导读

> 使用Python过程中遇到的问题梳理。

### 1、json.decoder.JSONDecodeError

- 问题描述

把json对象转换为字典返回，用单引号会报错。

```shell
json.decoder.JSONDecodeError: Expecting property name enclosed in double quotes: line 3 column 13 (char 23)
```

- 解决方法

把里面的单引号转换为双引号即可解决。
