# Flask：部署Linux+uWSGI+Nginx

## 导读

> 整合众多文章得出的干货，部分步骤不详细的自行百度。

### 一、系统配置

- 更新系统及更换镜像源

- 转为root用户以获取权限

```shell
sudo su root
```

- 修改主机名

```shell
# 修改配置文件，将其对应的主机名修改为新的主机名
vim /etc/hostname

# 需要将 /etc/hosts 中添加新的主机名
vim /etc/hosts

127.0.0.1       localhost
127.0.0.1       aly
```

- 重启服务器  

```shell
shutdown -r now
```

### 二、所需软件安装

1. Python3.6.5及以上

2. MySQL

3. Nginx

4. Git

### 三、安装&创建虚拟环境

### 四、项目代码注意事项

- 公网访问配置

```python
# 打开生产环境
DEBUG = False
```

- 同步数据库

### 五、安装与配置uwsgi

nginx是门户,它负责转发,它转发动态请求给uwsgi,然后uwsgi在转给django处理。

- 安装uwsgi

```shell
pip install uwsgi
```

- 测试是否安装完成并且正常

```shell
uwsgi --version
```

- 配置uwsgi.ini文件

在项目文件夹与manage.py同级的目录下创建uwsgi.ini，文件内容如下（注意路径）：

```shell
# flaskuwsgi.ini 文件
[uwsgi]
# uwsgi监听的socket，一会儿配置Nginx会用到
socket = 127.0.0.1:5050
# 在app加载前切换到该目录，设置为Flask项目根目录
chdir = /srv/goodsmovie
# 加载指定的python WSGI模块，设置为Flask项目的manage文件
wsgi-file = ./manage.py
# 指定app对象实例
callable = app
# 启动一个master进程来管理其他进程
master = true
# 工作的进程数
processes = 4
# 每个进程下的线程数量
threads = 2
# 当服务器退出的时候自动删除unix socket文件和pid文件
vacuum = true
# 使进程在后台运行，并将日志打到指定的日志文件或者udp服务器
daemonize = /srv/goodsmovie/uwsgi.log
```

- 加载配置文件

```shell
uwsgi --ini flaskuwsgi.ini

# 出现getting INI configuration from uwsgi.ini（成功）
```

项目有更新的时候，需要先关闭uwsgi然后重启即可

```shell
sudo killall -9 uwsgi
```

- 基本命令

```shell
# 启动uwsgi服务器
uwsgi --ini flaskuwsgi.ini

# 查看uwsgi是否运行
ps -aux | grep uwsgi

# 查看端口号占用
netstat -anp | grep 7878

# 结束uwsgi进程
pgrep uwsgi | xargs kill -s 9
```

### 六、配置Nginx

配置nginx，若启动失败，测试配置文件是否正确

```shell
sudo nginx -t
```

- nginx配置文件分开配置

nginx.conf 文件尽量不做修改，只需在最末尾加载配置文件，然后在conf.d文件中放入不同的conf文件进行编辑配置。

```shell
include /etc/nginx/conf.d/*.conf
```

- 编辑配置文件

```shell
vim /etc/nginx/conf.d/goodsmovie.conf
```

- 示例

```shell
# Flask网站配置
server {
# 设置监听端口号
listen 80;
# 设置对外访问入口，可以是域名可以是公网IP
server_name goodsmovie.top www.goodsmovie.top;
# 设置访问的语言编码
charset UTF-8;
# 访问日志记录
access_log /var/log/nginx/goodsmovie_access.log;
# 错误日志记录
error_log /var/log/nginx/goodsmovie_error.log;
# 设置虚拟主机的基本信息
location / {
include uwsgi_params;
# 刚才uwsgi设置的socket
uwsgi_pass 127.0.0.1:5050;
uwsgi_read_timeout 2;}
# 静态文件设置，nginx自己处理
location /static  {
# 过期时间
expires 30d;
# 项目静态文件地址
alias /srv/goodsmovie/app/static;}
# 创建SSL证书临时文件
location /.well-known/acme-challenge {
alias /srv/goodsmovie/.well-known/acme-challenge;}
# 配置nginx502错误配置
error_page 502 /502.html;
location = /502.html {
root /usr/share/nginx/html;}
# 配置nginx404错误配置
error_page 404 /404.html;
location = /404.html {
root /usr/share/nginx/html;}
# 设置fastcgi缓冲区为8块128k大小的空间
fastcgi_buffers 8 128k;
# nginx的超时参数设置为60秒
send_timeout 60;
# 开启gzip
gzip on;
# 设置压缩所需要的缓冲区大小
gzip_buffers 32 4K;
# gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
gzip_comp_level 6;
# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 100;
# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到
gzip_types application/javascript text/css text/xml;
# 配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
gzip_disable "MSIE [1-6]\.";
# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;
}
```

### 七、启动服务器

```shell
# 切换到项目目录下运行
uwsgi --ini flaskuwsgi.ini

# 重启nginx服务
/etc/init.d/nginx restart
```

### 八、维护更新

项目有更新的时候，需要先关闭uwsgi然后重启即可。

- 示例

```shell
# 关闭uwsgi
killall -9 uwsgi

# 切换项目所在目录
cd /srv

# 删除项目文件夹
rm -rf goodsmovie/

# 重新拉取最新代码
git clone git@gitee.com:guoxianru/goodsmovie.git

# 切换项目根目录
cd goodsmovie/

# 切换虚拟环境
workon python36_flask

# 加载uwsgi配置
uwsgi --ini flaskuwsgi.ini

# 重启nginx服务
/etc/init.d/nginx restart
```
