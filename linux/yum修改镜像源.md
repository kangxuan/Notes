# yum修改国内镜像源

- 进入yum镜像源路径

```shell
cd /etc/yum.repos.d/
```

- 备份镜像源

```shell
mv CentOS-Base.repo CentOS-Base.repo.backup
```

- 下载国内镜像源文件

```shell
wget http://mirrors.aliyun.com/repo/Centos-7.repo
```

- 更新缓存

```shell
yum clean all && yum makecache
```


