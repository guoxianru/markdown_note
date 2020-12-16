# Python：base64图像数据处理

## 导读

> 有时候会遇到图片的URL是data:image/png;base64，直接发起请问没有作用，其实这是一种网页优化的手段，需要其他方法解决。

### 一、介绍

1. data URI scheme 是一种网页优化的手段。直接把图像的内容崁入网页里面，减少页面的请求。

2. 浏览器并不会缓存这样的图片。

3. data URI scheme 虽然节省 HTTP 请求，但是如果这个图像要在网页多个地方显示的话，便会加大网页的内容，延长了下载的时间。

4. 其中一个解决办法是在一个 CSS class 中加入 data URL，在需要显示图像的区块调用这个 class。

### 二、解决方法

Python 代码实现：src转化为图片。

```python
import base64

src = "data:image/gif;base64,R0lGOD......"
data = src.split(",")[1]
image_data = base64.b64decode(data)
with open("1.gif", "wb") as f:
    f.write(image_data)

```

注意：image/后是图片格式，保存图片时格式要一致。
