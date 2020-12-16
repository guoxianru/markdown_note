# Django：DEBUG=False

## 导读

> 解决生产环境下静态文件的访问问题。

Django关闭DEBUG模式后，就相当于是生产环境了，Django官网上指出如果是django框架一旦作为生产环境，那么它的静态文件访问接口就不应该从Django框架中走了，应该有独立的web环境，首推nginx 。

在开发过程中，开发人员在框架的根目录下创建一个static目录，目录在根据里面有几个APP创建对应APP程序静态文件目录。但是一旦放到生产环境（也就是关闭掉DEBUG模式），你在nginx中就要单独做访问/static/目录的路由。

- STATICFILES_DIRS

列表中的目录是开发时创建的静态目录。

```python
STATIC_URL = "/static/"
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static")]

```

- MEDIA_URL

自定义文件上传路径。

```python
MEDIA_URL = "/media/"
MEDIA_DIRS = [os.path.join(BASE_DIR, "media")]
MEDIA_ROOT = os.path.join(BASE_DIR, "media")

```

- STATIC_ROOT

Django框架放到生产环境中的唯一的一个静态目录。

```python
PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))
STATIC_ROOT = os.path.join(PROJECT_ROOT, "../statics")

```

- 将所有静态文件统一收集到STATIC_ROOT目录。

```shell
python manage.py collectstatic
```

DEBUG=False时，就必须部署nginx或者其他web服务器来提供静态访问入口。

```shell
server {
    location /static {
    # 项目静态文件地址
    alias /静态目录/;}
}
```
