# Ubuntu16.04：常用软件安装

## 导读

> Ubuntu16.04桌面版做开发的一些常用软件安装方法整理。

### 一、Google Chrome

#### 1、官网下载deb格式直接安装

#### 2、解决root不能打开的问题

```shell
# 找到软件路径
whereis google-chrome

# 修改配置文件
sudo vim /usr/bin/google-chrome

# exec -a "$0" "$HERE/chrome" "$@"  
# 改为
exec -a "$0" "$HERE/chrome" "$@" --user-data-dir --no-sandbox

# 右键快捷方式，点击属性，在命令后添加
--no-sandbox
```

### 二、Pycharm

- 先在[PyCharm官网](https://www.jetbrains.com/pycharm/download/#section=linux)下载安装包

- 右键提取到文件夹，提取完成后，会生成一个pycharm的文件夹

- 在终端指定到pycharm /bin目录下，执行sh命令，打开安装

```shell
sudo sh ./pycharm.sh
```

- 如果报JDK的环境错误，先配置PyCharm的JDK环境

```shell
# 添加，更新，安装
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer

# 配置Java_home环境
sudo apt-get install oracle-java8-set-default
echo JAVA_HOME="/usr/lib/jvm/java-8-oracle" >> /etc/environment
source /etc/environment
```

### 三、VIM

- 安装VIM

```shell
sudo apt-get install vim
```

- VIM主题scheme设置

```shell
vim .vimrc

# 在vimrc文件里添加如下信息即可设置主题
colorscheme 主题插件名

# 例如
colorscheme desert
```

在/usr/share/vim/vim80/color文件夹里，vim已经自带了十几种主题插件，选择一种即可：

```shell
blue.vim      desert.vim    koehler.vim  peachpuff.vim  slate.vim
darkblue.vim  elflord.vim   morning.vim  README.txt     torte.vim
default.vim   evening.vim   murphy.vim   ron.vim        zellner.vim
delek.vim     industry.vim  pablo.vim    shine.vim
```

### 四、Wine

- 安装

```shell
# 下载秘钥
wget -nc https://dl.winehq.org/wine-builds/Release.key

# 导入秘钥:
sudo apt-key add Release.key

# 添加仓储
sudo apt-add-repository https://dl.winehq.org/wine-builds/ubuntu/

# 添加公钥
sudo apt-key adv --recv-keys --keyserver keyserver.Ubuntu.com F987672F

# 更新程序包
sudo apt-get update

# 安装wine稳定版
sudo apt-get install --install-recommends winehq-stable
wine程序安装在/opt目录，工作目录在~/.wine

# 安装winetricks
wget https://raw.githubusercontent.com/Winetricks/winetricks/master/src/winetricks
```

- 常用的命令

```shell
# 安装
wine  xx.exe

# wine的设置
winecfg

# wine系统配置
winetricks

# 任务管理器
wine  taskmgr

# 卸载软件
wine  uninstaller

# 注册表
wine  regedit

# 记事本
wine  notepad

# 重启wine
wineboot
```

### 五、Deepin-Wine

- 下载deepin-wine环境

[下载地址](https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu)，下载zip包，解压到本地文件夹，在文件夹中打开终端。

- 安装deepin-wine环境

```shell
# 安装deepin-wine环境
sudo sh ./install.sh
```

- 安装应用容器

[下载地址](http://mirrors.aliyun.com/deepin/pool/non-free/d/)，下载容器。

```shell
# 使用dpkg命令安装
sudo dpkg -i deb文件名

# 根据经验，通常情况下会报依赖关系的错误，我们可以使用以下的命令修复安装：
sudo  apt-get  install  -f
```

- 举例

TIM、WeChat、7-ZIP

### 六、SMPlayer

```shell
# 添加源仓库
sudo add-apt-repository ppa:rvm/smplayer

# 先更新，再安装
sudo apt-get update
sudo apt-get install smplayer smplayer-themes smplayer-skins

# intel的CPU32位解码器
sudo apt-get install w32codecs

# intel的CPU64位解码器
sudo apt-get install w64codecs
```

### 七、Navicat  Premium

- 下载

[下载地址](https://www.navicat.com.cn/download/navicat-premium)

- 解压tar文件

- 解压后  进入解压后的目录运行命令：

```shell
./start_navicat
```

- 安装后问题：

界面乱码

1. 打开乱码的界面，选择菜单栏第五个（如果Navicat版本不同的话，注意是乱码后括号里为T的那个，表示工具Tool），下拉菜单中选择最后一个，打开为选项。

2. 选项里左边选择第一个，在右边第一个下拉框中选择Noto Sans mono CJK SC Regular，编辑器选项和记录选项也都选择这个字体。

3. 确定保存时要注意，如果你用的虚拟机可能因为界面太小，只能显示到“默认”按钮，实际上下面还有“确定”和“取消”两个按钮显示不出来，千万不要点成默认按钮，否则又还原成默认的字体了。

4. 如果没显示出来确定按钮，用tab键慢慢切换到默认按钮的下一个按钮按回车就保存好了。

连接上数据库后中文数据是乱码，把Ubuntu的字符集修改为zh_CN.utf8。

```shell
# 查看系统支持的字符集
locale -a

# 修改字符集:
export LANG=zh_CN.utf8
```

- 破解方案

```shell
# 第一次执行start_navicat时，会在用户主目录下生成一个名为.navicat的隐藏文件夹
cd /root/.navicat64/

# 此文件夹下有一个system.reg文件，把此文件删除
sudo rm system.reg

# 下次启动navicat 会重新生成此文件，30天试用期会按新的时间开始计算
```

### 八、Kazam

```shell
# 录屏软件
sudo apt-get install kazam

# 摄像头软件
sudo apt-get install cheese
```

### 九、Nodejs+Cnpm

```shell
# 先用普通的apt工具安装低版本的node，然后再升级最新
sudo apt-get install nodejs
sudo apt-get install nodejs-legacy
sudo apt-get install npm

# 更换淘宝的镜像，这个是必须的，用过的node的人都知道
sudo npm config set registry https://registry.npm.taobao.org

# 查看下配置是否生效
sudo npm config list

# 安装更新版本的工具N
sudo npm install n -g

# 更新node版本
sudo n stable

# 安装cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 十、WPS

- 官网下载deb格式直接双击安装

- 解决打开WPS时出现的系统缺失字体问题

下载字体包，解压后，将目录中所有文件复制到/usr/share/fonts下，重新打开WPS，问题解决。
