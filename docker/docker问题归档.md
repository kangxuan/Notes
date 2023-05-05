# 常见问题

**空间不足问题**

[Docker run出现no space left on device错误的解决_乐事一下的博客-CSDN博客](https://blog.csdn.net/baidu_23433185/article/details/114398301)

```
# docker pull报错”write /var/lib/docker/tmp/GetImageBlob129067360: no space left on device“

# 先查看docker目录还剩多大空间 df -hl /var/lib/docker

[root@localhost vagrant]# df -hl /var/lib/docker
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  8.3G  8.0G  260M  97% /

# 或者 docker system df

[root@localhost vagrant]# docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              1                   1                   203.9MB             0B (0%)
Containers          1                   1                   33B                 0B (0%)
Local Volumes       0                   0                   0B                  0B
Build Cache         0                   0                   0B                  0B


# 解决方案删除无用的镜像或容器
docker rmi ...
```
