# Scrapy：多个爬虫同时运行

## 导读

> scrapy项目可能需要写多个爬虫，本文介绍如何让它们同时运行。

### 一、

在spiders目录的同级目录下创建一个commands目录，并在该目录中创建一个crawlall.py，将scrapy源代码里的commands文件夹里的crawl.py源码复制过来，只修改run()方法即可。（文件夹下面必须要有__init__文件）

```python
import os
from scrapy.commands import ScrapyCommand
from scrapy.utils.conf import arglist_to_dict
from scrapy.utils.python import without_none_values
from scrapy.exceptions import UsageError


class Command(ScrapyCommand):
    requires_project = True

    def syntax(self):
        return "[options] <spider>"

    def short_desc(self):
        return "Run all spider"

    def add_options(self, parser):
        ScrapyCommand.add_options(self, parser)
        parser.add_option(
            "-a",
            dest="spargs",
            action="append",
            default=[],
            metavar="NAME=VALUE",
            help="set spider argument (may be repeated)",
        )
        parser.add_option(
            "-o",
            "--output",
            metavar="FILE",
            help="dump scraped items into FILE (use - for stdout)",
        )
        parser.add_option(
            "-t",
            "--output-format",
            metavar="FORMAT",
            help="format to use for dumping items with -o",
        )

    def process_options(self, args, opts):
        ScrapyCommand.process_options(self, args, opts)
        try:
            opts.spargs = arglist_to_dict(opts.spargs)
        except ValueError:
            raise UsageError("Invalid -a value, use -a NAME=VALUE", print_help=False)
        if opts.output:
            if opts.output == "-":
                self.settings.set("FEED_URI", "stdout:", priority="cmdline")
            else:
                self.settings.set("FEED_URI", opts.output, priority="cmdline")
            feed_exporters = without_none_values(
                self.settings.getwithbase("FEED_EXPORTERS")
            )
            valid_output_formats = feed_exporters.keys()
            if not opts.output_format:
                opts.output_format = os.path.splitext(opts.output)[1].replace(".", "")
            if opts.output_format not in valid_output_formats:
                raise UsageError(
                    "Unrecognized output format '%s', set one"
                    " using the '-t' switch or as a file extension"
                    " from the supported list %s"
                    % (opts.output_format, tuple(valid_output_formats))
                )
            self.settings.set("FEED_FORMAT", opts.output_format, priority="cmdline")

    def run(self, args, opts):
        # 获取爬虫列表
        spd_loader_list = self.crawler_process.spider_loader.list()  # 获取所有的爬虫文件。
        print(spd_loader_list)
        # 遍历各爬虫
        for spname in spd_loader_list or args:
            self.crawler_process.crawl(spname, **opts.spargs)
            print("此时启动的爬虫为：" + spname)
        self.crawler_process.start()

```

### 二、

settings.py配置文件还需要加一条。

```python
COMMANDS_MODULE = "项目名称.目录名称"
# COMMANDS_MODULE = "spider.commands"

```

### 三、

最后启动crawlall即可！

当然，安全起见，可以先在命令行中进入该项目所在目录，并输入scrapy -h,可以查看是否有命令crawlall 。如果有，那就成功了，可以启动了

```shell
scrapy crawlall
# 爬虫好像是2个同时运行，而且运行时是交叉的
```
