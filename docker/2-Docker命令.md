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

**拉取镜像**

从远程仓库拉取镜像到本地

```shell
docker pull library/hello-world
```

**删除本地镜像**

```shell
docker rmi docker.io/hello-world
```

# 容器操作

**创建并启动容器**

- 交互式容器
  
  创建启动容器后，直接进入容器（即分配一个伪终端）。exit退出容器后，容器停止工作。

```shell
docker run -it --name=mycentos centos:7 /bin/bash
```

- 守护式容器
  
  以形成一个后台守护进程，针对于需要长期运行的服务容器。

```shell
docker run -id --name=mycentos centos:7 /bin/bash
```

docker run 参数解析

| 参数     | 解释                                          |
| ------ | ------------------------------------------- |
| -i     | 表示运行容器                                      |
| -t     | 表示创建运行容器后直接进入                               |
| --name | 表示容器名称，容器名称不能重复                             |
| -d     | 表示创建启动一个守护式容器                               |
| -p     | 表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个-p做多个端口映射 |
| -v     | 表示目录映射                                      |

**查看容器**

- 查看运行中的容器列表

```shell
docker ps
```

- 查看所有的容器列表

```shell
docker ps -a
```

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

**文件拷贝**

- 将宿主机的文件拷贝到容器内

```shell
# docker cp 文件路径 容器名称:容器内目录
docker cp abc.txt <container_name>:/
```

- 将容器内文件拷贝到宿主机

```shell
# docker cp 容器名称:文件路径 宿主机目录
docker cp <container_name>:/cba.txt 
```

*注意：容器在停止状态下也可以cp*

**进入容器**

```shell
docker exec -it <container_name> /bin/bash
```

**删除容器**

```shell
docker rm <container_name>/<container_id>
```

**挂载目录**

创建容器时，可以将宿主机中的一个目录映射到容器内的一个目录，这样修改了宿主机中的文件就相当于修改了容器中的文件。

可以代码开发文件放在宿主机，运行环境放在容器中。

```shell
docker -id -v 宿主机目录:容器内目录 --name=容器名称 镜像名称:TAG
docker -id -v ~/workspace:/www --name=mycentos2 centos:7

# 这样在宿主机workspace下开发，就直接可以运行调试了。
```

**查看容器内容**

```shell
docker inspect <container_name>
```
