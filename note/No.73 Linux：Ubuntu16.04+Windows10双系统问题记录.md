# No.73 Linux：Ubuntu16.04+Windows10双系统问题记录

## 导读

> 联想拯救者笔记本安装双系统（ubun16.04 + Windows10），遇到了许多奇奇怪怪的问题，在此记录一下。

### 一、Ubuntu系统安装

1. 进入bios设置，开机狂按F2。

2. 进入Configuration选项，把SATA Controller Mode设置为ACHI模式。

3. 进入Security选项，把Secure Boot选为disabled。

4. 进入Boot选项，将Boot Mode设置为UEFI，将USB Boot设置为Enabled。

5. 安装遇到卡在logo的问题以及安装完后卡在logo的问题。

情况一、

1.在选项卡的位置用上下键选择Install ubuntu的选项，先别点，按e进入编辑选项，会看到quiet splash --- 字样的代码，将 --- 去除，输入 nomodeset （内核不加载视频驱动程序）。按F10重新引导。

2.下次开机还会遇到问题，在引导界面中，在 ubuntu 选项上，先别点，按e进入编辑选项，会看到quiet splash --- 字样的代码，将 --- 去除，输入 nomodeset 。按F10重新引导。

3.进入系统后，按照系统适配安装专有NVIDIA驱动。

```shell
sudo vi /etc/default/grub

# 将
GRUB_CMDLINE_LINUX_DEFAULT=”quiet splash”
# 改为
GRUB_CMDLINE_LINUX_DEFAULT=”quiet splash nomodeset”

# 终端更新一下grub
sudo update-grub
```

情况二、

1.在选卡项的位置用上下键选择Install ubuntu的选项，先别点，按e进入编辑选项，会看到quiet splash --- 字样的代码，在--- 后面，输入 acpi=off （关闭高级电源管理接口）。按F10重新引导。

2.下次开机还会遇到问题，在引导界面中，在 ubuntu 选项上，先别点，按e进入编辑选项，会看到quiet splash --- 字样的代码，在--- 后面，输入 acpi=off 。按F10重新引导。

3.进入系统后，按照系统适配安装专有NVIDIA驱动。

```shell
sudo gedit /etc/default/grub
# 将acpi=off删去，双引号不用删去，保存。

# 终端更新一下grub
sudo update-grub
```

### 二、启动盘制作

1. 官网下载需要的Ubuntu镜像文件。
2. rufus制作U盘启动盘，操作很简单，相关教程很多。

### 三、安装Ubuntu系统

#### 1、分区设置

UEFI+GPT与Legacy+MBR不同的是不用挂载在/boot下，而是选择EFI。

- 200M，采用逻辑分区，用于efi。（这个类似于旧方法的boot）

efi系统分区，选中逻辑分区（这里不是主分区，请勿怀疑，老式的boot挂载才是主分区）和空间起始位置，大小2048Mb,它的作用和boot引导分区一样，但是boot引导是默认grub引导的，而efi显然是UEFI引导的。不要按照那些老教程去选boot引导分区，也就是最后你的挂载点里没有“/boot”这一项，否则你就没办法UEFI启动两个系统了。

- 8GM，采用逻辑分区，用于swap。

swap交换空间，这个也就是虚拟内存的地方，选择主分区和空间起始位置。如果你给Ubuntu系统分区容量足够的话，最好是能给到你物理内存的2倍大小，像我8GB内存，就可以给个16GB的空间给它，这个看个人使用情况，太小也不好，太大也没用。

- 20G，采用主分区，用于Ext 4，挂载到 / 。

最后，挂载“/”，（即根目录）类型为EXT4日志文件系统，选中主分区和空间起始位置，“/”就把除了之前你挂载的home的全部杂项囊括了，大小也不要太小，一般30GB。

- 将剩下所有空间分配给/home，采用逻辑分区，用于Ext 4，挂载到 home。

挂载“/home”，类型为EXT4日志文件系统，选中逻辑分区和空间起始位置，这个相当于你的个人文件夹，类似Windows里的User，我建议最好能分配稍微大点，因为你的图片、视频、下载内容基本都在这里面，一般50GB给home。

#### 2、启动问题

安装完成后，是Ubuntu的grub引导界面，可以在该页面选择进入Ubuntu或windows系统。

由于之前设置SATA Controller Mode为ACHI模式，发现导致Windows启动不了。解决办法：

1. 进入win10系统，重启系统为“安全模式”。
2. 进入bios设置，将SATA Controller Mode改为ACHI模式，保存退出。
3. 重启计算机，进入win10“安全模式”，进入安全模式后再重启，正常进入win10正常。

### 四、ubuntu windows双系统默认启动项轻松切换

1.同时按住键盘上的“Ctrl Alt T”三个键（即快捷键“Ctrl+Alt+T”），打开终端窗口

```shell
sudo gedit /etc/default/grub
```

