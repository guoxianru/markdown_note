# TCP：测试小工具TCPing

## 导读

> TCPing是使用TCP协议测试端口开放情况的小工具。

### 1、Windows

[下载地址](https://elifulkerson.com/projects/tcping.php)

将下载的 EXE 程序放到 C:\Windows\System32 文件夹下，即可在 cmd 中使用 tcping 命令。

语法

```shell
tcping [-q] [-t timeout_sec] [-u timeout_usec] <host> <port>

# -q : quiet mode, do not output anything (except error messages)
# -t : timeout in seconds
# -u : timeout in microseconds

# 举例
tcping www.baidu.com 80
tcping64 www.baidu.com 80
```

使用说明（只找到英文版）

```shell
NAME
    tcping - simulate "ping" over tcp by establishing a connection to network hosts.
    Measures the time for your system to [SYN], receive the target's [SYN][ACK] and send [ACK].  Note that the travel time for
    the last ACK is not included - only the time it takes to be put on the wire a tthe sending end.

SYNOPSIS
    tcping [-tdsvf46] [-i interval] [-n times] [-w interval] [-b n] [-r times][-j depth] [--tee filename] [-f] destination [port]

DESCRIPTION
    tcping measures the time it takes to perform a TCP 3-way handshake (SYN, SYN/ACK, ACK) between itself and a remote host.
    The travel time of the outgoing final ACK is not included, only the (minimal) amount of time it has taken to drop it on
    the wire at the near end.  This allows the travel time of the (SYN, SYN/ACK) to approximate the travel time of the
    ICMP (request, response) equivalent.

OPTIONS
    -4      Prefer using IPv4

    -6      Prefer using IPv6

    -t      ping continuously until stopped via control-c

    -n count
            send _count_ pings and then stop.  Default 4.

    -i interval
            Wait _interval_ seconds between pings.  Default 1.  Decimals permitted.

    -w interval
            Wait _interval_ seconds for a response.  Default 2.  Decimals permitted.

    -d      include date and time on every output line

    -f      Force sending at least one byte in addition to making the connection.

    -g count
            Give up after _count_ failed pings.

    -b type
            Enable audible beeps.
            '-b 1' will beep "on down".  If a host was up, but now its not, beep.
            '-b 2' will beep "on up".  If a host was down, but now its up, beep.
            '-b 3' will beep "on change".  If a host was one way, but now its the other, beep.
            '-b 4' will beep "always".

    -c      only show output on a changed state

    -r count
            Every _count_ pings, we will perform a new DNS lookup for the host in case it changed.

    -s      Exit immediately upon a success.

    -v      Print version and exit.

    -j      Calculate jitter.  Jitter is defined as the difference between the last response time and the historical average.

    -js depth
            Calculate jitter, as with -j but with an optional _depth_ argument specified. If _depth_ is specified tcping will
            use the prior _depth_ values to calculate a rolling average.

    --tee _filename_
            Duplicate output to the _filename_ specified.  Windows can still not be depended upon to have a useful command line
            environment. Don't tease me, *nix guys.

    --append
            When using --tee, append to rather than overwrite the output file.

    --file
            Treat the "destination" option as a filename.  That file becomes a source of destinations, looped through on a
            line by line basis.  Some options don't work in this mode and statistics will not be kept.


    destination
            A DNS name, an IP address, or (in "http" mode) a URL.
            Do not specify the protocol ("http://") in "http" mode.  Also do not specify server port via ":port" syntax.
            For instance:   "tcping http://www.elifulkerson.com:8080/index.html" would fail
            Use the style:  "tcping www.elifulkerson.com/index.html 8080" instead.

    port
            A numeric TCP port, 1-65535.  If not specified, defaults to 80.

    --header
            include a header with the command line arguments and timestamp.  Header is implied if using --tee.

HTTP MODE OPTIONS  
    -h      Use "http" mode.  In http mode we will attempt to GET the specified document and return additional values including
            the document's size, http response code, kbit/s.
    -u      In "http" mode, include the target URL on each output line.

    --post  Use POST instead of GET in http mode.
    --head  Use HEAD instead of GET in http mode.
    --get   Shorthand to invoke "http" mode for consistency's sake.

    --proxy-server _proxyserver_
            Connect to _proxyserver_ to request the url rather than the server indicated in the url itself.
    --proxy-port _port_
            Specify the numeric TCP port of the proxy server.  Defaults to 3128.
    --proxy-credentials username:password
            Specify a username:password pair which is sent as a 'Proxy-Authorization: Basic' header.


RETURN VALUE
    tcping returns 0 if all pings are successful, 1 if zero pings are successful and 2 for mixed outcome.

BUGS/REQUESTS
    Please report bugs and feature requests to the author via contact information on http://www.elifulkerson.com

AVAILABILITY
    tcping is available at http://www.elifulkerson.com/projects/tcping.php
```

### 2、Linux

```shell
# 下载tar包
wget http://linuxco.de/tcping/tcping-1.3.5.tar.gz

# 解压缩
tar -xvzf tcping-1.3.5.tar.gz

# 如果没有安装GCC，安装一下GCC
yum install gcc

# 使用GCC编译生成执行文件tcping
gcc -o tcping tcping.c

# 将tcping拷贝到路径/usr/bin下面
cp tcping /usr/bin

# 测试一下
tcping www.baidu.com 80
```
