# No.83 Python：时间转换相关操作

## 导读

> 在编写代码时，往往涉及时间、日期、时间戳的相互转换。

### 1、str类型时间-时间数组

```python
import time

# 字符类型的时间
ttime_str = "2020-01-01 11:22:33"
# 转为时间数组
timeArray = time.strptime(ttime_str, "%Y-%m-%d %H:%M:%S")
print(timeArray)
# 年
print(timeArray.tm_year)
# 月(1-12)
print(timeArray.tm_mon)
# 日(1-31)
print(timeArray.tm_mday)
# 时(0-23)
print(timeArray.tm_hour)
# 分(0-59)
print(timeArray.tm_min)
# 秒(0-61,60或61是闰秒)
print(timeArray.tm_sec)
# 周几(0到6,0是周一)
print(timeArray.tm_wday)
# 一年中的第几天(1-366)
print(timeArray.tm_yday)
# 是否为夏令时(1/夏令时,0/不是夏令时,-1/未知,默认-1)
print(timeArray.tm_isdst)

```

### 2、str类型时间-显示格式

```python
import time

# 字符类型的时间
time_str = "2020-01-01 11:22:33"
# 转为时间数组
timeArray = time.strptime(time_str, "%Y-%m-%d %H:%M:%S")
# 转为指定显示格式
otherStyleTime = time.strftime("%Y/%m/%d %H:%M:%S", timeArray)
print(otherStyleTime)

```

### 3、str类型时间-时间戳

```python
import datetime
import time


# 字符类型的时间
ttime_str = "2020-01-01 11:22:33"
# 转为时间数组
timeArray = time.strptime(ttime_str, "%Y-%m-%d %H:%M:%S")
print(timeArray)
# 转为时间戳
timeStamp = int(time.mktime(timeArray))
print(timeStamp)

# 使用time
timeStamp = 1577848953
# 转为时间数组
timeArray = time.localtime(timeStamp)
print(timeArray)
# 转为指定显示格式
otherStyleTime = time.strftime("%Y-%m-%d %H:%M:%S", timeArray)
print(otherStyleTime)

# 使用datetime
timeStamp = 1577848953
# 转为datetime.datetime
dateArray = datetime.datetime.fromtimestamp(timeStamp)
print(dateArray)
# 转为指定显示格式
otherStyleTime = dateArray.strftime("%Y-%m-%d %H:%M:%S")
print(otherStyleTime)

# 使用datetime，指定utc时间，相差8小时
timeStamp = 1577848953
# 转为datetime.datetime
dateArray = datetime.datetime.utcfromtimestamp(timeStamp)
print(dateArray)
# 转为指定显示格式
otherStyleTime = dateArray.strftime("%Y-%m-%d %H:%M:%S")
print(otherStyleTime)

```

### 4、获取当前时间

```python
import datetime
import time

# datetime获取当前时间，数组格式
now = datetime.datetime.now()
print(now)

# 获取当前时间戳
timeStamp = time.time()
print(timeStamp)
# 秒
timeStamp_s = int(timeStamp)
print(timeStamp_s)
# 毫秒
timeStamp_ms = int(timeStamp * 1000)
print(timeStamp_ms)
# 微妙
timeStamp_us = int(timeStamp * 1000000)
print(timeStamp_us)

```
