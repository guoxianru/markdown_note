# Linux：Nohup命令详解

## 导读

> 可以在Linux下永久的在后台执行程序。

### 1、简介

1. nohup 命令运行由 Command参数和任何相关的Arg参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 &（ 表示“and”的符号）到命令的尾部。

2. nohup 是 no hang up 的缩写，就是不挂断的意思。

3. nohup命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。

4. 在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中。

### 2、语法

```shell
nohup command > myout.file 2>&1 &
```

在上面的例子中，0 – stdin (standard input)，1 – stdout (standard output)，2 – stderr (standard error) ；

2>&1是将标准错误（2）重定向到标准输出（&1），标准输出（&1）再被重定向输入到myout.file文件中。

```shell
0 22 * * * /usr/bin/python /home/test.py > /home/test.log 2>&1
```

这是放在crontab中的定时任务，晚上22点时候怕这个任务，启动这个python的脚本，并把日志写在test.log文件中。

### 3、nohup和&的区别

- &

指在后台运行，但当用户推出(挂起)的时候，命令自动也跟着退出。

- nohup

不挂断的运行，注意并没有后台运行的功能就是指，用nohup运行命令可以使命令永久的执行下去，和用户终端没有关系，例如我们断开SSH连接都不会影响他的运行，注意了nohup没有后台运行的意思；&才是后台运行。
nohup COMMAND &

这样就能使命令永久的在后台执行。

### 4、举例

```shell
sh test.sh &
```

将sh test.sh任务放到后台 ，即使关闭xshell退出当前session依然继续运行，但标准输出和标准错误信息会丢失（缺少的日志的输出）。将sh test.sh任务放到后台 ，关闭xshell，对应的任务也跟着停止。

```shell
nohup sh test.sh
```

将sh test.sh任务放到后台，关闭标准输入，终端不再能够接收任何输入（标准输入），重定向标准输出和标准错误到当前目录下的nohup.out文件，即使关闭xshell退出当前session依然继续运行。

```shell
nohup sh test.sh &
```

将sh test.sh任务放到后台，但是依然可以使用标准输入，终端能够接收任何输入，重定向标准输出和标准错误到当前目录下的nohup.out文件，即使关闭xshell退出当前session依然继续运行。
