# Python：super的基本使用

## 导读

> super 函数是用于调用父类(超类)的一个方法。

super主要来调用父类方法来显示调用父类，在子类中，一般会定义与父类相同的属性（数据属性，方法），从而来实现子类特有的行为。也就是说，子类会继承父类的所有的属性和方法，子类也可以覆盖父类同名的属性和方法。

```python
class Parent(object):
    Value = "Hi, Parent value"

    def fun(self):
        print("This is from Parent")


# 定义子类，继承父类
class Child(Parent):
    Value = "Hi, Child  value"

    def ffun(self):
        print("This is from Child")


c = Child()
c.fun()
c.ffun()
print(Child.Value)

# 输出结果
# This is from Parent
# This is from Child
# Hi, Child value

```

但是，有时候可能需要在子类中访问父类的一些属性，可以通过父类名直接访问父类的属性，当调用父类的方法是，需要将”self”显示的传递进去的方式。

```python
class Parent(object):
    Value = "Hi, Parent value"

    def fun(self):
        print("This is from Parent")


class Child(Parent):
    Value = "Hi, Child  value"

    def fun(self):
        print("This is from Child")
        # 调用父类Parent的fun函数方法
        Parent.fun(self)


c = Child()
c.fun()

# 输出结果
# This is from Child
# This is from Parent
# 实例化子类Child的fun函数时，首先会打印上条的语句，再次调用父类的fun函数方法

```

这种方式有一个不好的地方就是，需要经父类名硬编码到子类中，为了解决这个问题，可以使用Python中的super关键字。

```python
class Parent(object):
    Value = "Hi, Parent value"

    def fun(self):
        print("This is from Parent")


class Child(Parent):
    Value = "Hi, Child  value"

    def fun(self):
        print("This is from Child")
        # Parent.fun(self)
        # 相当于用super的方法与上一调用父类的语句置换
        super(Child, self).fun()


c = Child()
c.fun()

# 输出结果
# This is from Child
# This is from Parent
# 实例化子类Child的fun函数时，首先会打印上条的语句，再次调用父类的fun函数方法

```
