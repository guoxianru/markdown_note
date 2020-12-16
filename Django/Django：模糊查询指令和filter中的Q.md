# Django：模糊查询指令和filter中的Q

## 导读

> 主要介绍模糊查询使用方法与指令。

```python
# 格式如下所示
表名.objects.filter(要查的字段__指令= "过滤的内容")

```

介绍django model 的一些常用查询指令

```python
# 精确等于，like ‘aaa’
__exact

# 精确等于，忽略大小写 ilike ‘aaa’
__iexact

# 包含，like ‘%aaa%’
__contains

# 包含忽略大小写，ilike ‘%aaa%’
__icontains

# 大于
__gt

# 大于等于
__gte

# 小于
__lt

# 小于等于
__lte

# 存在于一个list范围内
__in

# 以…开头
__startswith

# 以…开头 忽略大小写
__istartswith

# 以…结尾
__endswith

# 以…结尾，忽略大小写
__iendswith

# 在…范围内
__range

# 日期字段的年份
__year

# 日期字段的月份
__month

# 日期字段的日
__day

# 与 __exact=None的区别
__isnull=True/False
__isnull=True
```

Django 中的Q ，将filter与or ，and，not联系起来

```python
obj = UserInfo.objects.filter(password="123456", id__gt=2).exclude(phone=188)

```

等价于下面

```python
obj = UserInfo.objects.filter(Q(password="123456") & Q(id__gt=2) & ~Q(phone=188))

```
