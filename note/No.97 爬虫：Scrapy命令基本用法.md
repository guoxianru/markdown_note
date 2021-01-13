# No.97 爬虫：Scrapy命令基本用法

## 导读

> scrapy命令很多，在此整理一下。

### 1、全局命令

```shell
startproject
genspider
settings
runspider
shell
fetch
view
version
```

### 2、局部命令（只在项目中使用的命令）

```shell
crawl
check
list
edit
parse
bench
```

### 3、详解

```shell
# 创建项目
scrapy startproject myproject

# 在项目中创建新的spider文件
scrapy genspider mydomain mydomain.com
# mydomain为spider文件名，mydomain.com为爬取网站域名

# 运行spider文件
scrapy crawl <spider>

# 检查spider文件有无语法错误
scrapy check

# 列出spider路径下的spider文件
scrapy list

# 编辑spider文件，相当于打开vim模式，实际并不好用，在IDE中编辑更为合适
scrapy edit <spider>

# 将网页内容下载下来，然后在终端打印当前返回的内容，相当于 request 和 urllib 方法
scrapy fetch <url>

# 将网页内容保存下来，并在浏览器中打开当前网页内容，直观呈现要爬取网页的内容
scrapy view <url>

# 打开 scrapy 显示台，类似ipython，可以用来做测试
scrapy shell [url]

# 输出格式化内容：
scrapy parse <url> [options]

# 返回系统设置信息：
scrapy settings [options]
# 举例
scrapy settings --get BOT_NAME
# 输出
scrapybot

# 运行spider：
sapy runspider <spider_file.py>

# 显示scrapy版本，后面加 -v 可以显示scrapy依赖库的版本
scrapy version [-v]

# 测试电脑当前爬取速度性能：
scrapy bench
```
