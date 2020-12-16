# Scrapy：pipeline管道的open_spider、close_spider

## 导读

> 设置scrapy爬虫开启和关闭时的动作。

pipelines.py

```python
class DemoPipeline(object):

    # 开启爬虫时执行，只执行一次
    def open_spider(self, spider):
        # 为spider对象动态添加属性，可以在spider模块中获取该属性值
        # spider.hello = "world"
        # 可以开启数据库等
        pass

    # 处理提取的数据(保存数据)
    def process_item(self, item, spider):
        pass

    # 关闭爬虫时执行，只执行一次。
    # 如果爬虫中间发生异常导致崩溃，close_spider可能也不会执行
    def close_spider(self, spider):
        # 可以关闭数据库等
        pass

```
