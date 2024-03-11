# 单点部署ES

1. 创建专属网络

```shell
docker network create es-net
```

2. 拉取es

```shell
docker pull elasticsearch:7.12.1

7.12.1: Pulling from library/elasticsearch
7a0437f04f83: Pull complete
ed4a47ec20b2: Pull complete
74e4f4b7e738: Pull complete
ef2a2418a5f4: Pull complete
646dbf47f747: Pull complete
3ffbf21442fc: Pull complete
e04f00c0d464: Pull complete
Digest: sha256:622f854572780281bc85b5fde33be27e99670941ed8b7eea5ba4aaf533fa64ec
Status: Downloaded newer image for elasticsearch:7.12.1
docker.io/library/elasticsearch:7.12.1
```

3. 启动es

```shell
docker run -d \
    --name es \
    -e "ES_JAVA_OPTS=-Xms512m -Xms512m" \ # 配置内存占用大小，默认是1G
    -e "discovery.type=single-node" \ #运行模式：单点
    -v /Users/kx/workspace/docker/es/es-data:/usr/share/elasticsearch/data \ # 配置数据卷：data
    -v /Users/kx/workspace/docker/es/es-plugins:/usr/share/elasticsearch/plugins \ # 配置数据卷：plugins
    --privileged \
    --network=es-net \
    -p 9200:9200 \ # 对内的端口
    -p 9300:9300 \ # 对外的端口
elasticsearch:7.12.1

4069c246fb7f0a996bf930f2f5ec70e010e919af19654aeced3aea8dd6c830a7
```

4. 验证是否最终安装成功

浏览器访问127.0.0.1:9200

# 安装kibana

1. 拉取kibana镜像

```shell
docker pull kibana:7.12.1

7.12.1: Pulling from library/kibana
7a0437f04f83: Already exists
7a86526e9ebe: Pull complete
82bdbe52f520: Pull complete
b3544177b28b: Pull complete
54c4924dea71: Pull complete
62b707397fb8: Pull complete
fe176c615c83: Pull complete
25d370300ee0: Pull complete
a7ddea056c28: Pull complete
8eb9e99363e1: Pull complete
27c6179eff48: Pull complete
419632bc6dfe: Pull complete
0c05d50f8ca5: Pull complete
Digest: sha256:e96f8b6a90db0b4ba804f7023922448a1d752a85e77f6c645ec309fa0328627d
Status: Downloaded newer image for kibana:7.12.1
docker.io/library/kibana:7.12.1
```

2. 运行kibana

```shell
docker run -d \
    --name kibana \
    -e ELASTICSEARCH_HOSTS=http://es:9200 \
    --network=es-net \
    -p 5601:5601 \
kibana:7.12.1
```

# 安装ik分词器

1. 下载对应的ik分词器插件 https://github.com/infinilabs/analysis-ik/releases/tag/v7.12.1

2. 将下载下来的解压放到/Users/kx/workspace/docker/es/es-plugins下。

3. 重启es

```shell
docker restart es
```

## ik分词器的模式

1. `ik_smart`：智能切分，粗粒度

2. `ik_max_word`：最细切分，细粒度

## ik分词器停用词库和拓展词库

1. 编辑ik下的config/IKAnalyzer.cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
	<comment>IK Analyzer 扩展配置</comment>
	<!--用户可以在这里配置自己的扩展字典 -->
	<entry key="ext_dict">ext.dic</entry>
	 <!--用户可以在这里配置自己的扩展停止词字典-->
	<entry key="ext_stopwords">stopword.dic</entry>
	<!--用户可以在这里配置远程扩展字典 -->
	<!-- <entry key="remote_ext_dict">words_location</entry> -->
	<!--用户可以在这里配置远程扩展停止词字典-->
	<!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>

```

2. 新增ext.dic文件，用来拓展词库

```editorconfig
白嫖
奥利给
```

3. 新增/编辑stopword.dic文件，用来增加屏蔽词

```editorconfig
a
an
and
are
as
at
be
but
by
for
if
in
into
is
it
no
not
of
on
or
such
that
the
their
then
there
these
they
this
to
was
will
with
的
了
啊
习大大
```
