# Linux：修改文件夹权限和用户名用户组

## 导读

> Linux系统下经常遇到文件或者文件夹的权限问题，或者是因为文件夹所属的用户问题而没有访问的权限。

### 一、查看权限

在命令行使用命令，可以查看文件或者文件的权限

```shell
ll
# 或者
ls -a
```

输出结果

```shell
-rw-r--r--. 1 root root 6 Nov  9 16:42 a.txt
```

“-rw-r--r--”表示权限，一共有十个字符

“-”则表示是文件，如果是“d”则表示是目录（directory）

后面9个字符每3个字符又作为一个组，则有3组信息（“rw-”、“r--”、“r--”）

分别表示所属用户本身具有的权限、所属用户的用户组其他成员的权限、其他用户的权限。

r是读权限、w是写权限、x是可执行权限、-没有对应字符的权限。

Linux里面对这些字符设置对应的数值，r是4，w是2，x是1，-是0。

“rw-”是6（=4+2+0），a.txt的权限是644，属于root用户组的root用户。

### 二、修改权限：chmod

#### 1、改文件的权限

修改文件a.txt的权限为755

```shell
chmod 755 a.txt
```

#### 2、改文件夹的权限

- 只改变文件夹本身权限，不改动子文件（夹）

```shell
chmod 600 my/
```

- 改变文件夹及子目录下所有文件（夹）权限

```shell
# 中间是大写的R，不是小写
chmod -R 777 my/
```

### 三、修改所属用户和用户组：chown

这个和修改文件夹的权限是基本相同的，只不过是把chmod命令换成了chown。

#### 1、修改文件所属用户和用户组

```shell
# 修改a.txt文件所属用户（jay）和用户组（fefjay）
chown jay:fefjay a.txt
```

#### 2、修改文件夹所属用户和用户组

- 只改文件夹本身所属用户和用户组，不改子文件（夹）

```shell
chown redis:redis /var/lib/redis
```

- 改变文件夹及所有子文件（夹）所属用户和用户组

```shell
chown -R redis:redis /var/lib/redis

# 举例
chown -R mongo:mongo /var/lib/mongo
chown -R mongodb:mongodb /var/lib/mongodb
```
