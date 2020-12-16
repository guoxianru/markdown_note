# Pymongo：update更新数据

## 导读

> Pymongo update用法。

### 1、现在集合里有3条数据

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

### 2、更新单条数据

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

### 3、更新多条数据

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
