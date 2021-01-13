# No.46 MongoDB：Pymongo使用技巧

## 导读

> Pymongo用法。

### 1、update更新数据

- 现在集合里有3条数据

```python
import pymongo

mongo_client = pymongo.MongoClient(
    host="192.168.0.112", port=27017, username="admin", password="123456"
)
mongo_db = mongo_client["db1"]
# 读取数据
res = mongo_db.chat.find()
for i in res:
    print(i)

# 输出
# {"_id": ObjectId("5cb0ba3abd99392b1427c25e")}
# {"_id": ObjectId("5cb0bbf9bd993914d8b5d82c"), "name": "jack", "age": 13}
# {"_id": ObjectId("5cb0bbf9bd993914d8b5d82d"), "name": "mike", "age": 33}

```

- 更新单条数据

```python
import pymongo

mongo_client = pymongo.MongoClient(
    host="192.168.0.112", port=27017, username="admin", password="123456"
)
mongo_db = mongo_client["db1"]
# 更新数据
res = mongo_db.chat.update_one({"age": 13}, {"$set": {"age": 34}})
# modified_count，返回更新的条数
print(res, res.modified_count)
# 查询是否更新成功
res = mongo_db.chat.find_one({"age": 34})
print(res)

# 返回被更新对象
# <pymongo.results.UpdateResult object at 0x0000000002EDBF08>
# 1代表更新的条数
# 1
# 数据改变，更新成功
# {'_id': ObjectId('5cb0bbf9bd993914d8b5d82c'), 'name': 'jack', 'age': 34}

```

- 更新多条数据

```python
import pymongo

mongo_client = pymongo.MongoClient(
    host="192.168.0.112", port=27017, username="admin", password="123456"
)
mongo_db = mongo_client["db1"]
# 更新数据
res = mongo_db.chat.update_many({"age": {"$gte": 0}}, {"$set": {"age": 888}})
print(res, res.modified_count)

# 返回对象
# <pymongo.results.UpdateResult object at 0x0000000002EDBF08>
# 2代表更新2条数据
# 2

```

### 2、index索引相关操作

简单总结一下pymongo中与index操作相关一些函数, 常用的有：

```python
create_index
drop_index
index_information
```

最主要的是create_index, 可以用它来为mongo的collection建立索引。

以下操作一些简单的例子，代码如下：

```python
import pymongo as pm

client = pm.MongoClient(
    "mongodb://user:password@127.0.0.1:27017",
    ssl=True,
    ssl_ca_certs="/tmp/mongo_local.pem",
)

db = client["my_db"]

collection = db["my_collection"]

# 获得已有索引
print([x for x in dir(collection) if "index" in x])

# 创建索引
collection.create_index([("x", 1)], unique=True, background=True)

# 获得索引帮助
help(collection.create_index)

# 获得索引信息
collection.index_infomation()

# 根据索引说明符删除索引
collection.drop_index([("x", 1)])

# 根据索引名称删除索引
collection.drop_index("idx_x")

# 使用多个字段创建索引
collection.create_index([("x", 1), ("y", 1)])

```

语法中(‘x’,1), x 值为要创建的索引字段名，1为指定按升序创建索引，可以用pymongo.ASCENDING代替。如果你想按降序来创建索引，则指定为 -1 或 pymongo.DESCENDING。

在使用create_index()创建索引时，也可指定特定的参数(options)，常用可选参数如下：

- background：boolean

建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 “background” 可选参数。 “background” 默认值为False。

- unique：boolean

建立的索引是否唯一。指定为True来创建唯一索引。默认值为False.默认情况下，MongoDB在创建集合时会生成唯一索引字段_id。

- name：string

索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。例如create_index([(‘x’,1)]在不指定name时会生成默认的索引名称 ‘x_1’。

- expireAfterSeconds：integer

指定一个以秒为单位的数值，完成TTL设定，设定集合的生存时间。需要在值为日期或包含日期值的数组的字段的创建。
