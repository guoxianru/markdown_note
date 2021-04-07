# No.48 Redis：Python-Redis使用技巧

## 导读

> 简单介绍下redis，一个高性能key-value的存储系统，支持存储的类型有String、Hash、List、Set、Zset。在处理大规模数据读写的场景下运用比较多。

参考文章 [python操作Redis详解](https://www.cnblogs.com/fengting0913/p/13511383.html)

### 1、连接Redis数据库

- 直接连接

```python
import redis

"""
    redis://:password@host:port/db(TCP)
    rediss://:password@host:port/db(TCP+SSL)
    unix://:password@/path/to/socket.sock?db=db(UNIX+socket)
"""
# red = redis.Redis.from_url("redis://:password@127.0.0.1:6379/0")
red = redis.Redis(host="127.0.0.1", port=6379, password="1111", db=0, decode_responses=True)

```

- 连接池连接

连接池的原理是, 通过预先创建多个连接, 当进行redis操作时, 直接获取已经创建的连接进行操作

而且操作完成后, 不会释放, 用于后续的其他redis操作，这样就达到了避免频繁的redis连接创建和释放的目的, 从而提高性能。

redis模块采用ConnectionPool来管理对redis server的所有连接。

```python
import redis

pool = redis.ConnectionPool(host="127.0.0.1", port=6379, password="password", db=1)
red = redis.Redis(connection_pool=pool, decode_responses=True)

```

### 2、常用操作

```python
red.delete(*names)
# 根据删除redis中的任意数据类型

red.exists(name)
# 检测redis的name是否存在

red.keys(pattern='*')
# 根据模型获取redis的name
# 更多：
    # KEYS * 匹配数据库中所有 key 。
    # KEYS h?llo 匹配 hello ， hallo 和 hxllo 等。
    # KEYS h*llo 匹配 hllo 和 heeeeello 等。
    # KEYS h[ae]llo 匹配 hello 和 hallo ，但不匹配 hillo

red.expire(name ,time)
# 为某个redis的某个name设置超时时间

red.rename(src, dst)
# 对redis的name重命名

red.move(name, db)
# 将redis的某个值移动到指定的db下

red.randomkey()
# 随机获取一个redis的name（不删除）

red.type(name)
# 获取name对应值的类型

red.flushdb(asynchronous=False)
# 清空当前db中的数据，默认是同步，异步asynchronous=True，会新起一个线程进行清空操作，不阻塞主线程。

red.flushall(asynchronous=False)
# 清空所有db中的数据，默认是同步，异步asynchronous=True，会新起一个线程进行清空操作，不阻塞主线程。

# 批量提交请求pipeline
with red.pipeline(transaction=False) as p:
    p.sadd("name", 1)
    p.execute()

```

### 3、String

- string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。

- string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象。

- string类型是Redis最基本的数据类型，一个键最大能存储512MB。

```python
red.set(name, value, ex=None, px=None, nx=False, xx=False)
# 在Redis中设置值，默认，不存在则创建，存在则修改
# 参数：
     # ex，过期时间（秒）
     # px，过期时间（毫秒）
     # nx，如果设置为True，则只有name不存在时，当前set操作才执行
     # xx，如果设置为True，则只有name存在时，岗前set操作才执行

red.mset(*args, kwargs)
# 批量设置值
# 如：
    # mset(k1='v1', k2='v2')
    # mget({'k1': 'v1', 'k2': 'v2'})

red.setnx(name, value)
# 设置值，只有name不存在时，执行设置操作（添加）

red.setex(name, value, time)
# 设置值
# 参数：
    # time，过期时间（数字秒 或 timedelta对象）

red.psetex(name, time_ms, value)
# 设置值
# 参数：
    # time_ms，过期时间（数字毫秒 或 timedelta对象）

red.get(name)
# 获取值

red.mget(keys, args)
# 批量获取
# 如：
    # mget('hi', 'aliang')
    # r.mget(['hi', 'aliang'])

red.getset(name, value)
# 设置新值并获取原来的值

red.strlen(name)
# 返回name对应值的字节长度（一个汉字3个字节）

red.getrange(key, start, end)
# 获取子序列（根据字节获取，非字符）
# 参数：
    # key，根据键取值的键
    # start，起始位置（字节）
    # end，结束位置（字节）
# 如： "嗨阿良" ，0-3表示 "嗨"

red.setrange(name, offset, value)
# 修改字符串内容，从指定字符串索引开始向后替换（新值太长时，则向后添加）
# 参数：
    # offset，字符串的索引，字节（一个汉字三个字节）
    # value，要设置的值

red.setbit(name, offset, value)
# 对name对应值的二进制表示的位进行操作
# 参数：
    # name，redis的name
    # offset，位的索引（将值变换成二进制后再进行索引）
    # value，值只能是 1 或 0

red.getbit(name, offset)
# 获取name对应的值的二进制表示中的某位的值（0或1）

red.bitcount(key, start=None, end=None)
# 获取name对应的值的二进制表示中 1 的个数
# 参数：
    # key，Redis的name
    # start，位起始位置
    # end，位结束位置

red.bitop(operation, dest, keys)
# 获取多个值，并将值做位运算，将最后的结果保存至新的name对应的值
 
# 参数：
    # operation,AND（并） 、 OR（或） 、 NOT（非） 、 XOR（异或）
    # dest, 新的Redis的name
    # *keys,要查找的Redis的name
# 如：
    # bitop("AND", 'new_name', 'n1', 'n2', 'n3')
    # 获取Redis中n1,n2,n3对应的值，然后讲所有的值做位运算（求并集），然后将结果保存 new_name 对应的值中

red.incr(self, name, amount=1)
# 自增 name对应的值，当name不存在时，则创建name＝amount，否则，则自增。
# 参数：
    # name,Redis的name
    # amount,自增数（必须是整数）

red.incrbyfloat(self, name, amount=1.0)
# 自增 name对应的值，当name不存在时，则创建name＝amount，否则，则自增。
# 参数：
    # name,Redis的name
    # amount,自增数（浮点型）

red.decr(self, name, amount=1)
# 自减 name对应的值，当name不存在时，则创建name＝amount，否则，则自减。
# 参数：
    # name,Redis的name
    # amount,自减数（整数）

red.append(key, value)
# 在redis name对应的值后面追加内容
 
# 参数：
    # key, redis的name
    # value, 要追加的字符串

```

### 4、Hash

- Redis hash 是一个键值对集合。

- Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

```python
red.hset(name, key, value)
# name对应的hash中设置一个键值对（不存在，则创建；否则，修改）
# 参数：
    # name，redis的name
    # key，name对应的hash中的key
    # value，name对应的hash中的value
# 注：
    # hsetnx(name, key, value),当name对应的hash中不存在当前key时则创建（相当于添加）

red.hmset(name, mapping)
# 在name对应的hash中批量设置键值对
# 参数：
    # name，redis的name
    # mapping，字典，如：{'k1':'v1', 'k2': 'v2'}
# 如：
    # r.hmset('xx', {'k1':'v1', 'k2': 'v2'})

red.hget(name,key)
# 在name对应的hash中获取根据key获取value

red.hmget(name, keys, args)
# 在name对应的hash中获取多个key的值
# 参数：
    # name，reids对应的name
    # keys，要获取key集合，如：['k1', 'k2', 'k3']
    # *args，要获取的key，如：k1,k2,k3
# 如：
    # r.mget('xx', ['k1', 'k2'])
    # 或
    # print r.hmget('xx', 'k1', 'k2')

red.hgetall(name)
# 获取name对应hash的所有键值

red.hlen(name)
# 获取name对应的hash中键值对的个数

red.hkeys(name)
# 获取name对应的hash中所有的key的值

red.hvals(name)
# 获取name对应的hash中所有的value的值

red.hexists(name, key)
# 检查name对应的hash是否存在当前传入的key

red.hdel(name,*keys)
# 将name对应的hash中指定key的键值对删除

red.hincrby(name, key, amount=1)
# 自增name对应的hash中的指定key的值，不存在则创建key=amount
# 参数：
    # name，redis中的name
    # key， hash对应的key
    # amount，自增数（整数）

red.hincrbyfloat(name, key, amount=1.0)
# 自增name对应的hash中的指定key的值，不存在则创建key=amount
# 参数：
    # name，redis中的name
    # key， hash对应的key
    # amount，自增数（浮点数）
# 自增name对应的hash中的指定key的值，不存在则创建key=amount

red.hscan(name, cursor=0, match=None, count=None)
# 增量式迭代获取，对于数据大的数据非常有用，hscan可以实现分片的获取数据，并非一次性将数据全部获取完，从而放置内存被撑爆
 
# 参数：
    # name，redis的name
    # cursor，游标（基于游标分批取获取数据）
    # match，匹配指定key，默认None 表示所有的key
    # count，每次分片最少获取个数，默认None表示采用Redis的默认分片个数
# 如：
    # 第一次：cursor1, data1 = r.hscan('xx', cursor=0, match=None, count=None)
    # 第二次：cursor2, data1 = r.hscan('xx', cursor=cursor1, match=None, count=None)
    # ...
    # 直到返回值cursor的值为0时，表示数据已经通过分片获取完毕

red.hscan_iter(name, match=None, count=None)
# 利用yield封装hscan创建生成器，实现分批去redis中获取数据
# 参数：
    # match，匹配指定key，默认None 表示所有的key
    # count，每次分片最少获取个数，默认None表示采用Redis的默认分片个数
# 如：
    # for item in r.hscan_iter('xx'):
    #     print item

```

### 5、List

- Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。

```python
red.lpush(name,values)
# 在name对应的list中添加元素，每个新的元素都添加到列表的最左边
# 如：
    # r.lpush('oo', 11,22,33)
    # 保存顺序为: 33,22,11
# 扩展：
    # rpush(name, values) 表示从右向左操作

red.lpushx(name,value)
# 在name对应的list中添加元素，只有name已经存在时，值添加到列表的最左边
# 扩展：
    # rpushx(name, value) 表示从右向左操作

red.blpop(keys, timeout)
# 将多个列表排列，按照从左到右去pop对应列表的元素
# 参数：
    # keys，redis的name的集合
    # timeout，超时时间，当元素所有列表的元素获取完之后，阻塞等待列表内有数据的时间（秒）, 0 表示永远阻塞
# 更多：
    # r.brpop(keys, timeout)，从右向左获取数据

red.llen(name)
# name对应的list元素的个数

red.linsert(name, where, refvalue, value)
# 在name对应的列表的某一个值前或后插入一个新值
# 参数：
    # name，redis的name
    # where，BEFORE或AFTER
    # refvalue，标杆值，即：在它前后插入数据
    # value，要插入的数据

red.lset(name, index, value)
# 对name对应的list中的某一个索引位置重新赋值
# 参数：
    # name，redis的name
    # index，list的索引位置
    # value，要设置的值

red.lrem(name, value, num)
# 在name对应的list中删除指定的值
# 参数：
    # name，redis的name
    # value，要删除的值
    # num，  num=0，删除列表中所有的指定值；
           # num=2,从前到后，删除2个；
           # num=-2,从后向前，删除2个

red.lpop(name)
# 在name对应的列表的左侧获取第一个元素并在列表中移除，返回值则是第一个元素
# 更多：
    # rpop(name) 表示从右向左操作

red.lindex(name, index)
# 在name对应的列表中根据索引获取列表元素

red.lrange(name, start, end)
# 在name对应的列表分片获取数据
# 参数：
    # name，redis的name
    # start，索引的起始位置
    # end，索引结束位置

red.ltrim(name, start, end)
# 在name对应的列表中移除没有在start-end索引之间的值
# 参数：
    # name，redis的name
    # start，索引的起始位置
    # end，索引结束位置

red.rpoplpush(src, dst)
# 从一个列表取出最右边的元素，同时将其添加至另一个列表的最左边
# 参数：
    # src，要取数据的列表的name
    # dst，要添加数据的列表的name

red.brpoplpush(src, dst, timeout=0)
# 从一个列表的右侧移除一个元素并将其添加到另一个列表的左侧
# 参数：
    # src，取出并要移除元素的列表对应的name
    # dst，要插入元素的列表对应的name
    # timeout，当src对应的列表中没有数据时，阻塞等待其有数据的超时时间（秒），0 表示永远阻塞

# 自定义增量迭代
# 由于redis类库中没有提供对列表元素的增量迭代，如果想要循环name对应的列表的所有元素，那么就需要：
    # 1、获取name对应的所有列表
    # 2、循环列表
# 但是，如果列表非常大，那么就有可能在第一步时就将程序的内容撑爆，所有有必要自定义一个增量迭代的功能：
 
def list_iter(name):
    """
    自定义redis列表增量迭代
    :param name: redis中的name，即：迭代name对应的列表
    :return: yield 返回 列表元素
    """
    list_count = r.llen(name)
    for index in xrange(list_count):
        yield r.lindex(name, index)
 
# 使用
for item in list_iter("pp"):
    print item

```

### 6、Set

- Redis的Set是string类型的无序集合。

- 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。

```python
red.sadd(name,values)
# name对应的集合中添加元素

red.scard(name)
# 获取name对应的集合中元素个数

red.sdiff(keys, args)
# 在第一个name对应的集合中且不在其他name对应的集合的元素集合

red.sdiffstore(dest, keys, args)
# 获取第一个name对应的集合中且不在其他name对应的集合，再将其新加入到dest对应的集合中

red.sinter(keys, args)
# 获取多一个name对应集合的并集

red.sinterstore(dest, keys, args)
# 获取多一个name对应集合的并集，再讲其加入到dest对应的集合中

red.sismember(name, value)
# 检查value是否是name对应的集合的成员

red.smembers(name)
# 获取name对应的集合的所有成员

red.smove(src, dst, value)
# 将某个成员从一个集合中移动到另外一个集合

red.spop(name)
# 从集合的右侧（尾部）移除一个成员，并将其返回

red.srandmember(name, numbers)
# 从name对应的集合中随机获取 numbers 个元素

red.srem(name, values)
# 在name对应的集合中删除某些值

red.sunion(keys, args)
# 获取多一个name对应的集合的并集

red.sunionstore(dest,keys, args)
# 获取多一个name对应的集合的并集，并将结果保存到dest对应的集合中

sscan(name, cursor=0, match=None, count=None)
sscan_iter(name, match=None, count=None)
# 同字符串的操作，用于增量迭代分批获取元素，避免内存消耗太大

```

### 7、Zset

- Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。

- 不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

- zset的成员是唯一的,但分数(score)却可以重复。

```python
red.zadd(name, *args, kwargs)
# 在name对应的有序集合中添加元素
# 如：
     # zadd('zz', 'n1', 1, 'n2', 2)
     # zadd('zz', n1=11, n2=22)

red.zcard(name)
# 获取name对应的有序集合元素的数量

red.zcount(name, min, max)
# 获取name对应的有序集合中分数 在 [min,max] 之间的个数

red.zincrby(name, value, amount)
# 自增name对应的有序集合的 name 对应的分数

red.zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)
# 按照索引范围获取name对应的有序集合的元素
# 参数：
    # name，redis的name
    # start，有序集合索引起始位置（非分数）
    # end，有序集合索引结束位置（非分数）
    # desc，排序规则，默认按照分数从小到大排序
    # withscores，是否获取元素的分数，默认只获取元素的值
    # score_cast_func，对分数进行数据转换的函数
# 更多：
    # 从大到小排序
    # zrevrange(name, start, end, withscores=False, score_cast_func=float)
    # 按照分数范围获取name对应的有序集合的元素
    # zrangebyscore(name, min, max, start=None, num=None, withscores=False, score_cast_func=float)
    # 从大到小排序
    # zrevrangebyscore(name, max, min, start=None, num=None, withscores=False, score_cast_func=float)

red.zrank(name, value)
# 获取某个值在 name对应的有序集合中的排行（从 0 开始）
# 更多：
    # zrevrank(name, value)，从大到小排序

red.zrangebylex(name, min, max, start=None, num=None)
# 当有序集合的所有成员都具有相同的分值时，有序集合的元素会根据成员的 值 （lexicographical ordering）来进行排序，而这个命令则可以返回给定的有序集合键 key 中， 元素的值介于 min 和 max 之间的成员
# 对集合中的每个成员进行逐个字节的对比（byte-by-byte compare）， 并按照从低到高的顺序， 返回排序后的集合成员。 如果两个字符串有一部分内容是相同的话， 那么命令会认为较长的字符串比较短的字符串要大
 
# 参数：
    # name，redis的name
    # min，左区间（值）。 + 表示正无限； - 表示负无限； ( 表示开区间； [ 则表示闭区间
    # min，右区间（值）
    # start，对结果进行分片处理，索引位置
    # num，对结果进行分片处理，索引后面的num个元素
# 如：
    # ZADD myzset 0 aa 0 ba 0 ca 0 da 0 ea 0 fa 0 ga
    # r.zrangebylex('myzset', "-", "[ca") 结果为：['aa', 'ba', 'ca']
# 更多：
    # 从大到小排序
    # zrevrangebylex(name, max, min, start=None, num=None)

red.zrem(name, values)
# 删除name对应的有序集合中值是values的成员
# 如：zrem('zz', ['s1', 's2'])

red.zremrangebyrank(name, min, max)
# 根据排行范围删除

red.zremrangebyscore(name, min, max)
# 根据分数范围删除

red.zremrangebylex(name, min, max)
# 根据值返回删除

red.zscore(name, value)
# 获取name对应有序集合中 value 对应的分数

red.zinterstore(dest, keys, aggregate=None)
# 获取两个有序集合的交集，如果遇到相同值不同分数，则按照aggregate进行操作
# aggregate的值为:  SUM  MIN  MAX

red.zunionstore(dest, keys, aggregate=None)
# 获取两个有序集合的并集，如果遇到相同值不同分数，则按照aggregate进行操作
# aggregate的值为:  SUM  MIN  MAX

red.zscan(name, cursor=0, match=None, count=None, score_cast_func=float)
red.zscan_iter(name, match=None, count=None,score_cast_func=float)
# 同字符串相似，相较于字符串新增score_cast_func，用来对分数进行操作

```
