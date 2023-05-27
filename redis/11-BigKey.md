# BigKey如何产生

- 社交类，比如大V粉丝列表

- 汇总统计类，经年累月的积累

# BigKey的危害

- 内存不均，集群迁移困难

- 超时删除，不光是`del`命令，`ttl`也会有问题

- 网络流量阻塞

# BigKey定义

根据实际场景来说，结合《[阿里云Redis使用规范]([阿里云 Redis 开发规范-阿里云开发者社区](https://developer.aliyun.com/article/847324?spm=5176.26934562.main.1.b7d94d9etGSfFj))》，分为string和其他类。

- string类型超过10KB算BigKey

- hash、list、set、zset个数超过5000个算BigKey

# 如何发现BigKey

- bigkeys命令
  
  - 好处：给出每种数据结构Top 1 bigkey，同时给出每种数据类型的键值个数+平均大小
  
  - 想查询大于10kb的所有key，--bigkeys参数就无能为力了

```shell
redis-cli -a 123 --bigkeys
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.

# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).


-------- summary -------

Sampled 0 keys in the keyspace!
Total key length in bytes is 0 (avg len 0.00)


0 hashs with 0 fields (00.00% of keys, avg size 0.00)
0 lists with 0 items (00.00% of keys, avg size 0.00)
0 strings with 0 bytes (00.00% of keys, avg size 0.00)
0 streams with 0 entries (00.00% of keys, avg size 0.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00)

# 推荐
# 每隔 100 条 scan 指令就会休眠 0.1s，ops 就不会剧烈抬升，但是扫描的时间会变长
redis-cli -a 123 --bigkeys -i 0.1
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.

# Scanning the entire keyspace to find biggest keys as well as
# average sizes per key type.  You can use -i 0.1 to sleep 0.1 sec
# per 100 SCAN commands (not usually needed).


-------- summary -------

Sampled 0 keys in the keyspace!
Total key length in bytes is 0 (avg len 0.00)


0 hashs with 0 fields (00.00% of keys, avg size 0.00)
0 lists with 0 items (00.00% of keys, avg size 0.00)
0 strings with 0 bytes (00.00% of keys, avg size 0.00)
0 streams with 0 entries (00.00% of keys, avg size 0.00)
0 sets with 0 members (00.00% of keys, avg size 0.00)
0 zsets with 0 members (00.00% of keys, avg size 0.00)
```

- memory usage key

```shell
127.0.0.1:6379> memory usage k1
```

# 如何删除BigKey

- string

```shell
# 通常采用unlink
127.0.0.1:6379> unlink k1
```

- hash

```shell
# 采用hscan + hdel渐进删除
127.0.0.1:6379> hset user:01 name shanla age 20 score 100
(integer) 3
127.0.0.1:6379> hscan user:01 0
1) "0"
2) 1) "name"
   2) "shanla"
   3) "age"
   4) "20"
   5) "score"
   6) "100"
127.0.0.1:6379> hdel user:01 name age
(integer) 2
127.0.0.1:6379> hscan user:01 0
1) "0"
2) 1) "score"
   2) "100"
```

- list

```shell
# 采用ltrim 渐进删除
127.0.0.1:6379> rpush list 1 2 3 4 5
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
127.0.0.1:6379> ltrim list 0 2
OK
127.0.0.1:6379> lrange list 0 -1
1) "1"
2) "2"
3) "3"
```

- set

```shell
# 采用sscan + srem 渐进删除
127.0.0.1:6379> sadd set2 2 1 2 4 5 6
(integer) 5
127.0.0.1:6379> smembers set2
1) "1"
2) "2"
3) "4"
4) "5"
5) "6"
127.0.0.1:6379> sscan set2 0
1) "0"
2) 1) "1"
   2) "2"
   3) "4"
   4) "5"
   5) "6"
127.0.0.1:6379> srem set2 1 2
(integer) 2
127.0.0.1:6379> sscan set2 0
1) "0"
2) 1) "4"
   2) "5"
   3) "6"
```

- zset

```shell
# 通过zscan + zremrangebyrank 渐进删除
127.0.0.1:6379> zadd salary 20000 z3 30000 l4 40000 w5
(integer) 3
127.0.0.1:6379> zrange salary 0 -1 withscores
1) "z3"
2) "20000"
3) "l4"
4) "30000"
5) "w5"
6) "40000"
127.0.0.1:6379> zscan salary 0
1) "0"
2) 1) "z3"
   2) "20000"
   3) "l4"
   4) "30000"
   5) "w5"
   6) "40000"
127.0.0.1:6379> zremrangebyrank salary 0 1
(integer) 2
127.0.0.1:6379> zrange salary 0 -1 withscores
1) "w5"
2) "40000"
```

# MoreKey

当Redis的key有成百万、上千万的时候，通过`keys *`会导致阻塞等待，进而导致一系列的问题。

```shell
# 向redis灌入数据
for((i=1;i<=100*10000;i++)); do echo "hset setBigKey k$i v$i" >> /tmp/redisBigKeyTest.txt ;done;
# 通过管道写入
cat /tmp/redisTest.txt | redis-cli -a 123 --pipe
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
All data transferred. Waiting for the last reply...
Last reply received from server.
errors: 0, replies: 1000000

# 通过keys *遍历数据，需要20秒，keys是阻塞的，生成环境不能用
127.0.0.1:6379>keys *
...
 999999) "k728255"
1000000) "k526783"
1000001) "k201705"
1000002) "k43579"
1000003) "k68831"
(20.74s)
```

- scan
  
  - string类型`scan cursor [match pattern] [Count count]`
  
  - hash类型用hscan
  
  - set类型用sscan
  
  - zset类型用zscan
  
  - list类型用lrange

```shell
# count 只是大致的数字并不精准
127.0.0.1:6379> scan 0 match * count 10
1) "294912"
2)  1) "k685149"
    2) "k591466"
    3) "k256962"
    4) "k499610"
    5) "k793452"
    6) "k342639"
    7) "k877093"
    8) "k689359"
    9) "k820233"
   10) "k171829"
   11) "k438108"
```

- 禁止生产环境危险命令

```shell
vim redis.conf
# 禁止keys *
rename-command keys ""
# 禁止flushdb
rename-command flushdb ""
# 禁止flushall
rename-command flushall ""
```

# 调优

虽然提供了`unlink`、`flushdb async`、`flushall async`等命令解决`del`、`flushdb`、`flushall`等同步阻塞问题，但Redis内部机制还存在删除操作，比如ttl过期后的删除，所以对应提供了调优配置。

```shell
# 内存满逐出，no为同步，yes为异步
lazyfree-lazy-eviction no
# 过期key删除，no为同步，yes为异步
lazyfree-lazy-expire no
# 内部删除，比如rename srckey destkey时，如果destkey存在需要先删除destkey
lazyfree-lazy-server-del no
# slave接收完RDB文件后清空数据
replica-lazy-flush no
# 设置为yes时，用户用del删除，自动使用unlink
lazyfree-lazy-user-del no
# 设置为yes时，用户使用flushdb、flushall，自动使用flushdb async、flushall async
lazyfree-lazy-user-flush no
```

# 面试题

- 海量数据中查询某一固定前缀的key？
  
  - 通过`scan 0 match prefix*`依次查找，直到返回的游标是0结束
  
  - 最好使用 SCAN 命令进行分批迭代，以避免对Redis性能产生过大的影响。

```shell
127.0.0.1:6379> scan 0 match prefix_*
1) "294912"
2) (empty array)
127.0.0.1:6379> scan 294912 match prefix_*
1) "1015808"
2) (empty array)
127.0.0.1:6379> scan 1015808 match prefix_*
1) "344064"
2) (empty array)
...
# 直到返回0停止
```

- 如何在生产上限制keys*/flushdb/flushall等命令？

```shell
# 调整配置文件
# 禁止keys *
rename-command keys ""
# 禁止flushdb
rename-command flushdb ""
# 禁止flushall
rename-command flushall ""
```

- MEMORY USAGE命令是什么？

- 一般多大算BigKey，如何发现BigKey，如何删除BigKey？

- 针对BigKey你做过哪些调优？惰性释放lazyfree了解过吗？
