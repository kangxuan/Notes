# 3主3从Redis集群搭建

**理论**



哈希槽算法

**新建6个Redis容器实例**

```shell
# --net host 使用宿主机的IP和端口，默认
# --privileged=true 获取宿主机root用户权限
# --cluster-enabled yes 开启redis集群
# --appendonly yes 开启持久化
# --port  redis端口号
docker run -d --name redis-node-1 \
--net host \
--privileged=true \
-v /Users/kx/workspace/docker/redis-node-1:/data \
redis:6.0.8 \
--cluster-enabled yes \
--appendonly yes \
--port 6381

docker run -d --name redis-node-2 --net host --privileged=true -v /Users/kx/workspace/docker/redis-node-2:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6382

docker run -d --name redis-node-3 --net host --privileged=true -v /Users/kx/workspace/docker/redis-node-3:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6383

docker run -d --name redis-node-4 --net host --privileged=true -v /Users/kx/workspace/docker/redis-node-4:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6384

docker run -d --name redis-node-5 --net host --privileged=true -v /Users/kx/workspace/docker/redis-node-5:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6385

docker run -d --name redis-node-6 --net host --privileged=true -v /Users/kx/workspace/docker/redis-node-6:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6386


# 查看结果
docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                               NAMES
247d4ed2c980   redis:6.0.8   "docker-entrypoint.s…"   3 seconds ago    Up 2 seconds                                        redis-node-6
832aa8ec8477   redis:6.0.8   "docker-entrypoint.s…"   4 seconds ago    Up 2 seconds                                        redis-node-5
e1b682c50c63   redis:6.0.8   "docker-entrypoint.s…"   4 seconds ago    Up 3 seconds                                        redis-node-4
5d385d8361e6   redis:6.0.8   "docker-entrypoint.s…"   5 seconds ago    Up 3 seconds                                        redis-node-3
4d432ad569b0   redis:6.0.8   "docker-entrypoint.s…"   5 seconds ago    Up 4 seconds                                        redis-node-2
22eaf1978bcf   redis:6.0.8   "docker-entrypoint.s…"   24 seconds ago   Up 22 seconds                                       redis-node-1
```

**进入其中一台Redis构建集群关系**

```shell
# 进入redis-node-1容器
docker exec -it redis-node-1 /bin/bash

# 创建主从关系
# --cluster create 创建集群
# --cluster-replicas 1 表示为每个master创建一个slave节点
# IP为宿主机的IP
# redis-cli --cluster create 192.168.6.14:6381 192.168.6.14:6382 192.168.6.14:6383 192.168.6.14:6384 192.168.6.14:6385 192.168.6.14:6386 --cluster-replicas 1
redis-cli --cluster create 127.0.0.1:6381 127.0.0.1:6382 127.0.0.1:6383 127.0.0.1:6384 127.0.0.1:6385 127.0.0.1:6386 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 127.0.0.1:6385 to 127.0.0.1:6381
Adding replica 127.0.0.1:6386 to 127.0.0.1:6382
Adding replica 127.0.0.1:6384 to 127.0.0.1:6383
>>> Trying to optimize slaves allocation for anti-affinity
[WARNING] Some slaves are in the same host as their master
M: e9f6799de802f01ef0a001e02f7fc63b76ec003b 127.0.0.1:6381
   slots:[0-5460] (5461 slots) master
M: d38ee9755d159524733c8cdadf142ef641deb1c5 127.0.0.1:6382
   slots:[5461-10922] (5462 slots) master
M: 587a8c64afbe43f080aa2ae7cd293c07fb08713b 127.0.0.1:6383
   slots:[10923-16383] (5461 slots) master
S: 0a70839c1edb064753f820623e7ab17e9fd19992 127.0.0.1:6384
   replicates d38ee9755d159524733c8cdadf142ef641deb1c5
S: 7dc5673f6f5cdd652395f1dfa7c399c6366ac88e 127.0.0.1:6385
   replicates 587a8c64afbe43f080aa2ae7cd293c07fb08713b
S: 0ee0471c589de48408ff0d3eb897b0daa04b1f77 127.0.0.1:6386
   replicates e9f6799de802f01ef0a001e02f7fc63b76ec003b
Can I set the above configuration? (type 'yes' to accept):yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join

>>> Performing Cluster Check (using node 127.0.0.1:6381)
M: e9f6799de802f01ef0a001e02f7fc63b76ec003b 127.0.0.1:6381
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
S: 0a70839c1edb064753f820623e7ab17e9fd19992 127.0.0.1:6384
   slots: (0 slots) slave
   replicates d38ee9755d159524733c8cdadf142ef641deb1c5
M: d38ee9755d159524733c8cdadf142ef641deb1c5 127.0.0.1:6382
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: 587a8c64afbe43f080aa2ae7cd293c07fb08713b 127.0.0.1:6383
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 0ee0471c589de48408ff0d3eb897b0daa04b1f77 127.0.0.1:6386
   slots: (0 slots) slave
   replicates e9f6799de802f01ef0a001e02f7fc63b76ec003b
S: 7dc5673f6f5cdd652395f1dfa7c399c6366ac88e 127.0.0.1:6385
   slots: (0 slots) slave
   replicates 587a8c64afbe43f080aa2ae7cd293c07fb08713b
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

**查看集群状态**

```shell
# 进入任意一个redis
redis-cli -p 6381
127.0.0.1:6381> keys *
(empty array)
# cluster info 查看集群信息
127.0.0.1:6381> cluster info
cluster_state:ok # 集群状态
cluster_slots_assigned:16384
cluster_slots_ok:16384 # 已启动哈希槽位数量
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6 # 已知的多少个节点
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:99
cluster_stats_messages_pong_sent:100
cluster_stats_messages_sent:199
cluster_stats_messages_ping_received:95
cluster_stats_messages_pong_received:99
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:199

# 查看集群节点，可以看出 6381(master)-6386(slave)、6382(master)-6384(slave)、6383(master)-6385(slave)
127.0.0.1:6381> cluster nodes
e9f6799de802f01ef0a001e02f7fc63b76ec003b 127.0.0.1:6381@16381 myself,master - 0 1683553391000 1 connected 0-5460
0a70839c1edb064753f820623e7ab17e9fd19992 127.0.0.1:6384@16384 slave d38ee9755d159524733c8cdadf142ef641deb1c5 0 1683553393000 2 connected
d38ee9755d159524733c8cdadf142ef641deb1c5 127.0.0.1:6382@16382 master - 0 1683553392000 2 connected 5461-10922
587a8c64afbe43f080aa2ae7cd293c07fb08713b 127.0.0.1:6383@16383 master - 0 1683553393121 3 connected 10923-16383
0ee0471c589de48408ff0d3eb897b0daa04b1f77 127.0.0.1:6386@16386 slave e9f6799de802f01ef0a001e02f7fc63b76ec003b 0 1683553394125 1 connected
7dc5673f6f5cdd652395f1dfa7c399c6366ac88e 127.0.0.1:6385@16385 slave 587a8c64afbe43f080aa2ae7cd293c07fb08713b 0 1683553389087 3 connected
```


