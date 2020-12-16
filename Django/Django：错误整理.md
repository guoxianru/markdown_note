# Django：错误整理

## 导读

> 整合常见的Django报错及解决方法。

### 1、No module named 'django.core.urlresolvers'

最近从django1.9迁移到django2.0中出现一个意外的报错：

```python
from django.core.urlresolvers import reverse

# 报错
# No module named 'django.core.urlresolvers'

```

原因：django2.0 把原来的 django.core.urlresolvers 包更改为了 django.urls 包。

解决方法是：把导入的包都修改一下。

```python
# from django.core.urlresolvers import reverse
# 改为
from django.urls import reverse

```
