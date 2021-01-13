# No.27 MySQL：命令行远程连接

## 导读

> 远程连接服务器上的MySQL。

- 打开命令行

- 开启权限

```shell
# 使用root登录
mysql -uroot -p

# 赋予用户权限
grant all privileges on *.* to 连接用户@IP地址 identified by 连接密码;

# 刷新权限
flush privileges
```

- 连接

```shell
mysql -hIP地址 -u连接用户 -p连接密码
```

- 连接不上可能出现的原因

1. ERROR 1045 (28000): Access denied for user '用户名'@'IP地址' (using password: YES)，密码不对。

2. 防火墙阻挡远程连接数据库，关闭防火墙或允许连接MySQL。
