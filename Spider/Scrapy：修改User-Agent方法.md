# Scrapy：修改User-Agent方法

## 导读

> 使用Scrapy写爬虫的时候，会莫名其妙的被目标网站拒绝，很大部分是浏览器请求头的原因。

### 1、默认请求头

```python
"User-Agent": "Scrapy/1.8.0 (+http://scrapy.org)"
```

### 2、修改请求头

- 全局设置

所有爬虫所有连接生效。

settings.py

```python
# Crawl responsibly by identifying yourself (and your website) on the user-agent
USER_AGENT = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.162 Safari/537.36"

```

- 爬虫设置

单个爬虫所有连接生效。

爬虫文件

```python
class MySpider(scrapy.Spider):
    name = "myspider"
    custom_settings = {
        "USER_AGENT": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.162 Safari/537.36",
    }

```

- 链接设置

单个请求的单个链接生效。

爬虫文件

```python
USER_AGENT = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.162 Safari/537.36"


class MySpider(scrapy.Spider):
    name = "myspider"
    start_urls = ("https://baidu.com",)

    def start_requests(self):
        for url in self.start_urls:
            yield scrapy.Request(url, headers={"User-Agent": USER_AGENT})

```

- 中间件设置

从整个项目中去修改请求头的设置规则，变化多端，不同的写法，可以配置出不同的设置方式。

settings.py

```python
"DOWNLOADER_MIDDLEWARES": {
        "myspider.middlewares.UserAgentMiddleware": 544,
    }

```

middlewares.py

```python
class UserAgentMiddleware(object):
    def process_request(self, request, spider):
        request.headers["User-Agent"] = ""

```

### 3、优先级

```python
headers > custom_settings > settings.py > Scrapy默认

```
