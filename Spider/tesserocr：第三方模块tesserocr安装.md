# tesserocr：第三方模块tesserocr安装

## 导读

> 在爬虫过程中，难免会遇到各种各样的验证码，而大多数验证码还是图形验证码，这时候我们可以直接用 OCR 来识别。

### 1、介绍

tesserocr 是 Python 的一个 OCR 识别库 ，但其实是对 tesseract 做的一 层 Python API 封装，所以它的核心是 tesseract。 因此，在安装 tesserocr 之前，我们需要先安装tesseract。

### 2、相关链接

tesserocr [GitHub](https://github.com/sirfz/tesserocr)

tesserocr [PyPI](https://pypi.python.org/pypi/tesserocr)

tesseract [下载地址](http://digi.bib.uni-mannheim.de/tesseract)

tesseract [GitHub](https://github.com/tesseract-ocr/tesseract)

tesseract [语言包](http://github.com/tesseract-ocr/tessdata)

tesseract [文档](https://github.com/tesseract-ocr/tesseract/wiki/Documentation)

### 3、Windows下的安装

在 Windows 下，首先需要下载 tesseract，它为 tesserocr 提供了支持。

进入下载页面，可以看到有各种 .exe 文件的下载列表，这里可以选择下载版本 。

其中文件名中带有 dev 的为开发版本，不带 dev 的为稳定版本，可以选择下载不带 dev 的版本， 例如可以选择下载 tesseract-ocr-setup-3 .05.01.exe。

下载完成后双击运行，安装程序。需要注意的是，需要句选 Additional language data(download）选项来安装 OCR 识别支持的语言包，这样 OCR 便可以识别多国语言 。

给tesseract配置环境变量：

1. 将tesseract安装路径添加到path环境变量中

2. 将tesseract的语言包添加到环境变量中，在环境变量中新建一个系统变量，变量名称为TESSDATA_PREFIX，tessdata是放置语言包的文件夹，一般在你安装tesseract的目录下，即tesseract的安装目录就是tessdata的父目录，把TESSDATA_PREFIX的值设置为tessdata的目录。

3. 安装 tesserocr

```shell
pip install tesserocr pillow
```

如果命令会出错，下载whl文件安装 [下载地址](https://github.com/simonflueckiger/tesserocr-windows_build/releases)

选择相应版本，打开Cmd，进入whl文件当前所在目录下，进行安装。

### 4、Linux下的安装

对于Linux来说，不同系统已经有了不同的发行包了，它可能叫作tesseract-ocr或者tesseract，直接用对应的命令安装即可。

1、Ubuntu、Debian和Deepin

在Ubuntu、Debian和Deepin系统下，安装命令如下：

```shell
sudo apt-get install -y tesseract-ocr libtesseract-dev libleptonica-dev
```

2、CentOS、Red Hat

在CentOS和Red Hat系统下，安装命令如下：

```shell
yum install -y tesseract
```

在不同发行版本运行如上命令，即可完成tesseract的安装。安装完成后，便可以调用tesseract命令了。

接着，我们查看一下其支持的语言：

```shell
tesseract --list-langs
```

运行结果示例：

```shell
List of available languages (3):engosdequ
```

结果显示它只支持几种语言，如果想要安装多国语言，还需要安装语言包，官方叫作tessdata（[下载链接](https://github.com/tesseract-ocr/tessdata)）

利用Git命令将其下载下来并迁移到相关目录即可，不同版本的迁移命令如下所示。

在Ubuntu、Debian和Deepin系统下的迁移命令如下：

```shell
git clone https://github.com/tesseract-ocr/tessdata.gitsudo mv tessdata/* /usr/share/tesseract-ocr/tessdata
```

在CentOS和Red Hat系统下的迁移命令如下：

```shell
git clone https://github.com/tesseract-ocr/tessdata.gitsudo mv tessdata/* /usr/share/tesseract/tessdata
```

这样就可以将下载下来的语言包全部安装了。

这时我们重新运行列出所有语言的命令：

```shell
tesseract --list-langs
```

结果如下：

```shell
List of available languages (107):aframharaasmazeaze_cyrlbelbenbodbosbulcatcebceschi_simchi_tra...
```

可以发现，这里列出的语言就多了很多，比如chi_sim就代表简体中文，这就证明语言包安装成功了。

接下来再安装tesserocr即可，这里直接使用pip安装：

```shell
pip install tesserocr pillow
```

### 5、Mac下的安装

在Mac下，我们首先使用Homebrew安装ImageMagick和tesseract库：

```shell
brew install imagemagick brew install tesseract --all-languages
```

接下来再安装tesserocr即可：

```shell
pip install tesserocr pillow
```

这样我们便完成了tesserocr的安装。

### 6、验证安装

准备一张验证码图片

- 用 tesseract 命令测试：

```shell
tesseract image.png result -l eng
```

- 利用 Python 代码测试：

```python
import tesserocr
from PIL import Image

image = Image.open("image.png")
result = tesserocr.image_to_text(image)
print(result)

```

另外，还可以直接调用 tesserocr 模块的 file_to_text() 方法，可以达到同样的效果，但是直接调用file_to_text()方法，路径参数中不能出现中文字符。

```python
import tesserocr

print(tesserocr.file_to_text("image.png"))

```

如果成功输出结果，则证明 tesseract 和 tesserocr 都已经安装成功。

### 7、问题汇总

#### 7.1 报错信息

```shell
Traceback (most recent call last):
  File "c:\Users\NewJune\test.py", line 4, in <module>
    print(tesserocr.image_to_text(image))
  File "tesserocr.pyx", line 2400, in tesserocr._tesserocr.image_to_text
RuntimeError: Failed to init API, possibly an invalid tessdata path: C:\Python36\
```

解决方法：

将Tesseract-OCR目录下的tessdata文件夹（C:\Program Files\Tesseract-OCR\tessdata）整个拷贝到对应Python目录Scripts（C:\Users\Administrator\Envs\python36_spider\Scripts）中即可

#### 7.2 报错信息

```python
!strcmp(locale, "C"):Error:Assert failed:in file baseapi.cpp, line 209
[1] 34012 illegal hardware instruction python3 screenshotProcessor.py
```

该错误是在用docker基础镜像python:3.6上安装tesseract后导入tesserocr报错。

解决方法：

1.添加环境变量：

```shell
export LC_ALL=C
```

或者将该语句配置进~/.bash_profile | ~/.zshrc

2.执行相应的source命令导入环境变量：（执行脚本使用 /bin/bash ）

```shell
source ~/.bashrc
```
