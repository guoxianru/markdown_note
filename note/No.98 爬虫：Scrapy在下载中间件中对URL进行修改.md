# No.98 爬虫：Scrapy在下载中间件中对URL进行修改

## 导读

> 在scrapy中对请求URL进行处理。

- 问题描述：

用scrapy进行爬虫项目时，已进入URL队列的URL失效，需要进行替换。

- 解决方法

Scrapy可以在下载中间件中对URL进行修改。

request.url是传递到中间件的url，是只读属性，无法直接修改。

可以调用_set_url方法，为request对象赋予新的URL。

```python
def process_request(self, request, spider):
    old_url = request.url
    new_url = request.url.replace("str", "") + "str"
    request._set_url(new_url)

```
