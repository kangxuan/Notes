# Docker-compose

**概念**

Compose是Docker公司推出的软件工具，可以管理多个Docker容器组成一个应用，主要通过一个YAML文件`docker-compose.yml`,定义好多个容器之间的调用关系、先后顺序。然后只需要一个命令即可同时启动/关闭这些容器。

**痛点在哪**

如果需要同时部署好多个服务，根据现有的，只能每个服务单独编写Dockerfile，然后构建镜像、启动容器，这样会非常麻烦。所以需要一个工具进行统一管理，一键启动。

**核心概念**

- 一文件（`docker-compose.yml`）

- 两要素
  
  - 服务，一个个容器实例，比如订单微服务、MySQL容器、Nginx容器等等
  
  - 工程，由一组关联应用容器组成的一个完整的业务单元，在`docker-compose.yml`定义好。

# Docker-compose安装

[官网]([Compose file version 3 reference | Docker Documentation](https://docs.docker.com/compose/compose-file/compose-file-v3/))

[安装]([Overview | Docker Documentation](https://docs.docker.com/compose/install/))

# Docker-compose使用

1. 先编写好Dockerfile定义各个服务并构建出对应的镜像文件

2. 再使用docker-compose.yml定义一个完整的业务单元，编排好整体应用中的各个容器服务

3. 最后执行`docker-compose up`命令启动并运行整个应用程序，完成一键部署。

# Docker-compose常用命令

- 查看帮助

```shell
docker-compose -h
```

- 启动所有docker-compose服务

```shell
docker-compose up
# 启动所有docker-compose服务并后台运行
docker-compose up -d
```

- 停止并删除容器、网络、数据卷、镜像

```shell
docker-compose down
```

- 进入容器实例内部

```shell
docker-compose exec yml里面的服务id /bin/bash
```

- 展示当前docker-compose编排过的运行的所有容器

```shell
docker-compose ps
```

- 展示当前docker-compose编排过的容器进程

```shell
docker-compose top
```

- 查看容器输出日志

```shell
docker-compose logs yml里面的服务id
```

- 检查配置

```shell
docker-compose config
# 检查配置，有问题裁输出
docker-compose config -q
```

- 重启服务

```shell
docker-compose restart
```

- 启动服务

```shell
docker-compose start
```

- 停止服务

```shell
docker-compose stop
```

# compose编排例子

**服务清单**

- 制作项目服务镜像

- MySQL服务

- Redis服务

**制作项目服务镜像**

```shell
# 根据环境go build代码
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o main main.go
```

- 编写Dockerfile

```dockerfile
FROM loads/alpine:3.8

###############################################################################
#                                INSTALLATION
###############################################################################

# 设置固定的项目路径
ENV WORKDIR /app/main

# 添加应用可执行文件，并设置执行权限
ADD ./main   $WORKDIR/main
RUN chmod +x $WORKDIR/main

###############################################################################
#                                   START
###############################################################################
WORKDIR $WORKDIR
CMD ./main
```

- 制作镜像

```shell
docker build -t myblog:1.1 .
```

**docker-compose.yml**

```yml
version: "3"

services:
  myBlogService: # 博客项目服务
    image: myblog:1.1 # 镜像
    container_name: myblog1.1 # 容器名
    ports: # 端口映射
      - "9090:9090"
    volumes: # 数据卷映射
      - /Users/kx/workspace/go/src/myblog-gf/manifest:/app/main/manifest
      - /Users/kx/workspace/go/src/myblog-gf/resource:/app/main/resource
    networks: # 网络，使用同一网络，项目内连接Redis和MySQL通过服务名，不怕IP发生变化
      - my_net
    depends_on: # 依赖Redis和MySQL，所以Redis和MySQL会先执行
      - redis
      - mysql

  redis: # Redis服务
    image: redis:6.0.8
    container_name: redis02 # 容器名不是必须的，不过不定义docker-compose会自动生成
    ports:
      - "6379:6379"
    volumes:
      - /Users/kx/workspace/docker/redis/redis.conf:/etc/redis/redis.conf
      - /Users/kx/workspace/docker/redis/data:/data
    networks:
      - my_net
    command: redis-server /etc/redis/redis.conf # 容器创建后运行的命令

  mysql: # MySQL服务
    image: mysql:5.7
    container_name: mysql02
    environment: # -e
      MYSQL_ROOT_PASSWORD: '123456'
    ports:
      - "3306:3306"
    volumes:
      - /Users/kx/workspace/docker/mysql/log:/var/log/mysql
      - /Users/kx/workspace/docker/mysql/data:/var/lib/mysql
      - /Users/kx/workspace/docker/mysql/conf:/etc/mysql/conf.d
    networks:
      - my_net
    command: --default-authentication-plugin=mysql_native_password #解决外部无法访问

networks: # 创建Docker网络
    my_net:
```

**启动服务**

```shell
docker-compose up
# 或 后台运行
docker-compose up -d

# 结果
✔ Network myblog-gf_my_net  Created  
✔ Container mysql02         Created 
✔ Container redis02         Created 
✔ Container myblog1.1       Created 
```

**停止服务**

```shell
docker-compose stop

# 结果
✔ Container myblog1.1  Stopped 
✔ Container mysql02    Stopped 
✔ Container redis02    Stopped
```
