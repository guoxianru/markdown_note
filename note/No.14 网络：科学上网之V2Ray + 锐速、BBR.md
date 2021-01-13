# No.14 网络：科学上网之V2Ray + 锐速、BBR

## 导读

> 防火长城（英文名称Great Firewall of China，简写为Great Firewall，缩写GFW），也称中国防火墙或中国国家防火墙，指中华人民共和国政府在其管辖因特网内部建立的多套网络审查系统的总称，包括相关行政审查系统。

### 一、V2Ray + SS

#### 1、系统要求

CentOS 7、Ubuntu 16、Ubuntu 14、Debian 9

#### 2、切换root权限

```shell
sudo -i
```

#### 3、一键安装脚本

```shell
bash <(curl -s -L https://git.io/v2ray.sh)
```

#### 4、如果提示 curl: command not found，请安装 Curl

- Ubuntu/Debian系统安装Curl方法:

```shell
apt-get update -y && apt-get install curl -y
```

- CentOS系统安装Curl方法:

```shell
yum update -y && yum install curl -y
```

#### 5、安装步骤

1. 根据提示输入序号进行安装。

2. 协议很多，可以搜索了解下，也可以直接用默认的。有的需要有域名（websocket tls），有的就不用（tcp，mkcp）。没啥需求就用 TCP，追求更加安全就用 WS + TLS，ISP 多作用动态端口，VPS 网络不好就用 mKCP。

3. 端口号随意，为了避免冲突，推荐使用1000以上的端口号（不能超过65535）。

4. 广告拦截，会增加服务器负担，不建议开启。

5. 开启SS，按需使用。

6. SS端口号随意，不要和V2ray的端口号冲突。

7. SS连接密码，没有要求，越简单越好。

8. SS加密协议，按需选择。

9. 搭建完成，回车，回车。

#### 6、配置及管理

```shell
# V2Ray 配置文件路径
/etc/v2ray/config.json

# 查看 V2Ray 配置信息
v2ray info

# 修改 V2Ray 配置
v2ray config

# 生成 V2Ray 配置文件链接
v2ray link

# 生成 V2Ray 配置信息链接
v2ray infolink

# 生成 V2Ray 配置二维码链接
v2ray qr

# 修改 Shadowsocks 配置
v2ray ss

# 查看 Shadowsocks 配置信息
v2ray ssinfo

# 生成 Shadowsocks 配置二维码链接
v2ray ssqr

# 查看 V2Ray 运行状态
v2ray status

# 启动 V2Ray
v2ray start

# 停止 V2Ray
v2ray stop

# 重启 V2Ray
v2ray restart

# 查看 V2Ray 运行日志
v2ray log

# 更新 V2Ray
v2ray update

# 更新 V2Ray 管理脚本
v2ray update.sh

# 卸载 V2Ray
v2ray uninstall
```

### 二、BBR/锐速

#### 1、注意事项

- BBR是 Google 提出的一种新型拥塞控制算法,可以使 Linux 服务器显著地提高吞吐量和减少 TCP 连接的延迟。

- 安装锐速需要降级系统内核，而安装 Google BBR 则需要升级系统内核，故两者不能同时安装。

- 安装锐速需要降级系统内核，有可能造成系统不稳定，故不建议将其应用在重要的生产环境中。

- 本教程适用于 Centos 6+ / Debian 7+ / Ubuntu 14+ 系统，BBR魔改版不支持Debian 8。

#### 2、安装

- 下面是一个五合一的TCP网络加速脚本，其包括了BBR原版、BBR魔改版、暴力BBR魔改版、BBR plus、Lotsever(锐速)安装脚本。该脚本由94ish.me制作。

- 按照脚本菜单选项，选择对应安装的功能，来启用加速。

```shell
wget -N --no-check-certificate "https://gist.github.com/zeruns/a0ec603f20d1b86de6a774a8ba27588f/raw/4f9957ae23f5efb2bb7c57a198ae2cffebfb1c56/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```

#### 3、锐速管理命令

```shell
# 查看运行状态
/appex/bin/serverSpeeder.sh status
# 启动锐速
/appex/bin/serverSpeeder.sh start
# 停止锐速
/appex/bin/serverSpeeder.sh stop
# 重启锐速
/appex/bin/serverSpeeder.sh restart
# 卸载锐速
/appex/bin/serverSpeeder.sh uninstall
```
