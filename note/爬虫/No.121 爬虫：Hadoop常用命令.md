# No.121 爬虫：Hadoop常用命令

## 导读

> 基于Linux操作系统上传下载文件到HDFS文件系统基本命令学习。

### 1、FS Shell

调用文件系统(FS)Shell命令应使用 bin/hadoop fs \<args>的形式。

所有的的FS shell命令使用URI路径作为参数。

URI格式是scheme://authority/path。

对HDFS文件系统，scheme是hdfs，对本地文件系统，scheme是file。

其中scheme和authority参数都是可选的，如果未加指定，就会使用配置中指定的默认scheme。

一个HDFS文件或目录比如/parent/child可以表示成hdfs://namenode:namenodeport/parent/child，或者更简单的/parent/child（假设你配置文件中的默认值是namenode:namenodeport）。

大多数FS Shell命令的行为和对应的Unix Shell命令类似，不同之处会在下面介绍各命令使用详情时指出。

出错信息会输出到stderr，其他信息输出到stdout。

### 2、常用命令

```shell
# 查看文件夹
hadoop fs -ls /

# 新建文件夹
hadoop fs -mkdir /test

# 上传本地文件
hadoop fs -put /本地路径/a.txt /test

# 上传本地文件夹
hadoop dfs -put /本地路径 /test

# 查看文件
hadoop fs -cat /test/a.txt

# 删除文件夹
hadoop fs -rm -rf /test

```
