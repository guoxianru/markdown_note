# No.55 Django：部署成功项目升级为HTTPS

## 导读

> Django部署一条龙之HTTPS实现。

### 一、Let’s Encrypt免费证书

#### 1.预先工作

在创建证书之前，在项目根目录下(manage.py文件所在目录)创建文件夹：/.well-know，在改文件夹下创建文件：acme-challenge。

#### 2.配置Nginx

安装完Certbot 之后，需要简单配置Nginx以便Let's Encrypt能起作用。

编辑配置文件

```shell
sudo vim /etc/nginx/conf.d/addcoder.conf
```

示例

```shell
# Django网站配置
server {
# 设置监听端口号
listen 80;
# 设置对外访问入口，可以是域名可以是公网IP
server_name addcoder.com www.addcoder.com;
# 设置访问的语言编码
charset UTF-8;
# 访问日志记录
access_log /var/log/nginx/addcoder_access.log;
# 错误日志记录
error_log /var/log/nginx/addcoder_error.log;
# 设置虚拟主机的基本信息
location / {
include uwsgi_params;
# 刚才uwsgi设置的socket
uwsgi_pass 127.0.0.1:8868;
uwsgi_read_timeout 2;}
# 静态文件设置，nginx自己处理
location /static {
# 过期时间
expires 30d;
# 项目静态文件地址
alias /srv/addcoder/static/;}
# 创建SSL证书临时文件
location /.well-known/acme-challenge {
alias /srv/addcoder/.well-known/acme-challenge;}
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

#### 3.重启nginx，以便生效

```shell
# centos7
systemctl restart nginx

# Ubuntu16.04
/etc/init.d/nginx restart
```

#### 4.安装certbot

```shell
# centos7
yum install -y certbot

# Ubuntu16.04
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

#### 5.签发 SSL 证书

两种生成证书的方式

- andalone

certbot 会启动自带的 nginx（如果服务器上已经有nginx或apache运行，需要停止已有的nginx或apache）生成证书。

```shell
certbot certonly --standalone -d addcoder.com -d www.addcoder.com -d addcoder.cn -d www.addcoder.cn
```

- webroot(推荐)

```shell
certbot certonly --webroot -w /srv/addcoder -d addcoder.com -d www.addcoder.com -d addcoder.cn -d www.addcoder.cn
```

-w：指定项目绝对路径。

-d：指定生成证书域名，不可以直接写IP。

这条命令的输出类似于这样（有Congratulations）为成功。

证书存储于 /etc/letsencrypt/live/addcoder.com/fullchain.pem

#### 6.如果遇到 ImportError: No module named 'requests.packages.urllib3' 报错

- 原因

requests库版本问题

- 解决方法

```shell
pip install --upgrade --force-reinstall 'requests==2.6.0'
```

#### 7.生成前向安全性密钥

```shell
cd /etc/letsencrypt/archive/addcoder.com
openssl dhparam 2048 -out dhparam.pem
```

#### 8.其他证书

[腾讯云免费ssl证书](https://cloud.tencent.com/product/ssl)

[阿里云免费ssl证书](https://www.aliyun.com/product/cas?spm=5176.10695662.1171680.2.65712652pKnY59)

将下载的证书复制到Nginx安装目录下的cert目录(需要新建)，然后参照上述步骤。

### 二、将https配置进Nginx

#### 1.配置

```shell
# 将IP永久转移到域名
server {
listen 80;
listen 443;
server_name 47.241.190.116;
return 301 https://www.addcoder.com;
}

# Django网站配置
server {
# 设置监听端口
listen 80;
# 开启https、http2，默认端口
listen 443 ssl http2 default;
# 设置对外访问入口，可以是域名可以是公网IP
server_name addcoder.cn www.addcoder.cn addcoder.com www.addcoder.com;
# HTTP请求301永久跳转到HTTPS
if ($server_port = 80) {
return 301 https://$server_name$request_uri;}
# 允许网段
allow all;
# 指定证书文件
ssl_certificate /etc/letsencrypt/live/addcoder.com/fullchain.pem;
ssl_certificate_key /etc/letsencrypt/live/addcoder.com/privkey.pem;
# 前向安全性
ssl_dhparam /etc/letsencrypt/archive/addcoder.com/dhparam.pem;
# 使用ssl模块配置支持HTTPS访问
ssl_prefer_server_ciphers on;
ssl_session_timeout 5m;
# PCI DSS支付卡行业安全标准,禁用不安全的SSLv1 2 3,只使用TLS,PCI安全标准委员会规定开启TLS1.0将导致PCI DSS不合规,推荐配置.
ssl_protocols TLSv1.1 TLSv1.2;
# 需要配置符合PFS规范的加密套件,推荐配置.
ssl_ciphers    ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4:!DH:!DHE;
# HSTS（HTTP严格传输安全）的 max-age 需要大于15768000秒,推荐配置.
add_header Strict-Transport-Security "max-age=31536000";
# 设置访问的语言编码
charset UTF-8;
# 访问日志记录
access_log /var/log/nginx/addcoder_access.log;
# 错误日志记录
error_log /var/log/nginx/addcoder_error.log;
# 设置虚拟主机的基本信息
location / {
include uwsgi_params;
# 刚才uwsgi设置的socket
uwsgi_pass 127.0.0.1:8868;
uwsgi_read_timeout 2;}
# 静态文件设置，nginx自己处理
location /static {
# 过期时间
expires 30d;
# 项目静态文件地址
alias /srv/addcoder/static/;}
# 创建SSL证书临时文件
location /.well-known/acme-challenge {
alias /srv/addcoder/.well-known/acme-challenge;}
# 配置nginx502错误配置
error_page 502  /502.html;
location = /502.html {
root /usr/share/nginx/html;}
# 配置nginx404错误配置
error_page 404  /404.html;
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

#### 2.重启nginx，以便生效

```shell
# centos7
systemctl restart nginx

# Ubuntu16.04
/etc/init.d/nginx restart
```

### 三、Let’s Encrypt 续期

```shell
certbot renew
```

### 四、Let’s Encrypt 自动续期

Let’s Encrypt 的证书90天就过期了，所以，我们还需要设置自动化更新脚本。

```shell
# 编辑当前用户的定时作业
crontab -e

# 改变通知邮件目的地
MAILTO=820685755@qq.com

# 加入作业
00 10 * * 1 certbot renew --deploy-hook "systemctl restart nginx" >> /srv/log/certbot.log 2>&1
```

1. 这里是每周一的10点00分尝试更新证书，如果证书在30天内到期，则会更新证书，否则不会更新

2. --deploy-hook选项表示在更新成功以后才运行重载 nginx 的命令。

3. crontab 六个字段含义：

   - minute： 表示分钟，（整数 0 -59）。

   - hour：表示小时，可以是从0到23之间的任何整数。

   - day：表示日期，可以是从1到31之间的任何整数。

   - month：表示月份，可以是从1到12之间的任何整数。

   - week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。

   - command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

   - 如果字段使用 * 号，则为满足其他字段约束的每月都执行该命令。
