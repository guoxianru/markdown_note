# No.31 MongoDB：数据导出与导入

## 导读

> mongodb数据导出与导入主要分为二种：一种是针对库的mongodump和mongorestore，一种是针对库中表的mongoexport和mongoimport。

- 导出

```shell
# 针对库的mongodump
mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表 -o 文件存放路径

参数说明：
    -h 指明数据库宿主机的IP
    -u 指明数据库的用户名
    -p 指明数据库的密码
    -d 指明数据库的名字
    -c 指明collection的名字
    -o 指明到要导出的文件名
    -q 指明导出数据的过滤条件
    --port 指明数据库的端口

# 针对库中表的mongoexport
mongoexport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 -f 字段 -q 条件导出 --csv -o 文件名

参数说明：
    -f 导出指定字段，以逗号分割，-f uid,name,age导出uid,name,age这三个字段
    -q 可以根据查询条件导出，-q '{ "uid" : "100" }' 导出uid为100的数据
    --csv 表示导出的文件格式为csv的。这个比较有用，因为大部分的关系型数据库都是支持csv，在这里有共同点
```
　　
- 导入

```shell
# 针对库的mongodump
mongorestore -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 --drop 文件存在路径

参数说明：
    --drop：先删除所有的记录，然后恢复

# 针对库中表的mongoexport
# 恢复整表导出的非csv文件
mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --upsert --drop 文件名

参数说明：
    --upsert:插入或者更新现有数据

# 恢复部分字段的导出文件
mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --upsertFields 字段 --drop 文件名

参数说明：
    --upsertFields:更新部分的查询字段，必须为索引,以逗号分隔

# 恢复导出的csv文件
mongoimport -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -c 表名 --type 类型 --headerline --upsert --drop 文件名

参数说明：
    --type：导入的文件类型，默认json
```
