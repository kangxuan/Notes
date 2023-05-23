# 帮助启动类

- 启动Docker

```shell
systemctl start docker
```

- 停止Docker

```shell
systemctl stop docker
```

- 重启Docker

```shell
systemctl restart docker
```

- 查看Docker运行状态

```shell
systemctl status docker
```

- 开机启动

```shell
systemctl enable docker
```

- 查看Docker概要信息

```shell
docker info
```

- 查看Docker版本

```shell
docker version
```

- 帮助文档

```shell
docker --help

# 或查看某一个命令的文档
docker run --help
```

# 镜像操作

**查看本机镜像**

```shell
docker images
# 或
docker image ls

# 结果
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    feb5d9fea6a5   19 months ago   13.3kB

# REPOSITORY：镜像的仓库源
# TAG：镜像的标签版本号，类似MySQL的版本 5.6 5.7 8.0
# IMAGE ID：镜像ID，唯一的
# CREATED：创建时间
# SIZE：镜像大小
```

同一个仓库源可以有多个不同的TAG，代表这个仓库源不同的版本，使用`REPOSITORY:TAG`表示不同的镜像，如果不指定版本则默认为`latest`版本。

- `-a`列出所有的镜像（包含历史版本）

- `-q`只显示镜像ID

```shell
# 列出所有的镜像ID
docker images -qa
```

**搜索镜像**

```shell
docker search redis

# 结果
NAME                                         DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
centos                                       The official build of CentOS.                   7158                [OK]                
centos/systemd                               systemd enabled base container.                 108                                     [OK]
centos/mysql-57-centos7                      MySQL 5.7 SQL database server                   94                                      
centos/postgresql-96-centos7                 PostgreSQL is an advanced Object-Relational …   45                                      

# NAME：镜像名称
# DESCRIPTION：镜像描述
# STARS：点赞数
# OFFICIAL：是否是官方镜像
# AUTOMATED：是否自动构建，表示该镜像由Docker Hub自动构建流程创建的
```

- `--limit N`只列出多少条记录，默认是25条

```shell
docker search --limit 5 redis
```

**拉取镜像**

从远程仓库拉取（下载）镜像到本地

```shell
# 默认下载latest版本
docker pull Hello-world

# 下载特定版本
docker pull redis:6.0.8
```

**查看镜像/容器/数据卷占用空间**

```shell
docker system df

# 结果
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          2         1         104.2MB   104.2MB (99%)
Containers      2         0         0B        0B
Local Volumes   0         0         0B        0B
Build Cache     0         0         0B        0B

# TYPE 类型 Images镜像、Containers容器、Local Volumes本地数据卷 Build Cache缓存
# TOTAL 总数
# ACTIVE 激活数
# SIZE 大小
# RECLAIMABLE 可回收的
```

**删除本地镜像**

```shell
# 按仓库源名称删除
docker rmi hello-world

# 或 按IMAGE ID删除
docker rmi feb5d9fea6a5
```

- 强制删除单个镜像

```shell
docker rmi -f feb5d9fea6a5
# 结果
Untagged: hello-world:latest
Untagged: hello-world@sha256:4e83453afed1b4fa1a3500525091dbfca6ce1e66903fd4c01ff015dbcb1ba33e
Deleted: sha256:feb5d9fea6a5e9606aa995e879d862b825965ba48de054caab5ef356dc6b3412
```

- 强制删除多个镜像

```shell
docker rmi -f feb5d9fea6a5 16ecd2772934
# 或
docker rmi -f hello-world:latest redis:6.0.8
```

- 强制删除全部镜像

```shell
# 慎重操作
docker rmi -f $(docker images -qa)
```

**虚悬镜像**

仓库名和版本号都是`none`的镜像。

# 容器操作

**创建并启动容器**

- 交互式容器
  
  创建启动容器后，直接进入容器（即分配一个伪终端）。

```shell
# 以/bin/bash命令模式
docker run -it --name=mycentos centos:7 /bin/bash
```

- 守护式容器
  
  以形成一个后台守护进程，针对于需要长期运行的服务容器。注意针对像`centos`或`nginx`这样的后台运行的应用，没有前台进程运行，当`run -d`后发现没有前端进程没有运行的应用就会自杀退出。

```shell
# 启动ubuntu
docker run -d ubuntu

# ps后发现没有运行中的容器，是因为没有前端进程则会自杀
docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 解决方案：增加交互且后台运行就不会死
docker run -id ubuntu /bin/bash

docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED              STATUS              PORTS      NAMES
247753d1e5ad   ubuntu    "/bin/bash"               9 seconds ago        Up 4 seconds                   myubuntu01

# 启动redis
docker run -d redis

# 类似redis、MySQL这样的容器本身就不需要进行交互则不会自杀
docker ps
CONTAINER ID   IMAGE     COMMAND                   CREATED          STATUS         PORTS      NAMES
060010454a99   redis     "docker-entrypoint.s…"   11 seconds ago   Up 7 seconds   6379/tcp   charming_brahmagupta
```

