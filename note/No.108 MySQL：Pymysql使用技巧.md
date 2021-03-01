# No.108 MySQL：Pymysql使用技巧

## 导读

> 使用Python操作MySQL的小技巧。

### 1、获取插入数据的主键id

```python
import pymysql

database = pymysql.connect(
    host="127.0.0.1", port=3306, user="root", password="root", database="test"
)
cursor = database.cursor()

for i in range(5):
    cursor.execute('insert into test (name) values ("test")')
    print(database.insert_id())
    database.commit()


cursor.close()
database.close()

```

通过db.insert_id()方法可以获取插入数据的主键id, 注意一定要在commit之前获取,否则返回0。

### 2、创建时间、更新时间

```sql
DEFAULT CURRENT_TIMESTAMP
表示当插入数据的时候，该字段默认值为当前时间

ON UPDATE CURRENT_TIMESTAMP
表示每次更新这条数据的时候，该字段都会更新成当前时间
```

这两个操作是mysql数据库本身在维护，可以根据这个特性来生成【创建时间】和【更新时间】两个字段，且不需要代码来维护。

```sql
CREATE TABLE `test` (
    `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

### 3、Python插入数据库时字符串中含有单引号或双引号报错

可以使用 pymysql.escape_string()转换

```python
import pymysql

mydb = pymysql.connect(host="", user="", password="", database="", charset="",)
if type(str_content) is str:
    str_content = mydb.escape_string(str_content)

```

### 3、连接MySQL时raise err.InterfaceError("(0, '')")

Python长时间连接MySQL而没有进行任何处理，所以就自动断开了

```python
import pymysql

mydb = pymysql.connect(host="", user="", password="", database="", charset="")
cursor = mydb.cursor()
sql = ""
# 执行SQL前ping下MySQL服务器
mydb.ping(reconnect=True)
cursor.execute(sql)

```
