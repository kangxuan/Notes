1. 如何设计一个秒杀系统，保证商品不超卖？

```
setnx加锁
```

2. Redis有哪些数据类型？底层分别是什么数据结构？分别有什么运用场景？

```
redis有哪些数据？
1. string：字符串
    常见命令：get、set、mset、mget、setex、setnx、incr、decr、incrby、decrby、
            strlen、getset
    运用：普通数据缓存、无限点赞（incr）、分布式锁（setnx）
    底层：string
2. list：有序序列，单key多value，典型的双向链表，从左/右都可以插入，如果key不存在，
则会新建key，移除key后，如果没有了则删除key。
    常见命令：lpush、rpush、lrange、lpop、rpop、lindex（按照索引下标获取元素）、
llen、lrem
    运用：队列、发布订阅
    底层：list
3. hash：hash，典型的k-v模式，说的是vlaue也是k-v模式。
    常见命令：hset、hget、hmget、hgetall（获取所有值）、hlen、hkeys、hvals、
hincrby、hsetnx
    运用：添加购物车（hset）、增加商品数量（hincrby）
    底层：hash
4. set：集合，无序集合不会重复
    常见命令：sadd、smembers、sismember（是否在集合中）、srem、scard（统计数量）、
srandmember(随机)、spop、sdiff、sunion、sinter（求交集）
    运用：抽奖小程序、朋友圈查看共同好友
    底层：set
5. zset: 有序集合，在set基础上加了一个score
    常见命令：zadd、zrange（从小到大）、zrem、zrevrange（从大到小）、zrangebyscore、
zscore、zcard、zcount、zincrby
    运用：排行榜
    底层：zset
6. bitmap：bitmap是由0/1状态表现的二进制位的bit数组，是string类型作为底层结构实现
的一种统计二值状态的数据类型
    常见命令：setbit、getbit、bitcount、strlen、bitop
    运用：签到、布隆过滤器
    底层：string
7. hyperloglog：针对数据去重的基数统计，是专门用来做基数统计的算法，和集合对比，HyperLogLog
不存储元素本身，比集合省资源。
    运用：UV统计
    底层：string
8. geo：GEO主要是为了存储计算经纬度的数据类型，底层是一个zset，只是前面的score变成了经纬度
    常见命令：geoadd、geopos、geohash、geodist、georadius
    运用：经纬度距离计算
    底层：zset
9. stream：
    运用：消息队列
    底层：stream
```

3. 为什么要使用redis

```
1. 高性能
2. 高并发
```

4. redis有哪些高级功能

```
1. 管道（pipeline）导入到批量数据，可以用于热点数据初始化（主要是减少网络连接的时间）
    cat pipe.text | redis-cli -a 123 --pipe
2. 分布式锁
3. watch
4. 事务
5. 持久化
6. 主从、哨兵、集群
```

5. 什么缓存雪崩、缓存穿透、缓存击穿，分别该如何解决？

```
缓存雪崩：
1. 一个是redis服务挂了，导致不能提供服务，MySQL直面冲击
2. 大批量缓存集中失效，导致MySQL直面冲击。
解决方案：
1. 针对第一个主要采用主从、哨兵，集群来解决
2. 给每个key增加一个过期时间时，加一个随机数，避免集中失效。
缓存穿透：
是别人抓住不存在的key导致，redis和MySQL中都没有数据，导致MySQL直面冲击
解决方案：
简单处理：当出现redis和MySQL中都不存在的key，设置一个很短时间的缓存
复杂处理：布隆过滤器，就是将存在key放入到bitmap中，当请求时先判断是否存在布隆过滤器中，
如果布隆过滤器不存在则一定不存在，但有一个问题就是hash冲突，通过多个hash来减少误判。
缓存击穿：
当高并发的情况下，某一个热点key突然失效，会导致MySQL直面冲击
解决方案：
1. 双检加锁策略
2. 多级缓存策略
```

6. redis为什么快？

```
1. 内存访问
2. 单进程避免了上下文切换（采用IO多路复用，一个进程就能搞定成千上万的连接请求）
3. 渐进式rehash
redis中k-v是通过一张全局的hash表，如果key越来越多就会出现对数据进行扩容，如果立马对所
有的key要重新hash进行移动，会导致阻塞，redis采用了渐进式rehash，一次只处理一个key以及
key上的数据，这样就分摊了集中rehash的时间。
4. 缓存时间戳，redis中大量用到ttl，如果按照传统获取时间戳是要调用linux的内置函数，
这会导致时间浪费，redis会启动一个定时任务，将时间戳进行缓存，用的时候直接拿就好了。
```

7. redis有哪些应用场景？

```
1. 缓存
2. 计数器 incr/incrby、decr/decrby
3. 分布式会话 登录保持会话，授权会话
4. 排行榜 zset
5. 购物车 hash
6. 分布式锁 setnx
7. 消息队列 list/stream
```

8. BigKey如何删除？

```
对于string 是unlink，异步删除
对于
```

9. 单进程和多进程

```
redis4.0之后慢慢开始支持多线程，但多线程主要用来处理网络请求，提高并发性，
但实际命令还是采用的单进程
```

10. redis的过期键如何删除的？

```
redis因为涉及到ttl过期时间，所以说过期数据是非常多的。
对于大批量的过期数据redis采用了两种方式：
1. 定时任务删除，采用了贪心算法进行删除，肯定是有漏网之鱼的，
2. 惰性删除，当获取key时，发现这个数据已经过期，立马删除，弥补上面的漏网之鱼。
```

11. redis中的缓存淘汰策略？

```
两个维度：
1. 过期键中删除
2. 全部键中删除
四方面：
1. LRU：淘汰最长时间未使用的
2. LFU：淘汰一定时期内最不常用的
3. random：随机淘汰
4. ttl：淘汰马上过期的

8种算法：
volatile-lru：过期键中淘汰最长未使用的
allkeys-lru：全部键中淘汰最长未使用的
volatile-lfu: 过期键中淘汰不常用的
allkeys-lfu: 全部键中淘汰最不常用的
volatile-random: 过期键中随机淘汰
allkeys-random: 全部键中随机淘汰
volatile-ttl: 过期键中淘汰将要过期的
allkeys-ttl: 全部键中淘汰将要过期的
```

12. redis中如何做持久化？

```
RDB：是定时将数据放入磁盘，全量数据。
AOF：记录每个写操作语句，用于重放。
    AOF的三种回写磁盘机制：
    Always：立即同步回写，优势是不丢数据，劣势是同步回写影响性能
    everysec：每秒回写，写操作后将命令丢到AOF缓冲区，然后异步每秒回写磁盘，优势是
不影响性能，劣势是可能导致1秒的数据丢失
    no：写操作后丢到AOF缓冲区，由操作系统不忙的时候写回磁盘，优势是不影响性能，劣势
是可能丢失很多的数据

线上的如何做靠谱的持久化方案：
1. 开启RDB（稳妥的做法）
2. 开启AOF-everysec，最多丢失1秒内的数据。
```
