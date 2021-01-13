# No.28 Docker：基础操作

## 导读

> 运行、操作、拉取、删除。

### 1、运行容器

```shell
docker build -t scrapyd:latest .
docker run -d -p 6800:6800 scrapyd
docker run -i -t python:3.6 /bin/bash
docker run -d -p 4444:4444 selenium/standalone-chrome
docker run -d -p 4444:4444 --shm-size=2g selenium/standalone-chrome
```

### 2、操作容器

```shell
# 查看容器运行日志
docker logs -f 容器ID

# 进入容器
docker exec -it 容器ID /bin/bash

# 退出容器
exit
```

### 4、容器、镜像删除

```shell
# 查看所有容器（container）：
docker ps -a

# 停止所有容器（container），这样才能够删除其中的镜像（images）：
docker stop $(docker ps -a -q)

# 删除容器（container），通过容器（container）的id来指定删除：
docker rm <容器 id>

# 删除所有容器（container）：
docker rm $(docker ps -a -q)

# 查看所有镜像（images）：
docker images -a

# 删除镜像（images），通过镜像（images）的id来指定删除：
docker rmi <镜像 id>

# 删除untagged images，也就是那些id为<None>的image：
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

# 删除全部镜像（images）的话
docker rmi $(docker images -a)
```
