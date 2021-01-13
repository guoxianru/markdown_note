# No.96 爬虫：Scrapy多个spider时指定pipeline

## 导读

> Scrapy存在多个爬虫的时候如何指定对应的管道呢？

### 1、在 pipeline 里判断爬虫

- settings.py

```python
ITEM_PIPELINES = {
    "xxxx.pipelines.MyPipeline": 300,
}

```

- OneSpider.py

```python
class OneSpider(scrapy.spiders.Spider):
    name = "one"

```

- TwoSpider.py

```python
class TwoSpider(scrapy.spiders.Spider):
    name = "two"

```

- pipelines.py

```python
class MyPipeline(object):
    def process_item(self, item, spider):

        if spider.name == "one":
            print("one")
        elif spider.name == "two":
            print("two")

        return item

```

### 2、在爬虫里设置 pipeline（scrapy版本必须是1.1以上）

- settings.py

```python
ITEM_PIPELINES = {
    "xxxx.pipelines.OneSpiderPipeline": 300,
    "xxxx.pipelines.TwoSpiderPipeline": 400,
}

```

- OneSpider.py

```python
class OneSpider(scrapy.Spider):
    name = "one"
    custom_settings = {
        "ITEM_PIPELINES": {"xxxx.pipelines.OneSpiderPipeline": 300},
    }
```

- TwoSpider.py

```python
class TwoSpider(scrapy.Spider):
    name = "two"
    custom_settings = {
        "ITEM_PIPELINES": {"xxxx.pipelines.TwoSpiderPipeline": 400},
    }

```

- pipelines.py

```python
class OneSpiderPipeline(object):
    def process_item(self, item, spider):
        print("one")

        return item


class TwoSpiderPipeline(object):
    def process_item(self, item, spider):
        print("two")

        return item

```
