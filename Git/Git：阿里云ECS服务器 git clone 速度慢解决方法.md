# Git：阿里云ECS服务器 git clone 速度慢解决方法

## 导读

> 有些时候远程的ECS服务器 git clone 的速度会很慢，常维持在 10k/s 左右。

- 编辑 /etc/ssh/ssh_config。

```shell
vim /etc/ssh/ssh_config
```

- 找到 GSSAPIAuthentication no 这行,删掉前面的注释,然后保存退出。

- 然后就可以看到从远程仓库 git clone 的速度已经明显变快了。
