# GitHub：如何回退代码到历史指定版本

## 导读

> 前提是本地已经有了 GitHub 的 origin master 库或者克隆需要回退的代码到本地。

### 1、查询历史对应不同版本的ID ，用于回退使用

```shell
git log --pretty=oneline
```

### 2、使用 git log 命令查看所有的历史版本

```shell
# 获取你git的某个历史版本的id，假设查到历史版本的id：

fae6966548e3ae76cfa7f38a461c438cf75ba965
```

### 3、恢复到历史版本

```shell
git reset --hard fae6966548e3ae76cfa7f38a461c438cf75ba965
```

### 4、把修改推到远程服务器

```shell
git push -f -u origin master  
```

### 5、重新更新就可以了

```shell
git pull
```
