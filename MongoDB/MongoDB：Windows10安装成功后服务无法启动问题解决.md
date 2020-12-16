# MongoDB：Windows10安装成功后服务无法启动问题解决

## 导读

> 遇到这个问题的小伙伴来看下，具体的解决方法。

### 1、配置mongodb的环境变量，然后添加到Path

### 2、使用管理员身份打开cmd，然后删除安装时默认创建的mongodb服务,注意这里的服务名要换成你本机的

```shell
sc delete MongoDB
```

### 3、重新创建服务

```shell
mongod.exe --dbpath "C:\Program Files\MongoDB\Server\4.0\data" --logpath "C:\Program Files\MongoDB\Server\4.0\log\db.log" --install --serviceName "mongodb" --logappend --directoryperdb
```

### 4、命令启动服务

```shell
net start mongodb
```
