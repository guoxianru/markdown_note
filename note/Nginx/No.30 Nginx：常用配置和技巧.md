# No.30 Nginx：常用配置和技巧

## 导读

> 列举了几个Nginx常见实用的配置。

### 1、一个站点配置多个域名

```shell
server {
    listen   80;
    server_name ops-coffee.cn b.ops-coffee.cn;
}
```

server_name 后跟多个域名即可，多个域名之间用空格分隔。

### 2、一个站点配置多个域名

```python
server {
    listen   80;
    server_name a.ops-coffee.cn;

    location / {
        root /home/project/pa;
        index index.html;
    }
}

server {
    listen       80;
    server_name  ops-coffee.cn b.ops-coffee.cn;

    location / {
        root /home/project/pb;
        index index.html;
    }
}

server {
    listen       80;
    server_name  c.ops-coffee.cn;

    location / {
        root /home/project/pc;
        index index.html;
    }
}
```

### 3、基于Nginx虚拟主机配置实现

Nginx有三种类型的虚拟主机

- 基于IP的虚拟主机： 需要你的服务器上有多个地址，每个站点对应不同的地址，这种方式使用的比较少。

- 基于端口的虚拟主机： 每个站点对应不同的端口，访问的时候使用ip:port的方式访问，可以修改listen的端口来使用。

- 基于域名的虚拟主机： 使用最广的方式，上边例子中就是用了基于域名的虚拟主机，前提条件是你有多个域名分别对应每个站点，server_name填写不同的域名即可。

### 4、nginx添加账号密码验证

```shell
server {
    location / {
        auth_basic "please input user&passwd";
        auth_basic_user_file key/auth.key;
    }
}
```

### 5、nginx开启列目录

当你想让nginx作为文件下载服务器存在时，需要开启nginx列目录

```shell
server {
    location download {
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }
}
```

autoindex_exact_size： 为on(默认)时显示文件的确切大小，单位是byte；改为off显示文件大概大小，单位KB或MB或GB。

autoindex_localtime： 为off(默认)时显示的文件时间为GMT时间；改为on后，显示的文件时间为服务器时间。

默认当访问列出的txt等文件时会在浏览器上显示文件的内容，如果你想让浏览器直接下载，加上下边的配置。

```shell
if ($request_filename ~* ^.*?\.(txt|pdf|jpg|png)$) {
    add_header Content-Disposition 'attachment';
}
```

### 6、配置默认站点

```shell
server {
    listen 80 default;
}
```

当一个nginx服务上创建了多个虚拟主机时默认会从上到下查找，如果匹配不到虚拟主机则会返回第一个虚拟主机的内容。

如果你想指定一个默认站点时，可以将这个站点的虚拟主机放在配置文件中第一个虚拟主机的位置，或者在这个站点的虚拟主机上配置listen default。

### 7、不允许通过IP访问

```shell
server {
    listen       80 default;
    server_name  _;
    return      404;
}
```

可能有一些未备案的域名或者你不希望的域名将服务器地址指向了你的服务器，这时候就会对你的站点造成一定的影响，需要禁止IP或未配置的域名访问，我们利用上边所说的default规则，将默认流量都转到404去。

上边这个方法比较粗暴，当然你也可以配置下所有未配置的地址访问时直接301重定向到你的网站去，也能为你的网站带来一定的流量。

```shell
server {
    rewrite ^/(.*)$ https://ops-coffee.cn/$1 permanent;
}
```

### 8、直接返回验证文件

```shell
location = /XDFyle6tNA.txt {
    default_type text/plain;
    return 200 'd6296a84657eb275c05c31b10924f6ea';
}
```

很多时候微信等程序都需要我们放一个txt的文件到项目里以验证项目归属，我们可以直接通过上边这种方式修改nginx即可，无需真正的把文件给放到服务器上。

### 9、nginx配置upstream反向代理

