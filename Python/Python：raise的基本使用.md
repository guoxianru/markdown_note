# Python：raise的基本使用

## 导读

> 当程序出现错误，python会自动引发异常，也可以通过raise显示地引发异常。一旦执行了raise语句，raise后面的语句将不能执行。

### 1、演示raise用法

```python
try:
    s = None
    if s is None:
        print("s 是空对象")
        # 如果引发NameError异常，后面的代码将不能执行
        raise NameError
    # 这句不会执行，但是后面的except还是会走到
    print(len(s))
except TypeError:
    print("空对象没有长度")

s = None
if s is None:
    raise NameError
# 如果不使用try......except这种形式，那么直接抛出异常，不会执行到这里
print("is here?")

```

### 2、触发异常

我们可以使用raise语句自己触发异常

raise语法格式如下：

```python
raise [Exception [, args [, traceback]]]
```

语句中 Exception 是异常的类型（例如，NameError）参数标准异常中任一种，args 是自已提供的异常参数。

最后一个参数是可选的（在实践中很少使用），如果存在，是跟踪异常对象。

### 3、实例

一个异常可以是一个字符串，类或对象。 Python的内核提供的异常，大多数都是实例化的类，这是一个类的实例的参数。

```python
def mye(level):
    if level < 1:
        raise Exception("Invalid level!")
        # 触发异常后，后面的代码就不会再执行


try:
    # 触发异常
    mye(0)
except Exception as err:
    print(1, err)
else:
    print(2)

```
