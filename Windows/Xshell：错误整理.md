# Xshell：错误整理

## 导读

> 整合常见的Xshell报错及解决方法。

### 1、WARNING! The remote SSH server rejected X11 forwarding

- 在用 xshell 连接服务器时有时可能会遇到这种警告

```shell
WARNING! The remote SSH server rejected X11 forwarding
```

- 解决方法：

1. 点击菜单栏 > 文件 > 属性 > 隧道取消勾选”转发X11连接“，点击确定；

2. 重新连接，没有警告了。
