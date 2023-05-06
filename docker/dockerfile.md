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
