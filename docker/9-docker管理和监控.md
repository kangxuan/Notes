# Portainer

**安装**

[教程](https://docs.portainer.io/start/install-ce/server/docker)

- 直接run

```shell
docker run -d 
-p 8000:8000 \ # portainer服务端端口
-p 9000:9000 \ # portainer客户端端口
--name portainer \
--restart=always 
-v /var/run/docker.sock:/var/run/docker.sock \
-v /Users/kx/workspace/docker/portainer_data:/data \
portainer/portainer-ce:latest
```

- ps

```shell
docker ps

# 结果
CONTAINER ID   IMAGE                           COMMAND                  CREATED          STATUS          PORTS                                                      NAMES
86a7a54dc169   portainer/portainer-ce:latest   "/portainer"             16 minutes ago   Up 16 minutes   0.0.0.0:8000->8000/tcp, 0.0.0.0:9000->9000/tcp, 9443/tcp   portainer
```

**访问**

http://127.0.0.1:9000

# CIG

**痛点在哪**

docker有一个命令查看容器的资源占用情况

```shell
docker stats

# 结果
CONTAINER ID   NAME        CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O     PIDS
8a37d896cdb9   my_nginx1   0.00%     4.074MiB / 7.675GiB   0.05%     3.37kB / 2.26kB   0B / 12.3kB   5
86a7a54dc169   portainer   0.00%     11.74MiB / 7.675GiB   0.15%     807kB / 7.08MB    0B / 0B       7
371e864755b7   alpine1     0.00%     496KiB / 7.675GiB     0.01%     1.19kB / 0B       16.4kB / 0B   1
```

但`docker stats`统计结果只能是当前宿主机的全部容器，数据资料是实时的，没有地方存储、没有健康指标过线预警等功能。

**是什么**

三个软件的集合

- CAdvisor，用于监控收集

- InfluxDB，用于存储收集来的数据

- Granfana，用于将收集来的数据展示图表

**安装**

通过编排`docker-compose.yml`文件安装

```yml
version: "3.1"
services:
  influxdb:
    image: tutum/influxdb:0.9
    restart: always
    environment:
      - PRE_CREATE_DB=cadvisor
    ports:
      - 8083:8083
      - 8086:8086
    volumes:
      - /Users/kx/workspace/docker/influxdb:/data
  cadvisor:
    image: google/cadvisor
    links:
      - influxdb:influxsrv
    command: 
      - storage_driver=influxdb 
      - storage_driver_db=cadvisor 
      - storage_driver_host=influxsrv:8086
    restart: always
    ports:
      - 8080:8080
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
  grafana:
    user: "104"
    image: grafana/grafana
    restart: always
    links:
      - influxdb:influxsrv
    ports:
      - 3000:3000
    volumes:
      - /Users/kx/workspace/docker/grafana_data:/var/lib/grafana
    environment:
      - HTTP_USER=admin
      - HTTP_PASS=admin
      - INFLUXDB_HOST=influxsrv
      - INFLUXDB_PORT=8086
      - INFLUXDB_NAME=cadvisor
      - INFLUXDB_USER=root
      - INFLUXDB_PASS=root

```
