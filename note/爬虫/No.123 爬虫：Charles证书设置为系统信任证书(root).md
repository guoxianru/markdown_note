# No.123 爬虫：Charles证书设置为系统信任证书(root)

## 导读

> 将Charles证书设置为系统信任证书，前提条件是需要root手机。

### 1、使用MD5计算证书hash值

```shell
openssl x509 -subject_hash_old -in D:\Adownloads\charles-proxy-ssl-proxying-certificate.pem

# 得到密钥
3182384b
```

### 2、修改名字

```shell
cp charles-proxy-ssl-proxying-certificate.pem 3182384b.0
```

### 3、传进手机

```shell
adb push 3182384b.0 /sdcard/Download
```

### 4、导入系统证书目录(/system/etc/security/cacerts/)

RE文件管理器 [豌豆荚](https://www.wandoujia.com/apps/74465)

使用RE文件管理器(root权限)将证书放进系统证书目录。
