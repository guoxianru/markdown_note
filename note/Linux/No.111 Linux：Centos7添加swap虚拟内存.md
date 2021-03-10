# No.111 Linux：Centos7添加swap虚拟内存

## 导读

> 如果机器的虚拟内存swap不足或者需要调整，可以手动的增加。

### 1、查看内存的使用情况

```shell
free -m
```

### 2、创建一个swap文件，大小为1G

```shell
dd if=/dev/zero of=/home/swap bs=1024 count=1024000

# /home目录下面多了一个1G大小的文件swap
```

### 3、将文件格式转换为swap格式的

```shell
mkswap /home/swap
```

### 4、再用swapon命令把这个文件分区挂载swap分区

```shell
swapon /home/swap
```

### 5、为防止重启后swap分区变成0，要修改/etc/fstab文件

```shell
vi /etc/fstab

# 在文件末尾（最后一行）加上

/home/swap swap swap default 0 0
```
