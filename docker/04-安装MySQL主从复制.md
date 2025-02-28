# 一主一从

**新建MySQL主服务容器**

```shell
docker run -p 3307:3306 --name=mysql_master \
-v /Users/kx/workspace/docker/mysql_master/log:/var/log/mysql \
-v /Users/kx/workspace/docker/mysql_master/data:/var/lib/mysql \
-v /Users/kx/workspace/docker/mysql_master/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7
```

**新增主服务器my.cnf**

```shell
vim /Users/kx/workspace/docker/mysql_master/conf/my.cnf

# 配置内容
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=101
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
```

**重启主服务使配置生效**

```shell
docker restart mysql_master
```

**进入主服务并创建数据同步用户**

```shell
# 进入主服务容器
docker exec -it mysql_master /bin/bash

# 连接MySQL并创建同步用户
mysql -uroot -p123456

# 创建用户
CREATE USER 'slave'@'%' IDENTIFIED BY '123456';
# 赋予权限
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
```

**创建MySQL从服务容器**

```shell
docker run -p 3308:3306 --name=mysql_slave \
-v /Users/kx/workspace/docker/mysql_slave/log:/var/log/mysql \
-v /Users/kx/workspace/docker/mysql_slave/data:/var/lib/mysql \
-v /Users/kx/workspace/docker/mysql_slave/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7
```

**新增从服务my.cnf**

```shell
vim /Users/kx/workspace/docker/mysql_slave/conf/my.cnf

# 新增配置
[mysqld]
## 设置server_id，同一局域网中需要唯一
server_id=102
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能，以备Slave作为其它数据库实例的Master时使用
log-bin=mall-mysql-slave1-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理。
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断。
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
## relay_log配置中继日志
relay_log=mall-mysql-relay-bin
## log_slave_updates表示slave将复制事件写进自己的二进制日志
log_slave_updates=1
## slave设置为只读（具有super权限的用户除外）
read_only=1
```

**重启从服务容器使配置生效**

```shell
docker restart mysql_slave
```

**进入主服务容器查看主从同步状态**

```shell
mysql> show master status;

+-----------------------+----------+--------------+------------------+-------------------+
| File                  | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+-----------------------+----------+--------------+------------------+-------------------+
| mall-mysql-bin.000001 |      154 |              | mysql            |                   |
+-----------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

**进入从服务容器配置主从复制**

```shell
# 进入从服务容器
docker exec -it docker_slave /bin/bash

# 配置主从复制
mysql> change master to master_host='192.168.31.170', master_user='slave', master_password='123456', master_port=3307, master_log_file='mall-mysql-bin.000001', master_log_pos=154, master_connect_retry=30;
# master_host：主数据库的IP地址
# master_port：主数据库的运行端口
# master_user：在主数据库创建的用于同步数据的用户账号
# master_password：在主数据库创建的用于同步数据的用户密码
# master_log_file：指定从数据库要复制数据的日志文件，通过查看主数据的状态，获取File参数
# master_log_pos：指定从数据库从哪个位置开始复制数据，通过查看主数据的状态，获取Position参数
# master_connect_retry：连接失败重试的时间间隔，单位为秒。

# 开启同步
mysql> start slave;

# 查看主从同步状态 Slave_IO_Running 和 Slave_SQL_Running 等于No属于未开启
mysql> show slave status \G;

*************************** 1. row ***************************
               Slave_IO_State:
                  Master_Host: 192.168.31.170
                  Master_User: slave
                  Master_Port: 3307
                Connect_Retry: 30
              Master_Log_File: mall-mysql-bin.000001
          Read_Master_Log_Pos: 154
               Relay_Log_File: mall-mysql-relay-bin.000001
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mall-mysql-bin.000001
             Slave_IO_Running: No
            Slave_SQL_Running: No
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table:
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 154
              Relay_Log_Space: 154
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: NULL
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 0
                  Master_UUID:
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State:
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set:
            Executed_Gtid_Set:
                Auto_Position: 0
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:

# 开启主从同步
mysql> start slave;
# 如果 Slave_IO_Running和Slave_SQL_Running等于Yes则已经开启
mysql> show slave status \G;
```

**测试主从同步**

- 在主数据库中建表写入数据

```shell
mysql> create database db1;
Query OK, 1 row affected (0.02 sec)

mysql> use db1;
Database changed
mysql> create table t1(id int, name varchar(20));
Query OK, 0 rows affected (0.06 sec)

mysql> insert t1 values(1, "hello");
Query OK, 1 row affected (0.03 sec)
```

- 在从数据库中查询

```shell
mysql> use db1;
mysql> select * from t1;
+------+-------+
| id   | name  |
+------+-------+
|    1 | hello |
+------+-------+
1 row in set (0.00 sec)
```
