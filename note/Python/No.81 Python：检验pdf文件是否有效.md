# No.81 Python：检验pdf文件是否有效

## 导读

> 利用PyPDF2的PdfFileReader模块检测pdf文件的合法性。

### 1、基本原理

利用PyPDF2的PdfFileReader模块打开pdf文件，如果不抛异常，就认为此pdf文件有效。

有时打开并不抛出异常，但是有这种警告：

```shell
UserWarning: startxref on same line as offset [pdf.py:1680]。
```

这种情况pdf多半也是坏的，可进一步通过页数判断。但walker在测试中发现，对于正常pdf文件，进一步通过页数判断时有时会抛出异常。

### 2、pdf文件在本地磁盘上

```python
import traceback
from PyPDF2 import PdfFileReader

# 参数为pdf文件全路径名
def isValidPDF_pathfile(pathfile):
    bValid = True
    try:
        # PdfFileReader(open(pathfile, 'rb'))
        reader = PdfFileReader(pathfile)
        if reader.getNumPages() < 1:  # 进一步通过页数判断。
            bValid = False
    except:
        bValid = False
        print("*" + traceback.format_exc())

    return bValid

```

### 3、pdf是来自网络的bytes数据

由于PdfFileReader的参数为文件名或文件对象，所以需要做一下转换

- 方法一

```python
import traceback, tempfile
from PyPDF2 import PdfFileReader

# 参数为bytes类型数据。利用临时文件。
def isValidPDF_bytes(pdfBytes):
    bValid = True
    try:
        fp = tempfile.TemporaryFile()
        fp.write(pdfBytes)
        reader = PdfFileReader(fp)
        fp.close()
        if reader.getNumPages() < 1:  # 进一步通过页数判断。
            bValid = False
    except:
        bValid = False
        print("*" + traceback.format_exc())

    return bValid

```

- 方法二

```python
import io, traceback
from PyPDF2 import PdfFileReader

# 参数为bytes类型数据。利用BytesIO转换。
def isValidPDF_bytes(pdfBytes):
    bValid = True
    try:
        b = io.BytesIO(pdfBytes)
        reader = PdfFileReader(b)
        if reader.getNumPages() < 1:  # 进一步通过页数判断。
            bValid = False
    except:
        bValid = False
        print("*" + traceback.format_exc())

    return bValid

```

### 3、利用PDFlib判断

```python
import os
from PDFlib.PDFlib import PDFlib
from PDFlib.PDFlib import PDFlibException


def isValidPdf(pathfile):
    p = PDFlib()

    p.set_option("license=xxxxxx-xxxxxx-xxxxxx-xxxxxx-xxxxxx")
    p.set_option("errorpolicy=return")

    indoc = p.open_pdi_document(pathfile, "repair=none")
    print("indoc:" + str(indoc))
    print("pathfile size:" + str(os.path.getsize(pathfile)) + "B")
    bValid = False
    if indoc == -1:
        print("*" + p.get_errmsg())
        bValid = False
    else:
        pageNumber = p.pcos_get_number(indoc, "length:pages")
        print("pageNumber:" + str(pageNumber))
        if pageNumber < 1:  # 页数为0
            bValid = False
        else:
            bValid = True

    if bValid:
        p.close_pdi_document(indoc)

    return bValid

```
