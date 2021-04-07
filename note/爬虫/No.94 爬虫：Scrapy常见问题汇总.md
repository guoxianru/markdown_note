# No.94 爬虫：Scrapy常见问题汇总

## 导读

> Scrapy使用出现的错误，记录一下。

### 1、TimeoutError报错解决方法

- 问题描述

```python
twisted.internet.error.TimeoutError: User timeout caused connection failure:
```

一般是在全局配置settings.py中设置了 DOWNLOAD_TIMEOUT，或用了代理IP等，就会出现这类报错。

- 解决方法为
  
在middleware中，捕获这个报错，并返回request，让他重新请求这个对象。

```python
from twisted.internet.error import TimeoutError


def process_exception(self, request, exception, spider):
    # Called when a download handler or a process_request()
    # (from other downloader middleware) raises an exception.

    # Must either:
    # - return None: continue processing this exception
    # - return a Response object: stops process_exception() chain
    # - return a Request object: stops process_exception() chain
    if isinstance(exception, TimeoutError):
        return request

```

### 2、scrapy里面item传递数据后数据不正确的问题

按照理想状态，最后的item数据每个都是唯一的，但是实际情况是最后的item很多数据都是重复和错乱的，完全不对。

查找原因后，发现是因为使用 Request 函数传递 item 时，使用的是浅复制（对象的字段值被复制时，字段引用的对象不会被复制）。

解决方法

```python
import copy

# meta = {"item": item}
meta = {"item": copy.deepcopy(item)}

```

按照这个方法，把几个Request里面的都改了，数据是正常了。

但是，还没完。

此时，如果打开Pipeline，再跑一遍查看数据，发现还是错乱的。

因为，最后yield item后，如果需要下载图片，Pipeline的get_media_requests方法需要参数item，通过item获取图片URL再去下载，如果是浅拷贝就有可能跟上面的情况一样。

解决方法

```python
import copy

# yield item
yield copy.deepcopy(item)

```
