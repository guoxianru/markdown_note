# 科学上网：搭建Python版本Shadowsocks

## 导读

> 防火长城（英文名称Great Firewall of China，简写为Great Firewall，缩写GFW），也称中国防火墙或中国国家防火墙，指中华人民共和国政府在其管辖因特网内部建立的多套网络审查系统的总称，包括相关行政审查系统。

### 1、安装与卸载

```shell
# 下载
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh
# 赋予执行权限
chmod +x shadowsocks.sh
# 安装
./shadowsocks.sh 2>&1 | tee shadowsocks-all.log
# 卸载
./shadowsocks.sh uninstall
```

### 2、安装过程

执行之后会提示输入：语言、密码、端口、加密方式。。。

最后出现以下信息成功。

成功后从服务器控制台开放端口。

```shell
Congratulations, your_shadowsocks_version install completed!
Your Server IP :192.168.11.11
Your Server Port :8080
Your Password :1234567890
Your Encryption Method:aes-256-cfb
Welcome to visit:https://teddysun.com/486.html
Enjoy it
```

### 3、命令操作

```shell
# 启动
/etc/init.d/shadowsocks start
# 停止
/etc/init.d/shadowsocks stop
# 重启
/etc/init.d/shadowsocks restart
# 状态
/etc/init.d/shadowsocks status
```

### 4、配置文件

```shell
# 修改配置文件，修改完毕重启服务即可。
vim /etc/shadowsocks.json

# 单端口
{
    # 服务器IP，直接用0.0.0.0也可
    "server":"0.0.0.0",
    # 对外端口
    "server_port":8888,
    # 本地地址，可省略
    "local_address": "127.0.0.1",
    # 本地端口，可省略
    "local_port":1080,
    # 密码
    "password":"password",
    # 超时时间，可省略
    "timeout":300,
    # 加密策略，有多重策略，具体自查
    "method":"aes-256-cfb"，
    # 降低延迟，建议Linux内核在3.7+
    "fast_open":false
}

# 多端口
{
    "server":"0.0.0.0",
    "server_port":8888,
    "local_address":"127.0.0.1",
    "local_port":1080,
    # 每个端口对应一个密码
    "port_password":{
        "1111":"password1",
        "1112":"password2",
        "1113":"password3"
    },
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}
```

### 5、客户端

[下载地址](https://www.softpedia.com/get/Internet/Servers/Proxy-Servers/Shadowsocks.shtml)

下载安装客户端以后，只需按服务器的配置填写IP地址、服务器端口、本地端口、密码、加密方式等参数。

### 5、PAC代理

客户端支持全局代理和 PAC 代理两种方式，推荐谷歌插件switchyOmega。
