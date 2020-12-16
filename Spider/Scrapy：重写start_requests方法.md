# Scrapy：重写start_requests方法

## 导读

> scrapy的start_requests方法重写，添加更多操作。

有时scrapy默认的start_requests无法满足我们的需求，例如分页爬取，那就要对它进行重写，添加更多操作。

```python
def start_requests(self):

    # 自定义功能

    yield scrapy.Request(url="http://test.com", method="GET", callback=self.parse)


def parse(self, response):
    print(response.url)

```
