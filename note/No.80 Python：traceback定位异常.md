# No.80 Python：traceback定位异常

## 导读

> Python定位异常的相关操作，traceback。

### 1、演示用法

```python
import sys
import traceback

# 异常栈输出
traceback.print_exception(*sys.exc_info())
# traceback.print_exception()简化版
traceback.print_exc()
# 异常栈以字符串的形式返回
traceback.format_exc()

```

### 2、定位异常

```python
import sys
import traceback

try:
    raise SyntaxError
except:
    # 异常栈输出
    traceback.print_exception(*sys.exc_info())
    # traceback.print_exception()简化版
    traceback.print_exc()
    # 异常栈以字符串的形式返回
    traceback.format_exc()

```
