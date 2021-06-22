# No.125 Windows：为cmd设置代理

## 导读

> 为Windows的终端cmd设置代理。

cmd设置代理，需要执行以下命令

```shell
set http_proxy=http://127.0.0.1:10809
set https_proxy=http://127.0.0.1:10809
```

上面命令的作用是设置环境变量，会持续到cmd窗口关闭。
