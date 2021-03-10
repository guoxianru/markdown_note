# No.78 Python：try捕获异常

## 导读

> Python捕获异常的相关操作，try/except。

### 1、演示用法

```python
try:
    # 主代码块
    pass
except KeyError as e:
    # 异常时，执行该块
    pass
else:
    # 主代码块执行完，执行该块
    pass
finally:
    # 无论异常与否，最终执行该块
    pass

```

### 2、捕获异常

- 单个异常

```python
try:
    int("error")
except Exception as e:
    print(e)

```

- 多个异常

```python
try:
    int("error")
except IndexError as e:
    print(e)
except KeyError as e:
    print(e)
except ValueError as e:
    print(e)

```

- 先定义特殊提醒的异常，最后定义Exception，来确保程序正常运行并捕获所有异常

```python
try:
    int("error"
except KeyError as e:
    print(e)
except Exception as e:
    print(e)

```
