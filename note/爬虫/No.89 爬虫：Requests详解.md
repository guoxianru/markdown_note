# No.89 爬虫：Requests详解

## 导读

> requests是使用Apache2 licensed许可证的HTTP库，支持HTTP连接保持和连接池，支持使用cookie保持会话，支持文件上传，支持自动响应内容的编码，支持国际化的URL和POST数据自动编码，自动实现持久连接keep-alive。

### 1、基本介绍

- HTTP协议对资源的操作

![HTTP协议对资源的操作](../../pics/No.89%20爬虫：HTTP协议对资源的操作.png "HTTP协议对资源的操作")

- requests方法构造一个向服务器请求资源的Request对象，返回一个包含服务器资源的Response对象

|  方法  |  说明  |
|  ---  |  ---  |
|  requests.request()  |  构造一个请求，支撑一下各方法的基础方法  |
|  requests.get()  |  获取HTML网页的主要方法，对应于HTTP的GET  |
|  requests.head()  |  获取HTML网页头信息的方法，对应于HTTP的HEAD  |
|  requests.post()  |  向HTML网页提交POST请求的方法，对应于HTTP的POST  |
|  requests.put()  |  向HTML网页提交PUT请求的方法，对应于HTTP的PUT  |
|  requests.patch()  |  向HTML网页提交局部修改请求，对应于HTTP的PATCH  |
|  requests.delete()  |  向HTML页面提交删除请求，对应于HTTP的DELETE  |

- response对象的属性

|  属性  |  说明  |
|  ---  |  ---  |
|  response.status_code  |  HTTP请求的返回状态  |
|  response.text  |  HTTP响应内容的字符串形式  |
|  response.encoding  |  从HTTP header中猜测的响应内容编码方式(默认编码为ISO-8859-1)  |
|  response.apparent_encoding  |  从内容分析出的响应内容编码方式(备选编码方式)  |
|  response.content  |  HTTP响应内容的二进制形式  |

- requests库的异常

![requests库的异常](../../pics/No.89%20爬虫：requests库的异常.png "requests库的异常")

### 2、方法

```python
import requests

requests.get(url, params=None, **kwargs)

requests.post(url, data=None, json=None, **kwargs)

requests.head(url, **kwargs)

requests.delete(url, **kwargs)

requests.put(url, data=None, **kwargs)

requests.patch(url, data=None, **kwargs)

```

### 3、参数

```python
from requests.auth import HTTPProxyAuth

# 字典，HTTP请求头
headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.133 Safari/537.36"
}

# 字典或CookieJar，Request中的cookie
cookies = {"_ga": "GA1.2.311358392.1610634651"}

# 字典或字节序列，作为参数增加到url中
params = {"k1": "v1", "k2": "v2"}

# 字典、字节序列或文件对象，作为Request的对象
data = {"username": "admin", "pwd": "admin"}

# JSON格式的数据，作为Request的内容
json = {"username": "admin", "pwd": "admin"}

# 元组，支持HTTP认证功能
auth = HTTPProxyAuth("username", "passwd")

# 字典类型，传输文件
files = file = {"file": open("a.txt", "rb", encoding="utf-8")}

# 设定超时时间，秒为单位
timeout = 10

# 字典类型，设置访问代理服务器，可以增加登录认证
proxies = {"http": "http://61.24.25.21:5011/", "https": "http://61.24.25.21:5011/"}

# 默认为Ture，重定向开关
allow_redirects = True

# 默认为True，获取内容立即下载开关
stream = True

# 默认为True，认证SSL证书开关
verigy = True

# 本地SSL证书路径
cert = ("xxx/xxx/xxx/xxx/pem", "yyy/yyy/yyy.key")

```
