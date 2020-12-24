# PHP：CentOS7部署PHP5.4项目

## 导读

> 简单记录如何在centos7部署PHP项目。

### 一、CentOS7用Yum方式安装php-fpm

```shell
# PHP5.4
yum -y install php php-fpm php-gd php-mysql php-common php-pear php-mbstring php-mcrypt

# 启动服务
systemctl start php-fpm

# 停止服务
systemctl stop php-fpm

# 重启服务
systemctl restart php-fpm

# 服务状态
systemctl status php-fpm

# 开机启动
systemctl enable php-fpm
```

### 二、配置NGINX

```shell
server {
    listen      80;
    server_name 127.0.0.1;
    root        /www/wwwroot;
    index       index.php index.html index.htm;
    location    ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

### 三、重启NGINX，公网访问

```shell
systemctl restart nginx
```

通过 127.0.0.1:80 访问PHP网站。
