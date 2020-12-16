# Python：URL解析

## 导读

> 使用Python对URL进行解析，提取具体信息。

### 1、提取顶级域名信息

```shell
pip install tld
```

- 获取TLD名称作为字符串返回

```python
from tld import get_tld

get_tld("http://www.google.co.uk")
# 'co.uk'

# 在不存在的TLD上引发异常或以静默方式失败
get_tld("http://www.google.idontexist", fail_silently=True)
# None

```

- 获取TLD作为对象返回

```python
from tld import get_tld

res = get_tld("http://some.subdomain.google.co.uk", as_object=True)

res
# 'co.uk'

res.subdomain
# 'some.subdomain'

res.domain
# 'google'

res.tld
# 'co.uk'

res.fld
# 'google.co.uk'

res.parsed_url
# SplitResult(
#     scheme='http',
#     netloc='some.subdomain.google.co.uk',
#     path='',
#     query='',
#     fragment=''
# )

```

- 获取TLD名称返回

```python
from tld import get_tld, get_fld

# 忽略丢失的协议
get_tld("www.google.co.uk", fix_protocol=True)
# 'co.uk'

# 忽略丢失的协议
get_fld("www.google.co.uk", fix_protocol=True)
# 'google.co.uk'

```

- 获取TLD部件作为元组返回

```python
from tld import parse_tld

parse_tld('http://www.google.com')
# 'com', 'google', 'www'

```

- 获取第一级域名作为字符串返回

```python
from tld import get_fld

get_fld("http://www.google.co.uk")
# 'google.co.uk'

get_fld("http://www.google.idontexist", fail_silently=True)
# None

```

- 检查某个tld是否是有效的tld

```python
from tld import is_tld

is_tld('co.uk)
# True

is_tld('uk')
# True

is_tld('tld.doesnotexist')
# False

is_tld('www.google.com')
# False

```

- 更新TLD名称列表

```python
# update-tld-names

# or

from tld.utils import update_tld_names

update_tld_names()

```

### 2、对URL按照一定格式进行拆分

```python
from urllib.parse import urlparse

info = urlparse(
    "https://club.jd.com/comment/productPageComments.action?&productId=100000177748&score=0&sortType=5&page=1&pageSize=10&isShadowSku=0&fold=1"
)
print(info)

```

将url分成六个部分，返回一个包含6个字符串项目的元组：协议，位置，路径，参数，查询，判断。

```python
ParseResult(
    scheme="https",
    netloc="club.jd.com",
    path="/comment/productPageComments.action",
    params="",
    query="&score=0&sortType=5&page=1&pageSize=10",
    fragment="",
)

```

scheme是协议，netloc是域名服务器，path是路径，params是参数，query是查询，那么fragment是判断。