2.把grub文件中的 GRUB_DEFAULT=0中的0改为saved

```shell
# 将
GRUB_DEFAULT=0
# 改为
saved
```

3.在文件末尾添加

```shell
GRUB_SAVEDEFAULT=true
```

4.保存文件并退出

5.在终端输入

```shell
sudo update-grub
```

更新启动配置文件

6.重启系统

重启到启动菜单时，选择你要更改为默认启动项的系统，按 Enter 键确认启动即可，下次启动时刚刚选择的系统即为默认启动系统，直到你手动选择启动其他的系统为止。以后可以轻易的来回切换默认系统了

### 五、ubuntu系统重启卡死

- 卸载原有显卡驱动：

```shell
sudo apt-get remove --purge nvidia*
```

- 禁用集卡驱动：

```shell
sudo gedit /etc/modprobe.d/blacklist.conf

# 在文本最后添加：
blacklist nouveau
options nouveau modeset=0
```

- 更新系统内核

```shell
sudo update-initramfs -u
```

- 安装NVIDIA驱动

```shell
sudo add-apt-repository ppa:xorg-edgers/ppa -y
sudo apt-get update

# 根据需要选择版本
sudo apt-get install nvidia-384
```

- 检验是否安装成功

重启之后输入如下命令：

```shell
sudo nvidia-smi
nvidia-settings
```

若出现NVIDIA驱动界面则安装成功，此时重启卡死等问题均不再出现。

### 六、进入Ubuntu发现搜索不到WIFI列表

#### 1、原因：无线网卡被hard blocked

在终端敲入：

```shell
rfkill list all
```

会出现：

```shell
0:ideapad_wlan: Wireless LAN
Soft blocked: no
Hard blocked:yes

1:ideapad_bluetooth: Bluetooth
Soft blocked: no
Hard blocked: yes

2:phy0: Wireless LAN
Soft blocked: no
Hard blocked:no

3:hci0: Bluetooth
Soft blocked: yes
Hard blocked: no
```

可以看到，优先级前的ideapad_wlan的Hard blocked默认为yes，即ubuntu默认关闭了硬件wifi开关，而联想R720的wifi只有软件开关，没有硬件开关的启动，所以引起了wifi无法开启的问题。

#### 2、解决方法

从无线模块的显示列表可以看出，序号2的wifi模块是软硬件是可以启动的，所以，只要将前面默认的模块移出即可。

- 移出ideapad无线模块：

```shell
sudo modprobe -r ideapad_laptop
```

- 使用命令查看：

```shell
rfkill list all
```

如下提示：

```shell
2:phy0: Wireless LAN
Soft blocked: no
Hard blocked:no

3:hci0: Bluetooth
Soft blocked: yes
Hard blocked: no
```

即wifi模块工作正常，然而每次重启ubuntu系统都要重新进行模块移出，故可将该命令设置为开机自启动。

- 命令设置为开机自启动

在/etc/rc.local文件中添加命令：

```shell
sudo gedit /etc/rc.local

# 在exit 0之前加上添加命令：
echo "123" |sudo modprobe -r ideapad_laptop
```

开机启动后系统会自动执行改脚本文件，完成wifi模块的自动移出操作。

### 七、Ubuntu 更换国内源

1、在修改source.list前，最好先备份一份，以便日后恢复,设置为保留备份后，修改前的内容会自动保存为后缀名为bak的备份文件。

```shell
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

2、打开源列表文件

```shell
sudo gedit /etc/apt/sources.list
```

3、修改更新源，任选一种国内镜像源内容复制到source.list文件中，覆盖原文件内容（“#”开头的那一行为注释，可以直接复制进文件中）

```shell
# 阿里云
deb http://mirrors.aliyun.com/ubuntu/ xenial main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main

deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main

deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe

deb http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe
```

4、更新

```shell
# 清除索引
sudo apt-get clean

# 更新源
sudo apt-get update

# 修复损坏的软件包，尝试卸载出错的包，重新安装正确版本的
sudo apt-get -f install

# 更新软件
sudo apt-get upgrade

# 根据依赖关系更新
sudo apt-get dist-upgrade
```

### 八、windows和ubuntun双系统时间不对的问题

```shell
# 校准时间
sudo apt-get update
sudo apt-get install ntpdate
sudo ntpdate time.windows.com

# 然后将时间更新到硬件上：
sudo hwclock --localtime --systohc
```

重新进入windows10，发现时间恢复正常了！

### 九、移动 Ubuntu16.04 桌面左侧的启动器到屏幕底部

1.底

```shell
gsettings set com.canonical.Unity.Launcher launcher-position Bottom
```

2.左

```shell
gsettings set com.canonical.Unity.Launcher launcher-position Left
```

### 十、设置root用户登录图形界面

```shell
# 编辑配置文件
sudo gedit /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf

# 添加一行，增加登陆选项
greeter-show-manual-login=true

