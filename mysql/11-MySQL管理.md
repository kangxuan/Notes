# MySQL自带的数据库

| 数据库                | 含义                                                    |
| ------------------ | ----------------------------------------------------- |
| mysql              | 存储MySQL服务器正常运行所需要的各种信息（时区、主从、用户、权限等）                  |
| information_schema | 提供访问数据库元数据的各种表和视图，包含数据库、表、字段类型以及访问权限等                 |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于收集数据库服务器性能参数            |
| sys                | 包含了一系列方便DBA和开发人员利用performance_schema性能数据库进行性能调优和诊断的视图 |

# 常用工具

**mysql**

```sql
# mysql [option] [database]
# 选项
# -u, --username 指定用户名
# -p, --password 指定密码
# -h, --host=name 指定服务器IP或域名
# -P, --port=port 指定连接端口
# -e, --execute=... 执行SQL语句并退出

# 直接连接并执行语句并退出
root@d28f2bd07e1a:/# mysql -hlocalhost -uroot -p123456 test1 -e "select * from account";
mysql: [Warning] Using a password on the command line interface can be insecure.
+----+--------+---------+-----------+
| id | name   | money   | avg_money |
+----+--------+---------+-----------+
|  1 | ??     | 1000.00 |      1000 |
|  2 | ??     | 3000.00 |      3000 |
|  3 | shanla | 4000.00 |      NULL |
+----+--------+---------+-----------+
```

**mysqladmin**

是一个执行管理操作的客户端程序。可以用它来检查服务器的配置和状态，删除数据库等操作。

```sql
# mysqladmin [option] command ...
# 选项
# -u, --user=name 指定用户名
# -p, --password 指定密码
# -h, --host=name 指定服务器IP或域名
# -P, --port=port 指定连接端口
```

**mysqlbinlog**

查看binlog日志工具

```sql
# mysqlbinlog [options] log-files1 log-file2 ...
# 选项
# -d, --database=name 指定数据库名称，只列出指定的数据库相关操作
# -o, --offset=# 忽略日志中的前n行命令
# -r, --result-file=name 将输出的文本格式日志输出到指定的文件
# -s, --short-form 显示简单格式，省略掉一些信息
# --start-datatime=date1 --stop-datetime=date2 指定日期间隔内的所有日志
# --start-position=pos1 --stop-position=pos2 指定位置间隔内的所有日志

# 以简单格式查看binlog日志
mysqlbinlog -s binlog.000009
```

**mysqlshow**

mysqlshow客户端对象查找工具，用来很快地查找存在哪些数据库、数据中的表、表中的列或索引等。

**mysqldump**

mysqldump 客户端工具用来备份数据库或在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。

```sql
# mysqldump [options] db_name [tables]
# mysqldump [options] --database/-B db1 [db2 db3...]
# mysqldump [options] --all-databases/-A

# 备份数据库
mysqldump -uroot -p123456 db01 > db01.sql
# 备份db01数据库中的表数据，不备份表结构(-t)
mysqldump -uroot -p123456 -t db01 > db01.sql
# 将db01数据库的表的表结构与数据分开备份(-T)
mysqldump -uroot -p123456 -T /var/lib/mysql-files/ db01 score
```

**mysqlimport/source**

导入数据库

```sql
mysqlimport -uroot -p123456 test /tmp/city.txt

# 还可以使用source，在mysql中执行

```


