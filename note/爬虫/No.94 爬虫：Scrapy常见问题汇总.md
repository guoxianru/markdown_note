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
