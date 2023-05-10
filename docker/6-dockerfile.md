# Dockerfile概念

**Dockerfile是什么**

Dockerfile是构建Docker镜像的文本文件，是一条条构建镜像所需的指令和参数构成的脚本。

[Dockerfile reference | Docker Documentation](https://docs.docker.com/engine/reference/builder/)

**构建三步骤**

1. 编写Dockerfile文件

2. `docker build`构建镜像

3. `docker run`根据构建出来的镜像运行容器

**Dockerfile规范**

- 每条保留字指令都必须大写且后面必须要跟随至少一个参数

- 指令是按从上到下顺序执行

- `#`表示注释

- 每条指令都会创建一个新的镜像层并对镜像进行提交（一层一层的堆叠）

**docker build内部流程**

1. Docker从基础镜像运行一个容器（也就是`From`）

2. 执行一条指令对容器进行修改（其他指令）

3. 执行类似`docker commit`的操作提交一个新的镜像层

4. Docker再基于刚提交的新的镜像层运行一个容器

5. 循环执行指令直到最后一条

**Dockerfile生命周期**

1. Dockerfile是软件原材料

2. Docker镜像是软件的交付品

3. Docker容器是软件镜像的运行态

# Dockerfile常用指令

- FROM
  
  - 基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是from。

- MAINTAINER
  
  - 镜像维护者的姓名和邮箱地址

- RUN
  
  - 容器构建时需要运行的命令
  
  - 是在终端运行的命令
  
  - 在`docker build`时运行的

- EXPOSE
  
  - 当前容器对外暴露出的端口

- WORKDIR
  
  - 指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

- USER
  
  - 指定该镜像以什么样的用户去执行，如果都不指定，默认是root

- ENV
  
  - 用来在构建镜像过程中设置环境变量

- ADD
  
  - 将宿主机目录下的文件拷贝进镜像且会自动处理URL和解压tar压缩包

- COPY
  
  - 拷贝文件和目录到镜像中
  
  - 将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置

- VOLUME
  
  - 容器数据卷，用于数据保存和持久化工作

- CMD
  
  - 指定容器启动后的要干的事情
  
  - Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 `docker run `之后的参数替换
  
  - CMD是在`docker run` 时运行。

- ENTRYPOINT
  
  - 也是用来指定一个容器启动时要运行的命令
  
  - 类似于 CMD 指令，但是ENTRYPOINT不会被`docker run`后面的命令覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序
  
  - 在执行`docker run`的时候可以指定 ENTRYPOINT 运行所需的参数
  
  - 如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。

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
