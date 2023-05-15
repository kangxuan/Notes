# Docker镜像

**镜像是什么**

是一种轻量级、可执行的独立软件包，它包含了软件运行所需要的所有内容，把应用程序和配置依赖打包成一个可交付的运行环境（包括代码、运行时所需的库、环境变量和配置文件等），这个运行环境就叫做镜像文件。

**镜像的底层原理**

镜像就像搭积木一样，将所需的内容一层一层搭起来，所以可以看到`pull`镜像时是一个一个的下载，其基础就是UnionFS(联合文件系统)。

**UnionFS**

- UnionFS是一种分层、轻量级且高性能的文件系统。它支持对文件系统的修改作为一次提交来一层一层的堆叠，同时可以将不同目录挂载到同一个虚拟文件系统下。

- UnionFS是Docker镜像的基础，镜像通过分层来继承。

- 基于基础镜像可以制作各种具体的应用镜像。

- 其特性是一次性同时加载多个文件系统，但从外面看起来只能看到一个文件系统，联合加载会吧各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

**为何要采用分层结构**

- 镜像分层主要是为了共享资源、方便复制迁移，就是为了复用。

- 比如多个镜像都是从base镜像构建出来，那么Docker Host只需在磁盘上保存一份base镜像，同时内存只需要加载一份base镜像，就可以为所有容器服务。

- 镜像的每一层都能被复用共享。

# 改造其他镜像成为一个新的镜像

在基础的centos镜像上安装一个`vim`，并生成一个带`vim`功能的centos镜像。

- 搜索拉取官方的centos

```shell
docker search --limit 5 centos

# 结果
NAME                            DESCRIPTION                                      STARS     OFFICIAL   AUTOMATED
centos                          DEPRECATED; The official build of CentOS.        7571      [OK]
kasmweb/centos-7-desktop        CentOS 7 desktop for Kasm Workspaces             36
bitnami/centos-base-buildpack   Centos base compilation image                    0                    [OK]
bitnami/centos-extras-base                                                       0
couchbase/centos7-systemd       centos7-systemd images with additional debug…   7                    [OK]

# 拉取镜像
docker pull centos
```

- 启动一个centos容器并安装好vim功能

```shell
docker images

# 结果
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
centos          latest    5d0da3dc9764   19 months ago   231MB

# 启动一个交互式容器
docker run -it 5d0da3dc9764 /bin/bash
[root@e2ddfa017373 /] vim 1.txt
# 没有vim功能
bash: vim: command not found

# 安装vim
# yum update 不要执行，会导致磁盘超限
# [root@e2ddfa017373 /] yum update
[root@e2ddfa017373 /] yum install vim
```

- 提交安装了vim的容器成为一个新的镜像

```shell
# docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:标签名
docker commit -m="增加VIM" -a="shanla" 
```

**yum可能会遇到一些问题**

