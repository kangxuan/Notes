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
