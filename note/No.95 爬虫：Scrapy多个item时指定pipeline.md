# No.95 爬虫：Scrapy多个item时指定pipeline

## 导读

> Scrapy存在多个item的时候如何指定管道进行对应的操作呢？

有时，为了数据的干净清爽，我们可以定义多个item，不同的item存储不同的数据，避免数据污染。但是在pipeline对item进行操作的时候就要加上判断。

- items.py

```python
class OneItem(scrapy.Item):
    one = scrapy.Field()


class TwoItem(scrapy.Item):
    two = scrapy.Field()

```

- pipelines.py

```python
from xxx.items import OneItem, TwoItem


class MyPipeline(object):
    def process_item(self, item, spider):

        if isinstance(item, OneItem):
            print("one")
        elif isinstance(item, TwoItem):
            print("two")

        return item

```
