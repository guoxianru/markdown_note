# No.65 Linux：进程、端口相关操作

## 导读

> 有关Linux的进程、端口的查询，终止等操作。

### 一、进程

#### 1、进程查询

```shell
ps -aux
ps -aux | grep python
```

ps命令用于报告当前系统的进程状态。

a：显示当前终端下的所有进程信息，包括其他用户的进程。

u：使用以用户为主的格式输出进程信息。

x：显示当前用户在所有终端下的进程。

```shell
ps -elf
ps -elf | grep python
```

ps命令用于报告当前系统的进程状态。

-e：显示系统内的所有进程信息。

-l：使用长（long）格式显示进程信息。

-f：使用完整的（full）格式显示进程信息

```shell
pstree -aup
pstree -aup | grep python
```

以树状图的方式展现进程之间的派生关系，显示效果比较直观。

-a：显示每个程序的完整指令，包含路径，参数或是常驻服务的标示；

-c：不使用精简标示法；

-G：使用VT100终端机的列绘图字符；

-h：列出树状图时，特别标明现在执行的程序；

-H<程序识别码>：此参数的效果和指定"-h"参数类似，但特别标明指定的程序；

-l：采用长列格式显示树状图；

-n：用程序识别码排序。预设是以程序名称来排序；

-p：显示程序识别码；

-u：显示用户名称；

#### 2、进程终止

```shell
# 终止指定进程
kill -9 进程号

# 查找python进程并终止
ps -ef | grep python | grep -v grep | awk '{print $2}' | xargs kill -9

# 查询并终止python相关进程
pgrep python | xargs kill -s 9

# 终止所有名称匹配的进程
killall -9 完整进程名
```

### 二、端口

#### 1、端口查询

```shell
lsof -i:端口号
```

lsof(list open files)是一个列出当前系统打开文件的工具。

lsof -i:8080：查看8080端口占用

lsof abc.txt：显示开启文件abc.txt的进程

lsof -c abc：显示abc进程现在打开的文件

lsof -c -p 1234：列出进程号为1234的进程所打开的文件

lsof -g gid：显示归属gid的进程情况

lsof +d /usr/local/：显示目录下被进程开启的文件

lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长

lsof -d 4：显示使用fd为4的进程

lsof -i -U：显示所有打开的端口和UNIX domain文件

```shell
netstat -tunlp | grep 端口号
```

netstat -tunlp 用于显示 tcp，udp 的端口和进程等相关情况。

-t (tcp) 仅显示tcp相关选项

-u (udp)仅显示udp相关选项

-n 拒绝显示别名，能显示数字的全部转化为数字

-l 仅列出在Listen(监听)的服务状态

-p 显示建立相关链接的程序名
