# Python：高效编程技巧

## 导读

> 工作中经常要处理各种各样的数据，遇到项目赶进度的时候自己写函数容易浪费时间。Python 中有很多内置函数帮你提高工作效率。

### 一、根据条件在序列中筛选数据

- 假设有一个数字列表 data, 过滤列表中的负数

```python
data = [1, 2, 3, 4, -5]

# 使用列表推导式
result = [i for i in data if i >= 0]

# 使用 fliter 过滤函数
result = filter(lambda x: x >= 0, data)

```

- 学生的数学分数以字典形式存储，筛选其中分数大于 80 分的同学

```python
from random import randint

d = {x: randint(50, 100) for x in range(1, 21)}
r = {k: v for k, v in d.items() if v > 80}

```

### 二、对字典的键值对进行翻转

- 使用 zip() 函数

zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的列表。

```python
from random import randint, sample

s1 = {x: randint(1, 4) for x in sample("abfcdrg", randint(1, 5))}
d = {k: v for k, v in zip(s1.values(), s1.keys())}

```

### 三、统计序列中元素出现的频度

- 某随机序列中，找到出现次数最高的3个元素，它们出现的次数是多少

方法1:

```python
# 可以使用字典来统计，以列表中的数据为键，以出现的次数为值
from random import randint

# 构造随机序列
data = [randint(0, 20) for _ in range(30)]

# 列表中出现数字出现的次数
d = dict.fromkeys(data, 0)

for v in d:
    d[v] += 1

```

方法2：

```python
# 直接使用 collections 模块下面的 Counter 对象
from collections import Counter
from random import randint

data = [randint(0, 20) for _ in range(30)]

c2 = Counter(data)

# 查询元素出现次数
c2[14]

# 统计频度出现最高的3个数
c2.most_common(3)

```

- 对某英文文章单词进行统计，找到出现次数最高的单词以及出现的次数

```python
import re
from collections import Counter

# 统计某个文章中英文单词的词频
with open("test.txt", "r", encoding="utf-8") as f:
    d = f.read()

# 所有的单词列表
total = re.split("\W+", d)
result = Counter(total)
print(result.most_common(10))

```

### 四、根据字典中值的大小，对字典中的项进行排序

- 比如班级中学生的数学成绩以字典的形式存储，请按数学成绩从高到底进行排序

方法1:

```python
# 利用 zip 将字典转化为元组，再用 sorted 进行排序
from random import randint

data = {x: randint(60, 100) for x in "xyzfafs"}
sorted(data)
data = sorted(zip(data.values(), data.keys()))

```

方法2:

```python
# 利用 sorted 函数的 key 参数
from random import randint

data = {x: randint(60, 100) for x in "xyzfafs"}
data.items()
sorted(data.items(), key=lambda x: x[1])

```

### 五、在多个字典中找到公共键

- 实际场景：在足球联赛中，统计每轮比赛都有进球的球员

第一轮：{"C罗": 1, "苏亚雷斯":2, "托雷斯": 1..}

第二轮：{"内马尔": 1, "梅西":2, "姆巴佩": 3..}

第三轮：{"姆巴佩": 2, "C罗":2, "内马尔": 1..}

```python
from random import randint, sample
from functools import reduce

# 模拟随机的进球球员和进球数
s1 = {x: randint(1, 4) for x in sample("abfcdrg", randint(1, 5))}
s2 = {x: randint(1, 4) for x in sample("abfcdrg", randint(1, 5))}
s3 = {x: randint(1, 4) for x in sample("abfcdrg", randint(1, 5))}

# 首先获取字典的 keys，然后取每轮比赛 key 的交集。由于比赛轮次数是不定的，所以使用 map 来批量操作
# map(dict.keys, [s1, s2, s3])

# 然后一直累积取其交集，使用 reduce 函数
reduce(lambda x, y: x & y, map(dict.keys, [s1, s2, s3]))

```
