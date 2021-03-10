# No.92 爬虫：Requests添加Cookie的2种方式

## 导读

> 通过Cookie跳过登录，并用Session保持会话。

- 把cookie先写成字典形式，然后把字典转换为cookiejar

```python
# 注意：这个方法会替换掉原有的cookies
# 开启一个会话Session
s = requests.Session()
# 从chrome浏览器中取到的cookie值
cookie_dict = {
    "49BAC005-7D5B-4231-8CEA-16939BEACD67": "cktest001",
    "JSESSIONID": "F4FFF69B8XXXXXXC8DCB4C061C0",
    "JSESSIONIDSSO": "9D49C76FD6XXXXXF294242B44A",
}
# 把cookie值转换为cookiejar类型，然后传给Session
s.cookies = requests.utils.cookiejar_from_dict(
    cookie_dict, cookiejar=None, overwrite=True
)

```

- 追加cookies

```python
# 开启一个会话Session
s = requests.Session()
# 创建一个Cookie Jar对象
jar = requests.cookies.RequestsCookieJar()
# 向Cookie Jar对象中添加cookie值
jar.set("49BAC005-7D5B-4231-8CEA-1XXXXBEACD67", "cktXXXX001")
jar.set("JSESSIONID", "F4FFF69B8CXXXX80F0C8DCB4C061C0")
jar.set("JSESSIONIDSSO", "9D49C7XXXX448FDF5B0F294242B44A")
# 把cookies追加到Session中
s.cookies.update(jar)

```
