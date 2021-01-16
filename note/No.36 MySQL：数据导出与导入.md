# No.36 MySQL：数据导出与导入

## 导读

> MySQL快捷方便的对数据进行转移。

- 导出

```shell
# 导出整个数据库
mysqldump -u用户名 -p密码 数据库名称 > 导出的路径和文件名

# 导出数据库一个表，包括表结构和数据
mysqldump -u用户名 -p密码 数据库名 表名> 导出的路径和文件名

# 如果需要导出数据中多张表的结构及数据时，表名用空格隔开
mysqldump -u用户名 -p密码 数据库名 表名1 表名2 > 导出的路径和文件名

# 仅导出数据库结构
mysqldump -u用户名 -p密码 -d 数据库名 > 导出的路径和文件名

# 仅导出表结构
mysqldump -u用户名 -p密码 -d 数据库名 表名> 导出的路径和文件名

# 将语句查询出来的结果导出为.txt文件
mysql -u 用户名 -p密码 数据库名 -e "select * from 表名" > 导出的路径和文件名
```
　　
- 导入

第1种

```shell
# 将备份的数据导入指定数据库
mysql -uroot -p DBname < xxx.sql
```

第2种

```shell
# 进入mysql数据库控制台
mysql -u用户名 -p密码

# 选择要导入的数据库
mysql> use 数据库

# 使用source命令，后面参数SQL本文件
mysql> source xxx.sql
```
