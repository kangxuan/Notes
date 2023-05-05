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

- 如果继承的容器停止或删除数据卷数据依然存在，我感觉这个数据卷就像软连接一样

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

制作Docker image

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

# 本地制作镜像

**编辑Dockerfile**

```dockerfile
FROM xxx

ENV WORKDIR /app
ADD config.json $WORKDIR/
ADD shorturl $WORKDIR/
CMD /app/shorturl
```

**生成image**

```shell
# short-url:0.0.1 image名称和tag
docker build -t short-url:0.0.1 .
```

**本地运行测试**

```shell
# --name 容器名称
# -p 8001:9999 端口 8001是提供给外部的端口 9999是容器内部的端口
# -v 挂载盘
# short-url:0.0.1 image名称和tag
docker run -itd -p 8001:9999 --name aaa -v /xxx/go/src/shorurl:/config short-url:0.0.1
```

# 将本地镜像推送到远程仓库

**先将本地镜像tag到远程**

```shell
# short-url:0.0.1 本地镜像名称和tag
# xxx.xxx.com/gn_dev/shorturl:0.0.2 远程仓库
docker tag short-url:0.0.1 xxx.xxx.com/gn_dev/shorturl:0.0.2
```

**推送到远程**

```shell
docker push xxx.xxx.com/gn_dev/shorturl:0.0.2
```

**如果是私有仓库则需要登录**

```shell
docker login --username=xxx xxx.xxx.com

# 输入密码
```

# 服务端运行远程镜像

```shell
docker run -itd -p 9998:9999 --restart=always --name short-url-new -v /data/short-url:/config xxx,xxx.com/gn-dev/shorturl:0.0.2
```
