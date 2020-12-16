# Redis-Python：常用操作

## 导读

> 简单介绍下redis，一个高性能key-value的存储系统，支持存储的类型有string、list、set、zset和hash。在处理大规模数据读写的场景下运用比较多。

### 1、连接Redis数据库

- 直接连接

```python
import redis

red = redis.Redis(host="localhost", port=6379, db=1)

```

- 连接池连接

连接池的原理是, 通过预先创建多个连接, 当进行redis操作时, 直接获取已经创建的连接进行操作

而且操作完成后, 不会释放, 用于后续的其他redis操作，这样就达到了避免频繁的redis连接创建和释放的目的, 从而提高性能。

redis模块采用ConnectionPool来管理对redis server的所有连接。

```python
import redis

pool = redis.ConnectionPool(host="localhost", port=6379, db=1)
red = redis.Redis(connection_pool=pool)
red.set("key1", "value1")
red.set("key2", "value2")

```

### 2、String类型存取

```python
# 在Redis中设置值，默认不存在则创建，存在则修改
set(name, value, ex=None, px=None, nx=False, xx=False)
# ex，过期时间（秒）
# px，过期时间（毫秒）
# nx，如果设置为True，则只有key不存在时，当前set操作才执行,同#setnx(key, value)
# xx，如果设置为True，则只有key存在时，当前set操作才执行

# 设置过期时间（秒）
setex(name, value, time)

# 设置过期时间（豪秒）
psetex(name, time_ms, value)

# 批量设置值
mset(*args, **kwargs)
red.mget({"key1": "value1", "key2": "value2"})

# 获取某个key的值
get(name)
red.get("key1")

# 批量获取
mget(keys, *args)
red.mget("key1", "key1")

# 返回key对应值的字节长度（一个汉字3个字节）
strlen(name)
red.strlen("key")

# 在name对应的值后面追加内容
append(name, value)
red.set("key", "value")
print(r.get("key"))  # 输出:'value'
r.append("key", "one")
print(r.get("key"))  # 输出:'valueone'

```

### 3、Hash类型：一个name对应一个dic字典来存储

```python
# name对应的hash中设置一个键值对（不存在，则创建，否则，修改）
hset(name, key, value)
red.hset("name", "key", "value")

# 在name对应的hash中根据key获取value
hget(name, key)
red.hset("name", "key", "value")
print(red.hget("name", "key"))  # 输出:'value'

# 获取name所有键值对
hgetall(name)
red.hgetall("name")

# 在name对应的hash中批量设置键值对,mapping:字典
hmset(name, mapping)
dic = {"key1": "aa", "key2": "bb"}
red.hmset("name", dic)
print(red.hget("name", "key2"))  # 输出:bb

# 在name对应的hash中批量获取键所对应的值
hmget(name, keys, *args)
dic = {"key1": "aa", "key2": "bb"}
red.hmget("name", dic)
print(red.hmget("name", "key1", "key2"))  # 输出:['aa', 'bb']

# 获取hash键值对的个数
hlen(name)
print(red.hlen("name"))  # 输出：2

# 获取hash中所有key
hkeys(name)
red.hkeys("name")

# 获取hash中所有value
hvals(name)
red.hvals("name")

# 检查name对应的hash是否存在当前传入的key
hexists(name, key)
print(red.hexists("name", "key1"))  # 输出：Ture

# 删除指定name对应的key所在的键值对，删除成功返回1，失败返回0
hdel(name, *keys)
print(red.hdel("name", "key1"))  # 输出：1

# 与hash中key对应的值相加，不存在则创建key=amount(amount为整数)
hincrby(name, key, amount=1)
print(red.hincrby("name", "key", amount=10))  # 返回：10

```

### 4、list类型：一个name对应一个列表存储

```python
# 元素从list的左边添加，可以添加多个
lpush(name, *values)
red.lpush("name", "元素1", "元素2")

# 元素从list右边添加，可以添加多个
rpush(name, *values)
red.rpush("name", "元素1", "元素2")

# 当name存在时，元素才能从list的左边加入
lpushx(name, *values)
red.lpushx("name", "元素1")

# 当name存在时，元素才能从list的右边加入
rpushx(name, *values)
red.rpushx("name", "元素1")

# name列表长度
llen(name)
red.llen("name")

# 在name对应的列表的某一个值前或后插入一个新值
linsert(name, where, refvalue, value)
# 参数：
# name: redis的name
# where: BEFORE（前）或AFTER（后）
# refvalue: 列表内的值
# value: 要插入的数据

# 对list中的某一个索引位置重新赋值
lset(name, index, value)
red.lset("name", 0, "abc")

# 删除name对应的list中的指定值
lrem(name, value, num=0)
# 参数：
# name:  redis的name
# value: 要删除的值
# num:   num=0 删除列表中所有的指定值；
#        num=2 从前到后，删除2个；
#        num=-2 从后向前，删除2个'''
red.lrem("name", "元素1", num=0)

# 移除列表的左侧第一个元素，返回值则是第一个元素
lpop(name)
print(red.lpop("name"))

# 根据索引获取列表内元素
lindex(name, index)
print(red.lindex("name", 1))

# 分片获取元素
lrange(name, start, end)
print(red.lrange("name", 0, -1))

# 移除列表内没有在该索引之内的值
ltrim(name, start, end)
red.ltrim("name", 0, 2)

```

### 5、set类型：集合是不允许重复的列表

```python
# 给name对应的集合中添加元素
sadd(name, *values)
red.sadd("name", "aa")
red.sadd("name", "aa", "bb")

# 获取name对应的集合中的元素个数
scard(name)
red.scard("name")

# 获取name对应的集合的所有成员
smembers(name)
red.smembers("name")

# 在第一个name对应的集合中且不在其他name对应的集合的元素集合
sdiff(keys, *args)
red.sadd("name", "aa", "bb")
red.sadd("name1", "bb", "cc")
red.sadd("name2", "bb", "cc", "dd")
print(red.sdiff("name", "name1", "name2"))  # 输出:{aa}

# 检查value是否是name对应的集合内的元素
sismember(name, value)

# 将某个元素从一个集合中移动到另外一个集合
smove(src, dst, value)

# 从集合的右侧移除一个元素，并将其返回
spop(name)

```

### 6、其他常用操作

```python
# 清空当前db中的数据,默认是同步。
# 若开启异步asynchronous=True，会新起一个线程进行清空操作，不阻塞主线程。
flushdb(asynchronous=False)

# 清空所有db中的数据，默认是同步。异步同flushdb
flushall(asynchronous=False)

# 根据name删除redis中的任意数据类型
delete(*names)

# 检查redis的name是否存在
exists(name)

# 根据* ？等通配符匹配获取redis的name
keys(pattern="*")

# 为某个name的设置过期时间
expire(name, time)

# 重命名
rename(src, dst)

# 将redis的某个name移动到指定的db下
move(name, db)

# 随机获取一个redis的name（不删除）
randomkey()

# 获取name对应值的类型
type(name)

```