# 添加一行，禁用客人会话
allow-guest=false

# 给root设置密码
sudo passwd root
```

root用户在登录会有错误,读取/root/.profile时发生错误：mesg:tty n

```shell
sudo gedit /root/.profile

# 找到 mesg n，替换成
tty -s && mesg n || true
```

### 十一、快捷键和启动器无法打开终端

可能是装Python时遗留下来的问题，把指向原Python3.5的文件指向python3.6

```shell
# 进入路径
cd  /usr/lib/python3/dist-packages/gi/

# 重命名两个文件
sudo mv _gi_cairo.cpython-35m-x86_64-linux-gnu.so _gi_cairo.cpython-36m-x86_64-linux-gnu.so
sudo mv _gi.cpython-35m-x86_64-linux-gnu.so _gi.cpython-36m-x86_64-linux-gnu.so
```

再打开终端就正常了

### 十二、删除不常用的软件

1.firefox

- 查找firefox具体内容

```shell
dpkg --get-selections |grep firefox
```

- 卸载软件

```shell
sudo apt-get purge firefox firefox-locale-en unity-scope-firefoxbookmarks
```

2.不常用软件包清单

```shell
# libreoffice
sudo apt-get remove --purge libreoffice*

# Amazon链接
sudo apt-get remove --purge unity-webapps-common*

# 雷鸟邮件客户端
sudo apt-get remove --purge thunderbird*

# 自带的播放器
sudo apt-get remove --purge totem*

# 自带播放器
sudo apt-get remove --purge vlc*

# 自带的音乐播放器
sudo apt-get remove --purge rhythmbox*

# 自带的即时聊天应用
sudo apt-get remove --purge empathy*

# 自带的光盘刻录器
sudo apt-get remove --purge brasero*

# 扫描仪
sudo apt-get remove --purge simple-scan*

# 对对碰游戏
sudo apt-get remove --purge gnome-mahjongg*

# 纸牌游戏
sudo apt-get remove --purge aisleriot*

# 扫雷游戏
sudo apt-get remove --purge gnome-mines*

# 数独游戏
sudo apt-get remove --purge gnome-sudoku*

# BT客户端
sudo apt-get remove --purge transmission-common*

# 屏幕阅读
sudo apt-get remove --purge gnome-orca*

# 备份
sudo apt-get remove --purge deja-dup*

# 屏幕键盘
sudo apt-get remove --purge onboard*
```

3.自动卸载无用依赖

```shell
sudo apt-get autoremove
```

### 十三、Ubuntu16.04简易美化教程

- 安装unity-tweak-tool

```shell
sudo apt-get install unity-tweak-tool
```

- 安装主题、图标、鼠标指针、字体

```shell
# Mac主题、图标、鼠标指针：
sudo add-apt-repository ppa:noobslab/macbuntu
sudo apt-get update
sudo apt-get install macbuntu-os-icons-lts-v7
sudo apt-get install macbuntu-os-ithemes-lts-v7

# Mac字体：
sudo mkdir /usr/share/fonts/apple

# 将下载好的Mac字体压缩包解压到
/usr/share/fonts/apple
```

- 设置unity-tweak-tool

打开unity-tweak-tool

选择下载的主题、图标、鼠标指针、字体

- 终端的外观设置

在终端界面下右键选择配置文件首选项

勾选上“使用透明背景”将其透明度稍微拉到10%左右

将“内置方案”改成“Tango”

- cairo-dock

```shell
# 安装
sudo apt-get install  cairo-dock

# 启动
cairo-dock
```

在dash菜单中搜索“启动应用程序”并打开

点击添加

填入添加信息：“cairo-dock”

```shell
名称（N）：cairo-dock
命令（M）：cairo-dock
注释（E）：cairo-dock
```

- 打开unity-tweak-tool失败

```shell
sudo apt-get install unity-control-center
```

### 十四、解决挂载失败问题

```shell
# 进入root模式

sudo su

# 切换路径
cd /media/root
mkdir OS_tmp

# 建立一个临时挂载文件夹，nvme0n1p2为挂载失败盘
mount -o ro  /dev/nvme0n1p2  /media/root/OS_tmp
```

### 十五、root用户登录没有声音

1. 在dash菜单中搜索“启动应用程序”并打开
2. 点击添加
3. 填入添加信息：

```shell
名称（N）：声音
命令（M）：pulseaudio --start --log-target=syslog
注释（E）：声音
```

### 十六、设置中心打不开

```shell
sudo apt-get install unity-control-center
```

### 十七、小键盘开机自启

```shell
# 安装一个小软件
sudo apt-get install numlockx

# 编辑配置文件
sudo gedit /usr/share/lightdm/lightdm.conf.d/50-unity-greeter.conf

# 在最后添加：
greeter-setup-script=/usr/bin/numlockx on

# 重启或者注销。启动时，小键盘自动打开。
```
