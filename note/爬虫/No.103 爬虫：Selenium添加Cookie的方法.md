# No.103 爬虫：Selenium添加Cookie的方法

## 导读

> 详解selenium添加cookie的方法。

### 一、webdriver中常用的cookie方法

webdriver中提供了操作cookie的相关方法：

```python
# 获得cookie信息
get_cookies()

# 添加cookie
add_cookie(cookie_dict)

# 删除特定(部分)的cookie
delete_cookie(name)

# 删除所有的cookie
delete_all_cookies()

```

### 二、add_cookie()的用法

1.源码中的解释

源码中简略的向我们展示了如何添加cookie，源码如下：

```python
def add_cookie(self, cookie_dict):
    """
    Adds a cookie to your current session.

    :Args:
     - cookie_dict: A dictionary object, with required keys - "name" and "value";
        optional keys - "path", "domain", "secure", "expiry"

    Usage:
        driver.add_cookie({'name' : 'foo', 'value' : 'bar'})
        driver.add_cookie({'name' : 'foo', 'value' : 'bar', 'path' : '/'})
        driver.add_cookie({'name' : 'foo', 'value' : 'bar', 'path' : '/', 'secure':True})

    """
    self.execute(Command.ADD_COOKIE, {"cookie": cookie_dict})

```

从中可以看出add_cookie()这个函数有一个参数cookie_dict，它是以字典的形式传入的，字典中必选的键是"name"和"value"，可选的键是"path", "domin", "secure", "expiry"，其实源码中还漏了一个："httponly"。

2、cookie中键名的含义

```shell
name        cookie的名称
value       cookie对应的值，动态生成的
domain      服务器域名
expiry      Cookie有效终止日期
path        Path属性定义了Web服务器上哪些路径下的页面可获取服务器设置的Cookie
httpOnly    防脚本攻击
secure      在Cookie中标记该变量，表明浏览器和服务器之间的通信协议为加密认证协议。
```

### 三、实例

1.第一次测试

```python
from selenium import webdriver

driver = webdriver.Chrome()
cookies = {"value": "value", "name": "name"}
driver.add_cookie(cookie_dict=cookies)
driver.get("https://www.ketangpai.com/Main/index.html")

```

运行结果后发现报错了：Message: unable to set cookie。

解决方案：必须先加载网站，这样Selenium 才能知道cookie 属于哪个网站。

2、第二次测试

```python
from selenium import webdriver

driver = webdriver.Chrome()
cookies = {"value": "value", "name": "name"}
driver.get("https://www.ketangpai.com/User/login.html")
driver.add_cookie(cookie_dict=cookies)
driver.get("https://www.ketangpai.com/Main/index.html")

```

运行成功。
