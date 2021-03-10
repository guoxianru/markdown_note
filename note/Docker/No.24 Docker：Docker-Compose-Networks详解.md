# No.24 Docker：Docker-Compose-Networks详解

## 导读

> Networks可以使不同的应用程序在相同的网络中运行，解决网络隔离问题。

### 1、未显式声明网络环境的docker-compose.yml

例如，在目录app下创建docker-compose.yml，内容如下：

```shell
version: '3'
services:
  web:
    mage: nginx:latest
    container_name: web
    depends_on:
      - db
    ports:
      - "9090:80"
    links:
      - db
  db:
    image: mysql
    container_name: db
```

使用docker-compose up启动容器后，这些容器都会被加入app_default网络中。

```shell
# 查看网络列表
docker network ls

# 查看对应网络的配置
docker network inspect container_id

# 输出
NETWORK ID          NAME                     DRIVER              SCOPE
6f5d9bc0b0a0        app_default              bridge              local
0fb4027b4f6d        bridge                   bridge              local
567f333b9de8        docker-compose_default   bridge              local
bb346324162a        host                     host                local
a4de711f6915        mysql_app                bridge              local
f6c79184ed27        mysql_default            bridge              local
6358d9d60e8a        none                     null                local
```

### 2、networks关键字指定自定义网络

例如下面的docker-compose.yml文件，定义了front和back网络，实现了网络隔离。

其中proxy和db之间只能通过app来实现通信。

其中，custom-driver-1并不能直接使用，你应该替换为host, bridge, overlay等选项中的一种。

```shell
version: '3'

services:
  proxy:
    build: ./proxy
    networks:
      - front
  app:
    build: ./app
    networks:
      - front
      - back
  db:
    image: postgres
    networks:
      - back

networks:
  front:
    # Use a custom driver
    driver: custom-driver-1
  back:
    # Use a custom driver which takes special options
    driver: custom-driver-2
    driver_opts:
      foo: "1"
      bar: "2"
```

值得注意的是，这里定义了back和front两个网络，似乎它们的名字就定义成了back和font，但是你使用docker network ls命令并不能找到它们。

假如你是在myApp目录下运行的docker-compose up命令，那么这两个网络应该分别对应myApp_back和myApp_front。

### 3、配置默认网络

```shell
version: '2'

services:
  web:
    build: .
    ports:
      - "8000:8000"
  db:
    image: postgres

networks:
  default:
    # Use a custom driver
    driver: custom-driver-1
```

### 4、使用已存在的网络

```shell
networks:
  default:
    external:
      name: my-pre-existing-network
```

假如项目包含了两个docker-compose.yml，且使用了links选项，必须使用networks配置。

一个docker-compose.yml用于启动mysql服务，位于mysql/目录下：

```shell
version: "3"

services:
  dbmaster:
    image: master/mysql:latest
    container_name: dbmaster
    ports:
      - "3308:3306"
    volumes:
      - $HOME/Work/data/dbmaster:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: master
    logging:
      driver: "json-file"
      options:
        max-size: "1000k"
        max-file: "20"
    networks:
      - app

  dbslave:
    image: slave/mysql:latest
    container_name: dbslave
    ports:
      - "3309:3306"
    depends_on:
      - dbmaster
    volumes:
      - $HOME/Work/data/dbslave:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: slave
    logging:
      driver: "json-file"
      options:
        max-size: "1000k"
        max-file: "20"
    links:
      - dbmaster
    networks:
      - app

networks:
   default:
    external:
      name: app
```

另一个docker-compose.yml用于启动服务程序，位于cloudgo/目录下：

```shell
version: "3"

services:
  web:
    image: nginx:latest
    container_name: web
    depends_on:
      - cloudgo
    ports:
      - "9090:80"
    volumes:
      - $HOME/Work/docker/docker-compose/nginx/conf.d:/etc/nginx/conf.d
    links:
      - cloudgot
    logging:
      driver: "json-file"
      options:
        max-size: "1000k"
        max-file: "20"
    networks:
      - app

  cloudgo:
    image: cloudgo:latest
    container_name: cloudgo
    ports:
      - "8080:8080"
    logging:
      driver: "json-file"
      options:
        max-size: "1000k"
        max-file: "20"
    external_links:
      - dbmaster
      - dbslave
    networks:
      - app

networks:
  app:
    external: true
```

使用预先创建的网络，然后把他们加入这个已经创建好的网络，从而实现通信。

```shell
docker network create app
```

运行编写好的docker-compose.yml文件，首先运行启动mysql的配置文件，结果如下：

```shell
l$ docker-compose up
ERROR: Service "dbmaster" uses an undefined network "app"
```

明明已经创建好了，却还是报了错，说该网络未定义。尝试改变名称mysql_app，但是依旧报出同样的错误。最终证明，这种方法无法实现，至今没有找到官方文档给出的例子。

所以，最终将第一个docker-compose.yml文件中的networks配置改为如下内容：

```shell
networks:
   mysql_app:
     driver: bridge
```

在这个文件中定义一个网络，以便在后面使用。这里修改完毕，该文件其他地方凡是引用到了该网络的地方均要作出相同的修改。同样，第二个文件也一样。

### 5、其他的一些用法

使用aliases代替link，一般的使用格式如下：

```shell
services:
  some-service:
    networks:
      some-network:
        aliases:
         - alias1
         - alias3
      other-network:
        aliases:
         - alias2
```

在下面的例子中，web容器可以直接通过database:3306或者db:3306访问db容器了。

它们同时属于一个网络，并且db设置了主机别名，所以这样的访问方式是完全可以的。

```shell
version: '2'

services:
  web:
    depends_on:
      - worker
    networks:
      - new

  worker:
    depends_on:
      - db
    networks:
      - legacy

  db:
    image: mysql
    networks:
      new:
        aliases:
          - database
      legacy:
        aliases:
          - mysql

networks:
  new:
  legacy:
```

此时直接使用depends_on已经不再需要link，如果woker需要访问db,可以直接通mysql:port的方式。

### 6、使用networks的要点

1. 注意自定义网络的方式。

2. 注意docker-compose.yml文件的位置与网络默认命名的关系。

3. 注意遇到问题尝试几种替代方式去解决。
