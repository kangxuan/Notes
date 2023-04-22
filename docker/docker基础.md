# Docker基础概念

**Docker是什么？**

- `Docker`是一个提供集中式平台来管理执行应用程序的软件，它将软件组件包装成一个完整的标准化单元，其中包含了所有要运行的内容。

- `Docker`提供了一种叫`容器`的隔离环境，来运行应用程序的功能。可以在指定的主机同时运行多个容器。占用的内存和资源较少，它是安全的，因为容器是相互隔离。

**Docker的用途**

- 提供一次性环境

- 提供弹性的云服务

- 组建微服务架构

**Docker引擎**

`Docker引擎`是一个包含以下组件的客户端服务器应用程序。

- 一个守护进程。（server docker daemon）
- REST API用于指定程序可以用来与守护进程通信的接口，并指示其做什么。（REST API）
- 一个有命令行界面的客户端。（Client docker CLI）

**Docker架构**

其架构主要分为三部分

- 客户端（Client）：Docker提供命令行界面工具来进行和Docker守护进程交互。客户端可以构建、运行和停止应用程序。客户端还可以远程和Docker_Host进行交互。
- 服务端（Docker_Host）：包含容器、映像和Docker守护进程。提供了完整的环境来执行和运行应用程序。
- 注册表（Registry）：全局映像库，可以访问并使用这些映像在docker环境中运行这些应用程序。

**Docker名词解释**

- `Docker image`：
  - 镜像。
  - 镜像是只读的，里面包含要运行的文件。
  - 镜像是用来创建容器（`container`），是`Docker container`的设计蓝图。
  - `Docker image`可以通过`Dockerfile`创建，也可以在Docker hub/registry上下载。
  - `Docker image`包括`Dockerfile`、依赖和程序的代码。
  - `Dockerfile`文件中包含了一系列的指令来创建`Docker image`。
- `Docker container`：容器。Docker的运行组件，启动一个镜像就是一个容器，多个容器是相互隔离的。保证运行在容器中的程序运行在一个安全的环境中。
- `Docker hub/registry`：共享和管理docker镜像。用户可以往上面上传或从上面下载镜像，[Docker hub官网](https://registry.hub.docker.com/)

# Docker安装

**Linux安装**

`yum`安装

```shell
yum install docker
```

启动Docker

```shell
service docker start

# Redirecting to /bin/systemctl start  docker.service
```

设置开机自启动

```shell
chkconfig docker on
```

**MacOS安装**

进入[官网](https://www.docker.com/)下载软件，安装即可会得到客户端

# 尝试第一个Docker制作过程

以go为例，先编写一个go例子

```go
// filename:floot_type_view.go
package main

import "fmt"

func reverseWithGenerics[T any](s []T) []T {
	l := len(s)
	r := make([]T, l)

	for i, e := range s {
		r[l-i-1] = e
	}
	return r
}

func main() {
	fmt.Println(reverseWithGenerics[int]([]int{1, 2, 3, 4}))
	fmt.Println(reverseWithGenerics[float64]([]float64{0.1, 0.2, 0.3, 0.4}))
}
```

编译一个go可执行文件

```shell
# 将floot_type_view.go编译成一个名为floot_type_view-app的可执行文件
# 注意目标机器的系统
go build -o floot_type_view-app floot_type_view.go
```

编写Dockerfile

```dockerfile
# 导入官方go对应的版本
FROM golang:1.19.3

# 将当前目录下的所有文件（除了.dockerignore排除的路径或文件），都拷贝到 image 文件的 /app 目录里面
COPY . /app

# 指定工作目录为/app
WORKDIR /app

# 将可执行文件添加到工作目录下
ADD floot_type_view-app $WORKDIR/

# 运行go程序
CMD /app/floot_type_view-app
```

制作Docker  image

```shell
# 根据当前目录下的Dockerfile制作Docker image
docker image build -t floot_type_view-app .
# 或
docker build -t floot_type_view-app .
```

查看images

```shell
docker image ls
# 或
docker images
```

运行容器

```shell
docker container run floot_type_view-app
# 或
docker run floot_type_view-app

# 结果
[4 3 2 1]
[0.4 0.3 0.2 0.1]
```



# Docker基础操作

**查看Docker信息**

```shell
docker info
```

**查看Docker版本**

```shell
docker version
```

**修改镜像地址**

官网服务器在国外，可设置中国官方镜像

- docker中国镜像：[registry.docker-cn.com](http://registry.docker-cn.com)

- 阿里云镜像：[阿里巴巴开源镜像站-OPSX镜像站-阿里云开发者社区](https://developer.aliyun.com/mirror/)

修改Docker配置

```shell
# 编辑配置
cd /etc/docker
vim *.json
```

增加如下代码

```json
{                                                                                                                                
    "registry-mirrors": ["https://registry.docker-cn.com"],                                                                      
    "live-restore": true                                                                                                         
}
```

# 镜像操作

**搜索镜像**

```shell
docker search centos
```

结果：

```log
NAME                                         DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
centos                                       The official build of CentOS.                   7158                [OK]                
centos/systemd                               systemd enabled base container.                 108                                     [OK]
centos/mysql-57-centos7                      MySQL 5.7 SQL database server                   94                                      
centos/postgresql-96-centos7                 PostgreSQL is an advanced Object-Relational …   45                                      

NAME：镜像名称
DESCRIPTION：镜像描述
STARS：用户评价，反应一个镜像的受欢迎程度
OFFICIAL：是否是官方镜像
AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程创建的
```

**拉取镜像**

从远程仓库拉取镜像到本地

```shell
docker pull library/hello-world
```

**查看本地镜像**

```shell
docker images

# 或

docker image ls
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
