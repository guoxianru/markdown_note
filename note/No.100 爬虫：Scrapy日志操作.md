# No.100 爬虫：Scrapy日志操作

## 导读

> Scrapy提供了log功能，可以通过 logging 模块使用。

- logging设置

通过在setting.py中进行以下设置可以被用来配置logging

```python
# 默认: True，启用logging
LOG_ENABLED = True

# 默认: 'utf-8'，logging使用的编码
LOG_ENCODING = "utf-8"

# 默认: None，在当前目录里创建logging输出文件的文件名
LOG_FILE = "name.log"

# 默认: 'DEBUG'，log的最低级别
LOG_LEVEL = "DEBUG"

# 默认: False 如果为 True，进程所有的标准输出(及错误)将会被重定向到log中。
# 例如，执行 print "hello" ，其将会在Scrapy log中显示。
LOG_STDOUT = False

```

- Scrapy提供5层logging级别

CRITICAL - 严重错误(critical)

ERROR - 一般错误(regular errors)

WARNING - 警告信息(warning messages)

INFO - 一般信息(informational messages)

DEBUG - 调试信息(debugging messages)

- 日志按日期记录并保存成文件

```python
from datetime import datetime

# 当前时间
today = datetime.now()
# 日志文件按日期命名
log_file_path = "logs/log_{}_{}_{}.log".format(today.year, today.month, today.day)
# 日志输出级别
LOG_LEVEL = "DEBUG"
# 日志输出路径
LOG_FILE = log_file_path

```
