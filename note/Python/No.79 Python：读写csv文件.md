# No.79 Python：读写csv文件

## 导读

> python中有一个读写csv文件的包，直接import csv即可。利用这个python包可以很方便对csv文件进行操作。

### 1、读文件

```python
import csv

csv_reader = csv.reader(open("data.file", encoding="utf-8"))
for row in csv_reader:
    print(row)

```

csv_reader把每一行数据转化成了一个list，list中每个元素是一个字符串。

### 2、写文件

读文件时，我们把csv文件读入列表中，写文件时会把列表中的元素写入到csv文件中。

```python
list = ["1", "2", "3", "4"]
out = open(outfile, "w")
csv_writer = csv.writer(out)
csv_writer.writerow(list)

```

可能遇到的问题：直接使用这种写法会导致文件每一行后面会多一个空行。

解决办法如下：

```python
out = open(outfile, "w", newline="")
csv_writer = csv.writer(out, dialect="excel")
csv_writer.writerow(list)

```

在stackoverflow上找到了比较经典的解释，原来 python3里面对 str和bytes类型做了严格的区分，不像python2里面某些函数里可以混用。所以用python3来写wirterow时，打开文件不要用wb模式，只需要使用w模式，然后带上newline=''。

### 3、示例

- 简单读写

```python
import csv


class writer:
    def __init__(self):
        self.dict = {
            "标题": "标题",
            "链接": "链接",
            "服务": "服务",
            "dsr": "dsr",
            "店铺名": "店铺名",
            "价格": "店铺名",
            "付款人数": "付款人数",
            "发货地": "发货地",
        }
        out = open("outfile.csv", "w", newline="")
        self.csv_writer = csv.writer(out, dialect="excel")
        self.csv_writer.writerow(self.dict)

    def writer_to(self, key_value):
        self.csv_writer.writerow(key_value)


if __name__ == "__main__":
    a = writer()
    new = {
        "链接": "http://www.baidu.com",
        "标题": "我是标题",
    }
    a.dict.update(new)
    print(a.dict)
    a.writer_to(a.dict.values())

```

- 结合爬虫

```python
import csv
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException
from selenium.webdriver.common.action_chains import ActionChains

driver = ["1", "2"]
colspan = ["1", "2"]
try:
    out = open("类目.csv", "w", newline="")
except PermissionError:
    print("文件被其他程序占用")
    input("")
csv_writer = csv.writer(out, dialect="excel")
csv_writer.writerow(["宝贝ID", "类目"])


def open_chrome():
    driver[0] = webdriver.Chrome()
    driver[0].get("https://www.dianchacha.com")
    input("请登陆后按回车:")


def EC_located(one_group, value):
    """
     目的：简化代码长度，参数1选择one或者group切换选中模式
    :param value:要找的值【CSS选择器】
    :return:选择到的对象
    """
    wait = WebDriverWait(driver[0], 10)
    if one_group == "one":
        try:
            ecl = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, value)))
            return ecl
        except TimeoutException:
            print(value, "1元素未加载成功，等待超时")
    else:
        try:
            ecl = wait.until(
                EC.presence_of_all_elements_located((By.CSS_SELECTOR, value))
            )
            return ecl
        except TimeoutException:
            print(value, "1元素---组---未加载成功，等待超时")


def operating(ID):
    # 先获取ID输入框
    driver[0].get("https://www.dianchacha.com/item/info/index/iid/" + ID)
    html = driver[0].page_source
    if "未能找到亲的宝贝" not in html:
        colspans = EC_located("group", ".colspan-1")
        colspan[0] = str(colspans[1].text).replace("宝贝类目： ", "")
    else:
        return operating(ID)
    print(colspan)


def writer_txt():
    csv_writer.writerow([url[0], colspan[0]])
    print("保存", url[0], colspan[0], "成功")


url = ["0", "1"]


def main():
    open_chrome()
    file = "宝贝ID.txt"
    with open(file) as f:
        for line in f.readlines():
            url[0] = line
            print(line)
            operating(url[0])
            writer_txt()
        out.close()
        print("已完成")


if __name__ == "__main__":
    main()

```
