# No.72 Linux：Ubuntu16.04的apt、dpkg使用

## 导读

> apt和dpkg区别是dpkg绕过apt包管理数据库对软件包进行操作，所以你用dpkg安装过的软件包用apt可以再安装一遍，系统不知道之前安装过了，将会覆盖之前dpkg的安装。

### 1、apt的使用

apt会解决和安装模块的依赖问题,并会咨询软件仓库, 但不会安装本地的deb文件, apt是建立在dpkg之上的软件管理工具。

- 安装软件

```shell
# 在线安装软件包
apt-get install 软件名

# 重新安装软件包
apt-get install 软件名 --reinstall
```

- 卸载软件

```shell
# 删除软件包
sudo apt-get remove 软件名

# 删除软件包及配置文件
sudo apt-get remove --purge 软件名

# 删除不再需要的软件包
sudo apt-get autoremove
```

- 更新系统

```shell
# 清除索引
sudo apt-get clean

# 更新源
sudo apt-get update

# 更新软件
sudo apt-get upgrade

# 根据依赖关系更新
sudo apt-get dist-upgrade

# 修复损坏的软件包，尝试卸载出错的包，重新安装正确版本的
sudo apt-get -f install

# 删除不再需要的软件包
sudo apt-get autoremove
```

- 添加启动器和桌面快捷方式

```shell
# 进入快捷方式目录
/usr/share/applications

# 添加一个启动器配置文件，以vscode为例，创建vscode.desktop文件

sudo vim vscode.desktop

# 添加如下内容（需要做适当修改
[Desktop Entry]
Version=1.0
Name=${程序名称}
Exec=${可执行文件路径}
Terminal=false
Icon=${表示该可执行文件的图标}
Type=Application
Categories=Development

# 在Dock最上面的Search里面找到vscode程序，然后拖放到桌面上的Dock即可点击运行
```

### 2、dpkg的使用

dpkg是用来安装.deb文件,但不会解决模块的依赖关系,且不会关心ubuntu的软件仓库内的软件,可以用于安装本地的deb文件。

- 安装软件

```shell
# 安装软件，安装本地软件包，不解决依赖关系
sudo  dpkg  -i  deb文件名

# 根据经验，通常情况下会报依赖关系的错误，我们可以使用以下的命令修复安装
sudo  apt-get  install  -f

# 查看已经安装的软件，并找到自己的安装的软件名
sudo  dpkg  -l
```

- 卸载软件

```shell
# 删除软件包
sudo dpkg  -r 软件名

# 删除软件包及配置文件
sudo dpkg -P
```

- 处理软件包出错

```shell
# 将info文件夹更名
sudo mv /var/lib/dpkg/info /var/lib/dpkg/info_old

# 再新建一个新的info文件夹
sudo mkdir /var/lib/dpkg/info

# 更新
sudo apt-get update
sudo apt-get -f install

# 执行完上一步操作后会在新的info文件夹下生成一些文件
# 现将这些文件全部移到info_old文件夹下
sudo mv /var/lib/dpkg/info/* /var/lib/dpkg/info_old

# 把自己新建的info文件夹删掉
sudo rm -rf /var/lib/dpkg/info

# 把以前的info文件夹重新改回名字
sudo mv /var/lib/dpkg/info_old /var/lib/dpkg/info
```
