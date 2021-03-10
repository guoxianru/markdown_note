# No.88 Python：raise抛出异常

## 导读

> Python抛出异常的相关操作，raise。

### 1、演示用法

```python
try:
    # 抛出异常，后面的代码将不能执行。
    raise NameError
except:
    print("抛出异常")

```

### 2、触发异常

我们可以使用raise语句自己触发异常

raise语法格式如下：

```python
raise [Exception [, args [, traceback]]]
```

语句中 Exception 是异常的类型（例如，NameError）参数标准异常中任一种，args 是自已提供的异常参数。

最后一个参数是可选的（在实践中很少使用），如果存在，是跟踪异常对象。

### 3、自定义异常

```python
class MyException(Exception):
    def __init__(self, msg):
        self.message = msg

    def __str__(self):
        return self.message


try:
    raise MyException("自定义异常")
except MyException as e:
    print(e)

```

### 4、Python标准异常

|  异常  |  描述  |
|  ---  |  ---  |
| BaseException  |  所有异常的基类  |
| SystemExit  |  解释器请求退出
| KeyboardInterrupt  |  用户中断执行(通常是输入^C)  |
| Exception  |  常规错误的基类  |
| StopIteration  |  迭代器没有更多的值  |
| GeneratorExit  |  生成器(generator)发生异常来通知退出  |
| StandardError  |  所有的内建标准异常的基类  |
| ArithmeticError  |  所有数值计算错误的基类  |
| FloatingPointError  |  浮点计算错误  |
| OverflowError  |  数值运算超出最大限制  |
| ZeroDivisionError  |  除(或取模)零 (所有数据类型)  |
| AssertionError  |  断言语句失败  |
| AttributeError  |  对象没有这个属性  |
| EOFError  |  没有内建输入,到达EOF标记  |
| EnvironmentError  |  操作系统错误的基类  |
| IOError  |  输入/输出操作失败  |
| OSError  |  操作系统错误  |
| WindowsErro  |  系统调用失败  |
| ImportError  |  导入模块/对象失败  |
| LookupError  |  无效数据查询的基类  |
| IndexError  |  序列中没有此索引(index)  |
| KeyError  |  映射中没有这个键  |
| MemoryError  |  内存溢出错误(对于Python解释器不是致命的)  |
| NameError  |  未声明/初始化对象(没有属性)  |
| UnboundLocalError  |  访问未初始化的本地变量  |
| ReferenceError  |  弱引用(Weak reference)试图访问已经垃圾回收了的对象  |
| RuntimeError  |  一般的运行时错误  |
| NotImplementedError  |  尚未实现的方法  |
| SyntaxError  |  Python语法错误  |
| IndentationError  |  缩进错误  |
| TabError  |  Tab 和空格混用  |
| SystemError  |  一般的解释器系统错误  |
| TypeError  |  对类型无效的操作  |
| ValueError  |  传入无效的参数  |
| UnicodeError  |  Unicode 相关的错误  |
| UnicodeDecodeError  |  Unicode解码时的错误  |
| UnicodeEncodeError  |  Unicode编码时的错误  |
| UnicodeTranslateError  |  Unicode转换时的错误  |
| Warning  |  警告的基类  |
| DeprecationWarning  |  关于被弃用的特征的警告  |
| FutureWarning  |  关于构造将来语义会有改变的警告  |
| OverflowWarning  |  旧的关于自动提升为长整型(long)的警告  |
| PendingDeprecationWarning  |  关于特性将会被废弃的警告  |
| RuntimeWarning  |  可疑的运行时行为(runtime behavior)的警告  |
| SyntaxWarning  |  可疑的语法的警告  |
| UserWarning  |  用户代码生成的警告  |