- 网络不通，参考：[https://www.zhihu.com/question/303290256](https://www.zhihu.com/question/303290256)

```shell
# 开启宿主机IP路由转发功能
echo 1 > /proc/sys/net/ipv4/ip_forward
# Docker容器将使用宿主机的网络命名空间，容器中的进程直接暴露在公共网络中
# 具有相同的IP地址，因此有安全风险。
docker run --privileged -tid --net=host 镜像名称
```

- yum安装访问外网缓慢（或403），需要替换成国内镜像，参考yum修改镜像源笔记。

# 镜像仓库管理

**阿里云镜像服务**

- 首先开通阿里云镜像服务、创建命名空间、创建仓库

- 将镜像推送到Registry

```shell
# 先登录远程仓库
docker login --username=*** registry.cn-hangzhou.aliyuncs.com
# 将已有镜像打上符合阿里云镜像标准的tag
docker tag [ImageId] registry.cn-hangzhou.aliyuncs.com/shanla/my_centos:[镜像版本号]
# 推送到仓库
docker push registry.cn-hangzhou.aliyuncs.com/shanla/my_centos:[镜像版本号]
```

- 从Registry拉取仓库

```shell
# 拉取
docker pull registry.cn-hangzhou.aliyuncs.com/shanla/my_centos:1.1

# 结果
docker images
REPOSITORY                                           TAG       IMAGE ID       CREATED         SIZE
registry.cn-hangzhou.aliyuncs.com/shanla/my_centos   1.1       1863ecc1a5ab   47 hours ago    315MB
```

**Docker Registry**

Docker提供了一个镜像专门用于私有仓库管理

- 下载镜像Docker Registry

```shell
docker pull registry
```

- 运行私有库

```shell
# 映射端口，默认情况下仓库被创建在容器的/var/lib/registry，建议用容器数据卷映射方便管理
docker run -d -p 5000:5000 --privileged=true --net=host registry

# 或-v数据卷映射
docker run -d -p 5000:5000 -v xxx:xxx --privileged=true --net=host registry

docker ps
# 结果
CONTAINER ID   IMAGE      COMMAND                   CREATED         STATUS         PORTS                                       NAMES
7b0db17251b1   registry   "/entrypoint.sh /etc…"   3 minutes ago   Up 3 minutes   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   condescending_brahmagupta
```

- 查看私有库有哪些镜像

```shell
curl -XGET http://192.168.33.10:5000/v2/_catalog
```

- 将镜像打上符合私有库标准的tag

```shell
docker tag 1863ecc1a5ab 192.168.33.10:5000/my_centos:1.1

docker images
REPOSITORY                                           TAG       IMAGE ID       CREATED         SIZE
registry.cn-hangzhou.aliyuncs.com/shanla/my_centos   1.1       1863ecc1a5ab   2 days ago      315MB
192.168.33.10:5000/my_centos                         1.1       1863ecc1a5ab   2 days ago      315MB
```

- 修改Docker配置使之能支持http

```shell
vim /etc/docker/daemon.json

# 增加配置
"insecure-registries": ["192.168.33.10:5000"]

# 修改配置后需要重新docker
systemctl restart docker
```

- push到私有库

```shell
docker push 192.168.33.10:5000/my_centos:1.1
```

- 从私有库拉取镜像

```shell
curl -XGET http://192.168.33.10:5000/v2/_catalog

# 结果
{"repositories":["my_centos"]}

# pull
docker pull 192.168.33.10:5000/my_centos:1.1
```

# 容器数据卷

容器数据卷是一个目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于文件联合系统（UnionFS），因此可以提供持续化或共享数据的特性。其目的就是数据的持久化，完全独立于容器的生存周期，不会因为容器被删除而被删除。类似于redis的rdb和aof文件用于持久化数据。

**容器卷特性**

- 数据卷可以在容器之间共享或重用数据

- 卷中的更改立即生效

- 数据卷中的更改不会包含在镜像的更新中。

- 数据卷的生命周期会一直持续到没有容器使用它。

**运行一个带数据卷的容器**

```shell
# docker run -it --privileged=true -v 宿主机目录:容器内目录 镜像名/镜像ID
docker run -it --privileged=true -v /tmp/data_host:/tmp/data_docker --name=centos1 192.168.33.10:5000/my_centos:1.1 /bin/bash
```

- 如果没有对应目录会自动创建

- 在宿主机上创建修改文件对应容器也会有相应的更新，双向同步。

- 注意在容器停止之后，在宿主机上新建文件，重启容器后文件也会同步。

- 对于已存在宿主机的目录，新启动容器，数据卷数据依然会同步到容器对应的目录下。

**查看数据卷是否绑定成功**

```shell
# docker inspect 容器ID
docker inspect 36a6ece76a97

# 这就是容器卷的信息
"Mounts": [
            {
                "Type": "bind", # 类型为bind
                "Source": "/tmp/data_host", # 宿主机的目录
                "Destination": "/tmp/data_docker", # 容器内目录
                "Mode": "",
                "RW": true, # 读写，默认是rw，可选项为ro-只读
                "Propagation": "rprivate"
            }
        ],
```

**限制容器对数据卷只读**

```shell
# docker run -it --privileged=true -v 宿主机目录:容器内目录:ro 镜像名/镜像ID
docker run -it --privileged=true -v /tmp/data_host:/tmp/data_docker:ro --name=centos1 192.168.33.10:5000/my_centos:1.1 /bin/bash
```

**数据卷继承**

```shell
# --volumes-from 容器名/容器ID
docker run -it --privileged=true --volumes-from centos2 --name centos3 192.168.33.10:5000/my_centos:1.1 /bin/bash
```

- 如果继承的容器停止或删除数据卷数据依然存在，我感觉数据卷就像软连接一样

# 常见软件安装

**mysql**

- 搜索镜像

```shell
# 如果想安装具体版本需要去镜像官网查看
docker search mysql --limit=4

# 结果
NAME         DESCRIPTION                                      STARS     OFFICIAL   AUTOMATED
mysql        MySQL is a widely used, open-source relation…   14096     [OK]
mariadb      MariaDB Server is a high performing open sou…   5385      [OK]
percona      Percona Server is a fork of the MySQL relati…   606       [OK]
phpmyadmin   phpMyAdmin - A web interface for MySQL and M…   789       [OK]
```

- 拉取镜像

```shell
# 拉取最新
docker pull mysql

# 或 拉取指定版本，去镜像官网查看
docker pull mysql:5.7
```

- 启动容器

简单启动MySQL

```shell
# docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
# 要注意宿主机端口有没有被占用 
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d --privileged=true mysql:5.7
```

持久化数据启动MySQL

```shell
# 注意实战中上面简单方式不满足，需要增加数据卷用于持久化数据
docker run -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d \
--privileged=true \
-v /Users/kx/workspace/docker/mysql/log:/var/log/mysql \ # MySQL日志
-v /Users/kx/workspace/docker/mysql/data:/var/lib/mysql \ # MySQL数据
-v /Users/kx/workspace/docker/mysql/conf:/etc/mysql/conf.d \ # mysql配置
--name=mysql01 \
mysql:5.7

# 通过容器卷管理MySQL配置
vim /Users/kx/workspace/docker/mysql/conf/my.cnf

# 创建my.cnf增加配置
[client]
default_character_set=utf8
[mysqld]
collation_server=utf8_general_ci
character_set_server=utf8

# 配置更新
docker restart d60b8ae990e0

# 即使删除了容器，重新run依然数据依然会恢复
```

- 查看容器

```shell
docker ps

# 结果
CONTAINER ID   IMAGE       COMMAND                   CREATED          STATUS          PORTS                                                  NAMES
a0c1d48d31d8   mysql:5.7   "docker-entrypoint.s…"   49 seconds ago   Up 34 seconds   0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   sleepy_cohen
```

- 进入容器

```shell
docker exec -it a0c1d48d31d8 /bin/bash

# 连接MySQL
mysql -uroot -p123456
```

**redis**

- 拉取镜像

```shell
docker pull redis:6.0.8
```

- 配置数据卷的redis.conf

```shell
# 拷贝一份redis.conf到数据卷种，一定要对应的版本的redis.conf

# 修改redis.conf
vim redis.conf

# 修改如下配置
# 1. 配置密码，打开注释
requirepass 123

# 2. 允许外网访问，注释掉
# bind 127.0.0.1

# 3. 将daemonize配置从yes改成no，因为这个会和docker run -d冲突，导致docker run一直失败
daemonize no

# 4. 开启redis持久化
appendonly yes
```

- 启动容器

```shell
docker run -p 6379:6379 -d --privileged=true \
--name=redis01 \
-v /Users/kx/workspace/docker/redis/redis.conf:/etc/redis/redis.conf \ # 配置文件
-v /Users/kx/workspace/docker/redis/data:/data \ # 数据
redis:6.0.8 \
redis-server /etc/redis/redis.conf # 启动redis服务按数据卷的配置启动
```

- 查看容器

```shell
docker ps

# 结果
CONTAINER ID   IMAGE         COMMAND                  CREATED             STATUS             PORTS                               NAMES
05558f5e69d8   redis:6.0.8   "docker-entrypoint.s…"   4 seconds ago       Up 3 seconds       0.0.0.0:6379->6379/tcp              redis01
```

- 进入容器

```shell
docker exec -it 05558f5e69d8 /bin/bash

redis-cli
127.0.0.1:6379> auth 123
OK
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> get k1
"v1"
```

- 修改数据卷配置并重启容器使之生效

```shell
vim redis.conf

# 将数据库数量变成10
databases=10

# 重启redis容器
docker restart 05558f5e69d8

# 进入redis
docker exec -it 05558f5e69d8 /bin/bash

redis-cli
127.0.0.1:6379> auth 123
OK
127.0.0.1:6379> SELECT 15
(error) ERR DB index is out of range
127.0.0.1:6379> SELECT 10
(error) ERR DB index is out of range
127.0.0.1:6379> SELECT 9
OK
```
