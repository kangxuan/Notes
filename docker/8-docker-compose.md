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