```shell
http {
    ...
    upstream tomcats {
        server 192.168.106.176 weight=1;
        server 192.168.106.177 weight=1;
    }
    server {
        location /ops-coffee/ {
            proxy_pass http://test;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

<proxy_pass <http://test>与<proxy_pass <http://test/>的区别，分为以下两种情况：

- 目标地址中不带uri，<proxy_pass <http://test>>，此时新的目标url中，匹配的uri部分不做修改，原来是什么就是什么。

```shell
location /ops-coffee/ {
    proxy_pass  http://192.168.106.135:8181;
}

# 结果
http://domain/ops-coffee/            -->     http://192.168.106.135:8181/ops-coffee/
http://domain/ops-coffee/action/abc  -->     http://192.168.106.135:8181/ops-coffee/action/abc
```

- 目标地址中带uri（“/”也是uri），<proxy_pass <http://test/>>，此时新的目标url中，匹配的uri部分将会被修改为该参数中的uri。

```shell
location /ops-coffee/ {
    proxy_pass  http://192.168.106.135:8181/;
}

# 结果
http://domain/ops-coffee/            -->     http://192.168.106.135:8181
http://domain/ops-coffee/action/abc  -->     http://192.168.106.135:8181/action/abc
```

### 10、nginx配置upstream开启keepalive

```shell
http {
    ...
    upstream tomcat {
        server ops-coffee.cn:8080;
        keepalive 1024;
    }
    server {
        location / {
            proxy_http_version 1.1;
            proxy_set_header Connection "";
            proxy_pass http://tomcat;
        }
    }
}
```

nginx在项目中大多数情况下会作为反向代理使用，例如nginx后接tomcat，nginx后接php等，这时我们开启nginx和后端服务之间的keepalive能够减少频繁创建TCP连接造成的资源消耗，配置如上。

keepalive： 指定每个nginxworker可以保持的最大连接数量为1024，默认不设置，即nginx作为client时keepalive未生效。

proxy_http_version 1.1： 开启keepalive要求HTTP协议版本为HTTP 1.1。

proxy_set_header Connection ""： 为了兼容老的协议以及防止http头中有Connection close导致的keepalive失效，这里需要及时清掉HTTP头部的Connection。

### 11、nginx设置gzip

```shell
# 开启gzip
gzip off;

# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k;

# gzip 压缩级别，1-9，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
gzip_comp_level 1;

# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;

# 配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
gzip_disable "MSIE [1-6]\.";

# 设置压缩所需要的缓冲区大小
gzip_buffers 32 4k;

# 设置gzip压缩针对的HTTP协议版本
gzip_http_version 1.0;

# 进行压缩的文件类型。javascript有多种形式。其中的值可以在mime.types文件中找到
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png application/vnd.ms-fontobject font/ttf font/opentype font/x-woff image/svg+xml;
```

### 12、nginx配置二级域名

```shell
# 代理池配置
server {
    listen 80;
    server_name proxypool.addcoder.com;
    location / {
        proxy_pass http://47.94.245.242:5010/;
        add_header Access-Control-Allow-Origin *;
    }
}
```

### 13、自定义404或502页面

当服务器出错时，访问域名会报404或502错误，如果想展示信息给用户的话，必须要自定义界面。

```shell
nginx配置：
server {
# 配置nginx502错误配置
error_page 502 /502.html;
location = /502.html {
root /usr/share/nginx/html;}
# 配置nginx404错误配置
error_page 404 /404.html;
location = /404.html {
root /usr/share/nginx/html;}
}
```

配置完再去 /usr/share/nginx/html 目录下添加自定义的 404.html 和 502.html 文件即可。

### 14、404自动跳转到首页

```shell
server {
    location / {
       error_page 404 =  @ops-coffee;
    }
    location @ops-coffee {
       rewrite  .*  / permanent;
    }
}
```

网站出现404页面不是特别友好，我们可以通过上边的配置在出现404之后给自动跳转到首页去。
