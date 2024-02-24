# Redis内存相关话题

**Redis默认内存是多少，如何查看设置？**

- 查看Redis最大占用内存
  
  - Redis最大占用内存在redis.conf配置中，
  
  ```tsconfig
  ############################## MEMORY MANAGEMENT ################################
  
  # Set a memory usage limit to the specified amount of bytes.
  # When the memory limit is reached Redis will try to remove keys
  # according to the eviction policy selected (see maxmemory-policy).
  #
  # If Redis can't remove keys according to the policy, or if the policy is
  # set to 'noeviction', Redis will start to reply with errors to commands
  # that would use more memory, like SET, LPUSH, and so on, and will continue
  # to reply to read-only commands like GET.
  #
  # This option is usually useful when using Redis as an LRU or LFU cache, or to
  # set a hard memory limit for an instance (using the 'noeviction' policy).
  #
  # WARNING: If you have replicas attached to an instance with maxmemory on,
  # the size of the output buffers needed to feed the replicas are subtracted
  # from the used memory count, so that network problems / resyncs will
  # not trigger a loop where keys are evicted, and in turn the output
  # buffer of replicas is full with DELs of keys evicted triggering the deletion
  # of more keys, and so forth until the database is completely emptied.
  #
  # In short... if you have replicas attached it is suggested that you set a lower
  # limit for maxmemory so that there is some free RAM on the system for replica
  # output buffers (but this is not needed if the policy is 'noeviction').
  #
  # maxmemory <bytes>
  ```
  
  - 查看内存的命令
  
  ```shell
  # 查看最大内存设置
  127.0.0.1:6379> config get maxmemory
  1) "maxmemory"
  2) "0"
  # 查看内存使用情况
  127.0.0.1:6379> info memory
  # Memory
  used_memory:80537592
  used_memory_human:76.81M
  used_memory_rss:87949312
  used_memory_rss_human:83.88M
  used_memory_peak:80692192
  used_memory_peak_human:76.95M
  used_memory_peak_perc:99.81%
  used_memory_overhead:49250200
  used_memory_startup:859320
  used_memory_dataset:31287392
  used_memory_dataset_perc:39.27%
  allocator_allocated:80757592
  allocator_active:81084416
  allocator_resident:85184512
  total_system_memory:1041592320
  total_system_memory_human:993.34M
  used_memory_lua:31744
  used_memory_vm_eval:31744
  used_memory_lua_human:31.00K
  used_memory_scripts_eval:0
  number_of_cached_scripts:0
  number_of_functions:0
  number_of_libraries:0
  used_memory_vm_functions:32768
  used_memory_vm_total:64512
  used_memory_vm_total_human:63.00K
  used_memory_functions:184
  used_memory_scripts:184
  used_memory_scripts_human:184B
  maxmemory:0
  maxmemory_human:0B
  maxmemory_policy:noeviction
  allocator_frag_ratio:1.00
  allocator_frag_bytes:326824
  allocator_rss_ratio:1.05
  allocator_rss_bytes:4100096
  rss_overhead_ratio:1.03
  rss_overhead_bytes:2764800
  mem_fragmentation_ratio:1.09
  mem_fragmentation_bytes:7432392
  mem_not_counted_for_evict:8
  mem_replication_backlog:0
  mem_total_replication_buffers:0
  mem_clients_slaves:0
  mem_clients_normal:1800
  mem_cluster_links:0
  mem_aof_buffer:8
  mem_allocator:jemalloc-5.2.1
  active_defrag_running:0
  lazyfree_pending_objects:0
  lazyfreed_objects:0
  ```

- Redis默认内存是多少
  
  - 默认最大内存是”0“，在64位操作系统是不限制内存大小，在32位操作系统最多使用3GB。

- 一般生产上如何配置
  
  - 一般推荐设置内存为最大的物理内存的3/4，给操作系统留有一定的余地。
  
  - 操作系统挂了啥也不是。

- 如何修改Redis的内存设置
  
  - 通过配置文件修改
  
  - 通过命令修改
  
  ```shell
  # 1是一个字节
  127.0.0.1:6379> config set maxmemory 1
  OK
  ```

- 如果Redis内存打满后会出什么错

```shell
127.0.0.1:6379> set k1 v111
(error) OOM command not allowed when used memory > 'maxmemory'.
```

# 过期键如何被删除的

**三种思路**

- 立即删除
  
  - 就是一旦过期立马删除
  
  - 好处是内存利用率很高，一旦出现过期则立马删除
  
  - 坏处是CPU不友好，CPU总是高度运转，无时无刻的在遍历过期数据进行删除，一旦遇到需要高频使用CPU的情况就有出现问题。

- 惰性删除
  
  - 过期不删除，等查询的时候再删除
  
  - 好处是对CPU比较友好，不用无时无刻关注
  
  - 坏处是对内存不友好，如果Redis中存在非常多的过期键，而这些过期键又恰好没有被访问到，那么不会被删除，会导致大量的内存浪费，相当于一种内存泄漏。

- 定期删除
  
  - 择中选择，定期抽样，判断是否过期
  
  - 好处是达到了一种内存和CPU的平衡
  
  - 需要定好删除策略，不然依然会有漏网之鱼

# Redis缓存淘汰策略

先看配置，官方提供了8中淘汰策略。

```tsconfig
# MAXMEMORY POLICY: how Redis will select what to remove when maxmemory
# is reached. You can select one from the following behaviors:
#
# volatile-lru -> Evict using approximated LRU, only keys with an expire set.
# allkeys-lru -> Evict any key using approximated LRU.
# volatile-lfu -> Evict using approximated LFU, only keys with an expire set.
# allkeys-lfu -> Evict any key using approximated LFU.
# volatile-random -> Remove a random key having an expire set.
# allkeys-random -> Remove a random key, any key.
# volatile-ttl -> Remove the key with the nearest expire time (minor TTL)
# noeviction -> Don't evict anything, just return an error on write operations.
#
# LRU means Least Recently Used
# LFU means Least Frequently Used
#
# Both LRU, LFU and volatile-ttl are implemented using approximated
# randomized algorithms.
#
# Note: with any of the above policies, when there are no suitable keys for
# eviction, Redis will return an error on write operations that require
# more memory. These are usually commands that create new keys, add data or
# modify existing keys. A few examples are: SET, INCR, HSET, LPUSH, SUNIONSTORE,
# SORT (due to the STORE argument), and EXEC (if the transaction includes any
# command that requires memory).
#
# The default is:
#
# maxmemory-policy noeviction
```

**分类**

- 2个维度
  
  - 过期键中筛选
  
  - 所有键中筛选

- 4个方面
  
  - LRU
    
    - 最近最少使用页面置换算法，即淘汰最长时间未被使用的页面。
  
  - LFU
    
    - 最近最不常用页面置换算法，即淘汰一定时期内访问次数最少的页面。
  
  - random
    
    - 随机的意思
  
  - ttl
    
    - 删除马上要过期的key

- 8个选项
  
  - volatile-lru，对所有设置了过期的key使用LRU算法进行删除。
  
  - allkeys-lru，对所有key使用LRU算法进行删除。
  
  - volatile-lfu，对所有设置了过期的key使用LFU算法进行删除。
  
  - allkeys-lfu，对所有key使用LFU算法进行删除。
  
  - volatile-random，对所有设置了过期的key使用随机删除。
  
  - allkeys-random，对所有key使用随机删除。
  
  - volatile-ttl，删除马上要过期的key。
  
  - noeviction，不删除任何key

**生产上推荐**

不确定使用什么策略的情况下推荐使用allkeys-lru。
