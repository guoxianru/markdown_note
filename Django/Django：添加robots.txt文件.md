# Django：添加robots.txt文件

## 导读

> 三种方法，按需使用

### 方法1：将 robots.txt 放到 templates 目录，修改 urls.py

```python
# urls.py
from django.views.generic import TemplateView

url(
    r"^robots\.txt$",
    TemplateView.as_view(template_name="robots.txt", content_type="text/plain"),
),

```

### 方法2：不需添加 robots.txt 文件，修改 urls.py

```python
# urls.py
from django.http import HttpResponse

url(
    r"^robots\.txt$",
    lambda r: HttpResponse(
        "User-agent: *\nDisallow: /admin", content_type="text/plain"
    ),
),

```

### 方法3：将 robots.txt 放到根目录，修改 nginx 配置

```shell
location  /robots.txt {
    alias  /根目录/robots.txt;
}
```