docker run 参数解析

| 参数     | 解释                                         |
| ------ | ------------------------------------------ |
| -i     | 表示以交互模式运行容器（interactive），通常和t一起使用          |
| -t     | 为容器重新分配一个伪终端（tty），通常和i一起使用                 |
| --name | 表示容器名称，容器名称不能重复，如果不指定Docker会为容器默认一个名称      |
| -d     | 表示以守护式容器运行容器，也就是后台运行运行容器                   |
| -p     | 表示端口映射，前者是宿主机端口，后者是容器的映射端口。可以使用多个-p做多个端口映射 |
| -P     | 端口映射，Docker默认端口                            |
| -v     | 表示目录映射                                     |

**查看容器**

- 查看运行中的容器列表

```shell
docker ps
```

- 查看所有的容器列表

```shell
docker ps -a
```

- 查看最近启动的容器

```shell
docker ps -l
```

- 列出最近N个容器

```shell
docker ps -n 1
```

- 只列出容器ID

```shell
docker ps -q
```

**退出容器**

- `exit`退出，容器退出

- `ctrl+p+q`退出，容器不退出

**启动和停止容器**

- 启动已有容器

```shell
docker start <container_name>/<container_id> # 容器名或容器ID
```

- 停止容器运行

```shell
docker stop <container_name>/<container_id>
```

- 重启容器

```shell
docker restart <container_name>/<container_id>
```

- 强制停止容器

```shell
docker kill <container_name>/<container_id>
```

**删除容器**

注意不能删除正在运行中的容器，必须先停止它。

```shell
docker rm <container_name>/<container_id>
```

- 强制删除容器

```shell
docker rm -f <container_name>/<container_id>
```

- 一次性删除所有容器

```shell
docker rm -f $(docker ps -aq)

#或
docker ps -aq | xargs docker rm -f
```

**进入容器**

- `exec`进入容器，会打开一个新的终端启动新的进程，`exit`后容器不会退出

```shell
docker exec -it <container_name>/<container_id> /bin/bash
```

- `attach`直接进入容器启动命令的终端不会启动新的进程，`exit`后容器会退出

```shell
docker attach <container_name>/<container_id>
```

**查看容器内容**

以`JSON`格式查看容器详细内容

```shell
docker inspect <container_name>/<container_id>
```

**文件拷贝**

- 将宿主机的文件拷贝到容器内

```shell
# docker cp 文件路径 容器名称:容器内目录
docker cp abc.txt <container_name>:/
```

- 将容器内文件拷贝到宿主机

```shell
# docker cp 容器名称:文件路径 宿主机目录
docker cp <container_name>:/cba.txt /
```

*注意：容器在停止状态下也可以cp*

**导出和导入容器**

- `export`导出容器的内容为一个`tar`归档文件

```shell
docker export <container_id> > 文件名.tar

# 例子
docker export d91fb23f17ee > ubuntu.tar
```

- `import`从tar包中的内容创建一个新的文件系统再导入为镜像，再`run`一个容器可以还原容器

```shell
# 先删除容器 d91fb23f17ee
docker stop d91fb23f17ee
docker rm d91fb23f17ee

# 此时已经没有d91fb23f17ee这个容器了
docker ps

# 通过tar包还原 读取ubuntu.tar数据通过docker import导入成一个新的镜像
# docker import - 镜像用户/镜像名:TAG
cat ubuntu.tar | docker import - shanla/ubuntu:3.7 

docker images
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
shanla/ubuntu   3.7       4d5d8df08527   5 seconds ago   72.8MB

# 启动一个新容器
docker run -id 4d5d8df08527 /bin/bash
# ps
docker ps
CONTAINER ID   IMAGE          COMMAND                   CREATED          STATUS          PORTS      NAMES
f03ec825c760   4d5d8df08527   "/bin/bash"               30 seconds ago   Up 28 seconds              vigilant_lalande
# 进入容器会发现以前的数据还在
docker exec -it f03ec825c760 /bin/bash
```

**挂载目录**

创建容器时，可以将宿主机中的一个目录映射到容器内的一个目录，这样修改了宿主机中的文件就相当于修改了容器中的文件。

可以代码开发文件放在宿主机，运行环境放在容器中。

```shell
docker -id -v 宿主机目录:容器内目录 --name=容器名称 镜像名称:TAG
docker -id -v ~/workspace:/www --name=mycentos2 centos:7

# 这样在宿主机workspace下开发，就直接可以运行调试了。
```

**查看容器日志**

当容器异常时可查看容器日志协助排错

```shell
# docker logs 容器ID
docker logs 7c8b72f51d19

*** FATAL CONFIG FILE ERROR (Redis 7.0.0) ***
Reading the configuration file, at line 504
>>> 'dir /data/myredis'
No such file or directory
```
