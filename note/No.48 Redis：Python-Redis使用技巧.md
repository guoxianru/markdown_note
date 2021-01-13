# No.48 Redis：Python-Redis使用技巧

## 导读

> 简单介绍下redis，一个高性能key-value的存储系统，支持存储的类型有String、Hash、List、Set、Zset。在处理大规模数据读写的场景下运用比较多。

### 1、连接Redis数据库

- 直接连接

```python
import redis

red = redis.Redis(host="127.0.0.1", port=6379, password="password", db=1)
# red = redis.StrictRedis(host="127.0.0.1", port=6379, password="password", db=1)

```

- 连接池连接

连接池的原理是, 通过预先创建多个连接, 当进行redis操作时, 直接获取已经创建的连接进行操作

而且操作完成后, 不会释放, 用于后续的其他redis操作，这样就达到了避免频繁的redis连接创建和释放的目的, 从而提高性能。

redis模块采用ConnectionPool来管理对redis server的所有连接。

```python
import redis

pool = redis.ConnectionPool(host="127.0.0.1", port=6379, password="password", db=1)
red = redis.Redis(connection_pool=pool)
# red = redis.StrictRedis(connection_pool=pool)

```

### 2、常用操作

```python
# 清空当前db中的数据，默认是同步，异步asynchronous=True，会新起一个线程进行清空操作，不阻塞主线程。
red.flushdb(asynchronous=False)
# 清空所有db中的数据，默认是同步。
red.flushall(asynchronous=False)
# 根据name删除redis中的任意数据类型
red.delete("name")
# 检查name是否存在
red.exists("name")
# 获取所有name
red.keys(pattern="*")
red.keys(pattern="a*")
# 为某个name的设置过期时间
red.expire("name", 3)
# 重命名
red.rename("name1", "name2")
# 将name移动到指定的db
red.move('name', db=1)
# 随机获取一个name
red.randomkey()

```

### 3、String

- string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。

- string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。

- string类型是Redis最基本的数据类型，一个键最大能存储512MB。

```python
# 设置键值
red.set("key", "value")
# 设置键值及过期时间，以秒为单位
red.setex("key", 3, "value")
# 设置多个键值
red.mget({"key1": "value1", "key2": "value2"})
# 追加指定键的值
red.set("key", "value")  # 输出:'value'
red.append("key", "one")  # 输出:'valueone'
# 获取键值
red.get("key")
# 获取多个键值
red.mget("key1", "key2")
# 获取key对应值的字节长度(一个汉字3个字节)
red.strlen("key")

```

### 4、Hash

- Redis hash 是一个键值对集合。

- Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

```python
# 设置单个属性(不存在创建，已存在修改)
red.hset("name", "key", "value")
# 设置多个属性
red.hmset("name", {"key1": "aa", "key2": "bb"})
# 获取指定键的所有属性
red.hkeys("name")
# 获取属性的值
red.hget("name", "key")
# 获取多个属性的值
red.hmget("name", "key1", "key2")
# 获取所有属性的值
red.hvals("name")
# 删除指定键的指定属性
red.hdel("name", "key1")
# 获取指定键的所有属性
red.hgetall("name")
# 获取指定键的属性个数
red.hlen("name")
# 检查指定键是否存在当前传入的属性
red.hexists("name", "key1")
# 与hash中key对应的值相加，不存在则创建key=amount(amount为整数)
red.hincrby("name", "key", amount=10)

```

### 5、List

- Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。

```python
# 在左侧插入元素
red.lpush("name", "元素1", "元素2")
# 当name存在时，元素才能从list的左边加入
red.lpushx("name", "元素1")
# 在右侧插入元素
red.rpush("name", "元素1", "元素2")
# 当name存在时，元素才能从list的右边加入
red.rpushx("name", "元素1")
# 在name对应的列表的某一个值前或后插入一个元素(BEFORE/AFTER)
red.linsert("name", "BEFORE", "列表内元素", "新插入元素")
# name列表长度
red.llen("name")
# 对list中的某一个索引位置重新赋值
red.lset("name", 0, "abc")
# 删除name对应的list中的指定值
red.lrem("name", 2, "元素1")
# 移除列表的左侧第一个元素，返回值则是第一个元素
red.lpop("name")
# 根据索引获取列表内元素
red.lindex("name", 1)
# 分片获取元素
red.lrange("name", 0, -1)
# 移除列表内没有在该索引之内的值
red.ltrim("name", 0, 2)

```

### 6、Set

- Redis的Set是string类型的无序集合。

- 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

```python
# 给name对应的集合中添加元素
red.sadd("name", "aa")
red.sadd("name", "aa", "bb")
# 获取name对应的集合中的元素个数
red.scard("name")
# 获取name对应的集合的所有成员
red.smembers("name")
# 随机获得集合中的元素
red.srandmember("name", 1)
# 在第一个name对应的集合中且不在其他name对应的集合的元素集合
red.sadd("name", "aa", "bb")
red.sadd("name1", "bb", "cc")
red.sadd("name2", "bb", "cc", "dd")
red.sdiff("name", "name1", "name2")  # 输出:aa
# 检查value是否是name对应的集合内的元素
red.sismember("name", "value")
# 将某个元素从一个集合中移动到另外一个集合
red.smove("name1", "name2", "value")
# 从集合的右侧移除一个元素，并将其返回
red.spop("name")
# 删除指定元素
red.srem("name", "value")

```

### 7、Zset

- Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

- 不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

- zset的成员是唯一的,但分数(score)却可以重复。

```python
# 在name对应的有序集合中添加元素
red.zadd("name", "value1", 1)
red.zadd("name", "value2", 2, "value3", 3)
# 获取有序集合内元素的数量
red.zcard("name")
# 获取有序集合中分数在[min,max]之间的个数
red.zcount("name", 1, 5)
# 按照索引范围获取name对应的有序集合的元素
red.zrange("name", 0, 1)  # 从小到大
red.zrevrange("name", 0, 1)  # 从大到小
# 获取value值在name对应的有序集合中的排行位置(从0开始)
red.zrank("name", "a2")  # 从小到大
red.zrevrank("name", "a2")  # 从大到小
# 获取name对应有序集合中 value对应的分数
red.zscore("name", "value")
# 删除name对应的有序集合中值是values的成员
red.zrem("name", "value1", "value2")
# 根据排行范围删除[min,max]
red.zremrangebyrank("name", 1, 5)
# 根据分数范围删除[min,max]
red.zremrangebyscore("name", 1, 5)

```
