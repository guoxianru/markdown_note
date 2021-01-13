# No.50 Redis：数据备份与恢复

## 导读

> Redis数据库的备份与恢复非常简单与方便。

### 1、备份

Redis SAVE 命令用于创建当前数据库的备份。

```sql
redis 127.0.0.1:6379> SAVE
OK
```

该命令将在 redis 安装目录中创建dump.rdb文件。

### 2、恢复

如果需要恢复数据，只需将备份文件 (dump.rdb) 移动到 redis 安装目录并启动服务即可。获取 redis 目录可以使用 CONFIG 命令，如下所示：

```sql
redis 127.0.0.1:6379> CONFIG GET dir
1) "dir"
2) "/usr/local/redis/bin"
```

以上命令 CONFIG GET dir 输出的 redis 安装目录为 /usr/local/redis/bin。

### 3、其他方法：Bgsave

创建 redis 备份文件也可以使用命令 BGSAVE，该命令在后台执行。

```sql
127.0.0.1:6379> BGSAVE
Background saving started
```
