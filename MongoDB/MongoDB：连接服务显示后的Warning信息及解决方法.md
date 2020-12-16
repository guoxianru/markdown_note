# MongoDB：连接服务显示后的Warning信息及解决方法

## 导读

> 使用MongoDB时遇到过一些问题，建立数据库连接时出现了很多warnings。专门从网上搜索了一下，整理了以下内容，含解决办法。

### 第1种

```shell
Access control is not enabled for the database.
```

出现这个警告的原因是新版本的MongDB为了让我们创建一个安全的数据库必须要进行验证，也就是要操作数据库前要添加用户和密码，MongoDB更新后，不建议简单的建立连接了。

添加用户和密码的方法自行百度。

### 第2种

```shell
/sys/kernel/mm/transparent_hugepage/enabled is ‘always’.
/sys/kernel/mm/transparent_hugepage/defrag is ‘always’.
```

解决方法：

```shell
echo "never" > /sys/kernel/mm/transparent_hugepage/enabled
echo "never" >  /sys/kernel/mm/transparent_hugepage/defrag
```

### 第3种

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
