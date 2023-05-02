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

**遇到的问题**

- `yum install`时遇到这样的报错：`Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: No URLs in mirrorlist`，原因是： CentOS 已经停止维护的问题。2020 年 12 月 8 号，CentOS 官方宣布了停止维护 CentOS Linux 的计划，并推出了 CentOS Stream 项目，CentOS Linux 8 作为 RHEL 8 的复刻版本，生命周期缩短，于 2021 年 12 月 31 日停止更新并停止维护（EOL），更多的信息可以查看 CentOS 官方公告。如果需要更新 CentOS，需要将镜像从 [mirror.centos.org](http://mirror.centos.org/) 更改为 [vault.centos.org](https://vault.centos.org/)

```shell
sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
```

**参考文献**

[centos镜像_centos下载地址_centos安装教程-阿里巴巴开源镜像站](https://developer.aliyun.com/mirror/centos?spm=a2c6h.13651102.0.0.3e221b11kNuBTt)


