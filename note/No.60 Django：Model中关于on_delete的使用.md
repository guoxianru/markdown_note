# No.60 Django：Model中关于on_delete的使用

## 导读

> 简单介绍on_delete使用。

### 1、常见的使用方式

设置为null

### 2、关于别的属性的介绍

- CASCADE：这就是默认的选项，级联删除，你无需显性指定它。

- PROTECT：保护模式，如果采用该选项，删除的时候，会抛出ProtectedError错误。

- SET_NULL：置空模式，删除的时候，外键字段被设置为空，前提就是blank=True, null=True，定义该字段的时候，允许为空。

- SET_DEFAULT：置默认值，删除的时候，外键字段设置为默认值，所以定义外键的时候注意加上一个默认值。

- SET()：自定义一个值，该值只能是对应的实体了

### 3、补充说明:关于SET()的使用

```python
# 官方案例
def get_sentinel_user():
    return get_user_model().objects.get_or_create(username="deleted")[0]


class MyModel(models.Model):
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL, on_delete=models.SET(get_sentinel_user),
    )

```
