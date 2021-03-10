# No.12 网络：代理服务的理论实践

## 导读

> 代理服务器（Proxy Server）是个人网络和Internet服务商之间的中间代理机构，它负责转发合法的网络信息，对转发进行控制和登记。

### 1、原理

扫描一个网络中的服务器，扫出那些启用代理服务的机器，测试它们是哪种类型的代理（透明代理、匿名代理还是高匿代理），然后代理提供商将这些代理提供给它的客户。

### 2、扫描代理服务器

nmap是一个网络扫描的工具，它可以用来扫描对方服务器启用了哪些端口、哪些服务，服务器是否在线，以及猜测服务器可能运行的操作系统。

```shell
nmap 49.51.193.128

......22/tcp open ssh 111/tcp open rpcbind 1080/tcp open socks......
```

针对一个网段作扫描，会扫出在这个网段中有哪些在线的机器，每台机器上启用了哪些服务。

```shell
nmap 49.51.193.0/24
```

### 3、检测代理类型

代理基本上分成三种类型，三者在技术层面的区别，主要在于HTTP请求头的内容不同。

- 透明代理：对方知道是代理，知道你是谁。

```shell
REMOTE_ADDR = Proxy IP
HTTP_VIA = Proxy IP
HTTP_X_FORWARDED_FOR = Your IP
```

- 匿名代理：对方知道是代理，不知道你是谁。

```shell
REMOTE_ADDR = proxy IP
HTTP_VIA = proxy IP
HTTP_X_FORWARDED_FOR = proxy IP
```

- 高匿代理：对方不知道你是代理，不知道你是谁。

```shell
REMOTE_ADDR = Proxy IP
HTTP_VIA = not determined
HTTP_X_FORWARDED_FOR = not determined
```

客户端通过代理向web服务器发起请求，web程序打印出请求头，通过分析请求头的内容就可以知道这个代理是哪种类型的。

```python
# Flask实例
import json

from flask import Flask, request

app = Flask(__name__)


@app.route("/")
def hello():
    header = {}
    if "REMOTE_ADDR" in request.headers:
        header["REMOTE_ADDR"] = request.headers["REMOTE_ADDR"]
    if "HTTP_VIA" in request.headers:
        header["HTTP_VIA"] = request.headers["HTTP_VIA"]
    if "HTTP_X_FORWARDED_FOR" in request.headers:
        header["HTTP_X_FORWARDED_FOR"] = request.headers["HTTP_X_FORWARDED_FOR"]
    return json.dumps(header)


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)

```

### 4、维护代理池

有了代理和代理的类型，可以做成代理池，提供接口给客户，通过接口来获取可用的代理。

需要保证代理池中的代理是有效的，定期的去检查代理的有效性，把失效的从列表中去除，把新的有效的加入进来。
