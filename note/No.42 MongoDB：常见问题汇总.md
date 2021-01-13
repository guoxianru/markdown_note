# No.42 MongoDB：常见问题汇总

## 导读

> 使用MongoDB时遇到过一些问题，整理了以下内容，含解决办法。

### 1、建立数据库连接时出现了很多Warnings

- 第1种

```shell
Access control is not enabled for the database.
```

出现这个警告的原因是新版本的MongDB为了让我们创建一个安全的数据库必须要进行验证，也就是要操作数据库前要添加用户和密码，MongoDB更新后，不建议简单的建立连接了。

添加用户和密码的方法自行查找。

- 第2种

```shell
/sys/kernel/mm/transparent_hugepage/enabled is ‘always’.
/sys/kernel/mm/transparent_hugepage/defrag is ‘always’.
```

解决方法：

```shell
echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
echo "never" >  /sys/kernel/mm/transparent_hugepage/defrag
```

- 第3种

```shell
soft rlimits too low. rlimits set to 1024 processes, 65535 files. Number of processes should be at least 32767.5 : 0.5 times number of files.
```

解决方法：

```shell
vim /etc/security/limits.conf
# 添加一下几行：
mongod  soft  nofile  64000
mongod  hard  nofile  64000
mongod  soft  nproc  32000
mongod  hard  nproc  32000
# 重启mongod：
systemctl restart mongod
service mongod restart
```

### 2、Windows10安装成功后服务无法启动问题解决

- 配置mongodb的环境变量，然后添加到Path

- 使用管理员身份打开cmd，然后删除安装时默认创建的mongodb服务,注意这里的服务名要换成你本机的

```shell
sc delete MongoDB
```

- 重新创建服务

```shell
mongod.exe --dbpath "C:\Program Files\MongoDB\Server\4.0\data" --logpath "C:\Program Files\MongoDB\Server\4.0\log\db.log" --install --serviceName "mongodb" --logappend --directoryperdb
```

- 命令启动服务

```shell
net start mongodb
```
