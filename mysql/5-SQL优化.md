# 插入数据

**大量数据插入**

- 通过批量插入方法

```sql
insert into student
values (5, '张无忌', '2000100105'),
       (6, '杨逍', '2000100106');
```

- 手动控制事务避免频繁自动提交事务

```sql
start transaction;
insert into student values(7, '范遥', '2000100107');
insert into student values(8, '冷谦', '2000100108');
insert into student values(9, '说不得', '2000100109');
commit;
```

- 主键顺序插入，性能高于乱序插入
  
  - 因为顺序插入不会引起页分裂

**巨量数据插入**

针对于百万级的数据，MySQL提供了工具load指令进行插入

```shell
# 客户端连接服务端时，加上参数 -–local-infile 
mysql –-local-infile -u root -p 
# 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关 
set global local_infile = 1; 
# 执行load指令将准备好的数据，加载到表结构中
# fields terminated by 字段以 "," 分割
# lines terminated by 行以 "\n" 分割
load data local infile '/root/sql1.log' into table tb_user fields terminated by ',' lines terminated by '\n' ;
```

# 主键优化

MySQL数据存储是将一行一行数据存储在page中，当page中存储满了之后会申请开辟一个新的page。

- 按照主键顺序插入会减少页分裂问题，提高效率

- 如果乱序插入就导致页分裂问题，导致效率降低

**主键设计的原则**

- 在满足业务需求的情况下，尽量降低主键的长度，减少磁盘空间的占用。

- 插入数据时，尽量选择顺序插入或ID自增插入。

- 尽量不要使用UUID做主键，太长了。

- 尽量避免修改主键的值，修改主键的值会导致页合并等问题。

- 尽量使用软删除，避免导致页合并问题。

# 排序优化

排序底层有两种方式

| 方式             | 说明                                                  |
| -------------- | --------------------------------------------------- |
| Using filesort | 通过表的索引或全表扫描，读取数据放入到排序缓冲区sort buffer中完成排序操作后，返回给客户端。 |
| Using index    | 通过有序索引顺序扫描直接返回有序数据，不需要额外进行排序，这种方式效率高。               |

首先说明，排序要想优化到Using index，必须使用select 具体的字段且要在索引中出现，而不能使用select *，因为会产生回表，还会是Using filesort。

索引情况

```sql
mysql> show index from tb_user;
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| Table   | Non_unique | Key_name             | Seq_in_index | Column_name | Collation | Cardinality | Sub_part | Packed | Null | Index_type | Comment | Index_comment | Visible | Expression |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
| tb_user |          0 | PRIMARY              |            1 | id          | A         |          24 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| tb_user |          0 | idx_user_phone       |            1 | phone       | A         |          24 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| tb_user |          1 | idx_user_name        |            1 | name        | A         |          24 |     NULL |   NULL |      | BTREE      |         |               | YES     | NULL       |
| tb_user |          1 | idx_user_pro_age_sta |            1 | profession  | A         |          16 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| tb_user |          1 | idx_user_pro_age_sta |            2 | age         | A         |          22 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| tb_user |          1 | idx_user_pro_age_sta |            3 | status      | A         |          24 |     NULL |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
| tb_user |          1 | idx_email_5          |            1 | email       | A         |          23 |        5 |   NULL | YES  | BTREE      |         |               | YES     | NULL       |
+---------+------------+----------------------+--------------+-------------+-----------+-------------+----------+--------+------+------------+---------+---------------+---------+------------+
```

**单字段排序**

- 排序字段有单列索引，但查询字段超出了索引字段会走回表，排序方式是`Using filesort`

```sql
# phone有索引，但select * 
mysql> explain select * from tb_user order by phone;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra          |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
|  1 | SIMPLE      | tb_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   24 |   100.00 | Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+

# 不是select * 但查询了name
mysql> explain select id,name,phone from tb_user order by phone;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra          |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
|  1 | SIMPLE      | tb_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   24 |   100.00 | Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+

# 正确的方式
mysql> explain select id,phone from tb_user order by phone;
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys | key            | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_phone | 46      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+

# 或
mysql> explain select id from tb_user order by phone;
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys | key            | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_phone | 46      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-------------+
```

- 排序字段是多列索引，能够用到索引

```sql
# 联合索引第一个字段
mysql> explain select id from tb_user order by profession;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+

# 联合索引第二个字段
mysql> explain select id from tb_user order by age;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using filesort |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
```

**多字段排序**

- 非主键、多个字段有单独的单列索引，整体索引失效

```sql
# phone 和 name 都有单列索引，合在一起排序使用的事Using filesort
mysql> explain select id, name,phone from tb_user order by name, phone;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra          |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
|  1 | SIMPLE      | tb_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   24 |   100.00 | Using filesort |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+----------------+
```

- 非主键、多个字段建立了联合索引，索引生效
  
  - 满足最左前缀法则，即order by后的字段按照联合索引的字段顺序才有最好的效果
  
  - 注意这里的最左前缀法则和where不同，where里面的顺序没有要求
  
  - 联合索引字段排序，要使用和建立索引相同的排序方式，要么都是asc，要么都是desc，否则效果也不是最佳。
  
  - 如果非要解决一个asc、一个desc，那么需要建一个对应的索引。比如A字段是asc、B字段是desc，那么建索引`create index idx_A_B on table_name(A asc,B desc)`。这样才会最佳

```sql
# 按照顺序排序，最佳
mysql> explain select id, profession, age, status from tb_user order by profession, age, status;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
# 按照顺序，即使没有写全字段也是最佳
mysql> explain select id, profession, age, status from tb_user order by profession, age;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-------------+
# 打乱顺序，效果不是最佳
mysql> explain select id, profession, age, status from tb_user order by age, status, profession;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using filesort |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
# 没有按照最左前缀法则，效果都不是最佳的
mysql> explain select id, profession, age, status from tb_user order by age, profession;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using filesort |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+

# 联合索引默认都是asc，此时都用desc，会多一个反转排序
mysql> explain select id, profession, age, status from tb_user order by profession desc, age desc;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+----------------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra                            |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+----------------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Backward index scan; Using index |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+----------------------------------+

# 一个用asc，一个用desc，效果也不是最佳
mysql> explain select id, profession, age, status from tb_user order by profession asc, age desc;
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key                  | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using filesort |
+----+-------------+---------+------------+-------+---------------+----------------------+---------+------+------+----------+-----------------------------+
```

- 有主键，多个字段有单独的单列索引，主键索引生效，只会用到一个索引

```sql
mysql> explain select id,phone from tb_user order by id, phone;
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-----------------------------+
| id | select_type | table   | partitions | type  | possible_keys | key            | key_len | ref  | rows | filtered | Extra                       |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-----------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | NULL          | idx_user_phone | 46      | NULL |   24 |   100.00 | Using index; Using filesort |
+----+-------------+---------+------------+-------+---------------+----------------+---------+------+------+----------+-----------------------------+
```

**order by优化原则**

- 根据排序字段合理合适的索引，多字段排序时，遵循最左前缀法则。

- 尽量使用覆盖索引，否则使用Using filesort。

- 多字段排序, 一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC）。

- 如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小sort_buffer_size(默认256k)。

# Group By优化

针对分组的字段，也需要建索引，多个字段分组时也遵循最左前缀法则。

**单个字段分组**

- 无索引

```sql
# 先删除name字段的索引
mysql> drop index idx_user_name on tb_user;
Query OK, 0 rows affected (0.04 sec)
# Using temporary，性能不高
mysql> explain select name, count(*) from tb_user group by name;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | SIMPLE      | tb_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   24 |   100.00 | Using temporary |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
```

- 有索引，且是单列索引

```sql
mysql> explain select phone, count(*) from tb_user group by phone;
+----+-------------+---------+------------+-------+----------------+----------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys  | key            | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+----------------+----------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_phone | idx_user_phone | 46      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+----------------+----------------+---------+------+------+----------+-------------+
```

- 有索引，且是联合索引
  
  - 遵循最左前缀法则，

```sql
# 按照顺序效果最佳
mysql> explain select profession, count(*) from tb_user group by profession;
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_pro_age_sta | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
# 没有按照顺序，效果不佳
mysql> explain select age, count(*) from tb_user group by age;
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows | filtered | Extra                        |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_pro_age_sta | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using temporary |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
```

**多字段分组**

- 无索引肯定效率最低

- 有索引，且是都是单列索引，这样使用效果不佳

```sql
# 将name的单列索引加回来
mysql> create index idx_user_name on tb_user(name);
Query OK, 0 rows affected (0.04 sec)
mysql> explain select phone,name,count(*) from tb_user group by phone,name;
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
| id | select_type | table   | partitions | type | possible_keys | key  | key_len | ref  | rows | filtered | Extra           |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
|  1 | SIMPLE      | tb_user | NULL       | ALL  | NULL          | NULL | NULL    | NULL |   24 |   100.00 | Using temporary |
+----+-------------+---------+------------+------+---------------+------+---------+------+------+----------+-----------------+
```

- 有索引，且是联合索引
  
  - 遵循最左前缀法则

```sql
# 遵循最左前缀法则效果最佳
mysql> explain select profession, age, status, count(*) from tb_user group by profession, age, status;
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_pro_age_sta | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
mysql> explain select profession, age, count(*) from tb_user group by profession, age;
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows | filtered | Extra       |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_pro_age_sta | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+-------------+

# 不遵循最左前缀法则效果不佳
mysql> explain select profession, age, count(*) from tb_user group by age,profession;
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
| id | select_type | table   | partitions | type  | possible_keys        | key                  | key_len | ref  | rows | filtered | Extra                        |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
|  1 | SIMPLE      | tb_user | NULL       | index | idx_user_pro_age_sta | idx_user_pro_age_sta | 54      | NULL |   24 |   100.00 | Using index; Using temporary |
+----+-------------+---------+------------+-------+----------------------+----------------------+---------+------+------+----------+------------------------------+
```

**group by优化原则**

- 在分组操作时，可以通过索引来提高效率。

- 分组操作时，索引的使用也是满足最左前缀法则的。

# Limit优化

在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低。

**优化思路**

通过使用覆盖索引、子查询形式进行优化

```sql
explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id;
```

但这种纯粹数据量问题靠单纯的SQL优化是不行的，需要进行分表。

# Count优化

count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是

NULL，累计值就加 1，否则不加，最后返回累计值。

**Count的不同用法**

| count用法   | 含义                                                                                   |
| --------- | ------------------------------------------------------------------------------------ |
| count(id) | InnoDB 引擎会遍历整张表，把每一行的 主键id 值都取出来，返回给server层。server层拿到主键后，直接按行进行累加(主键不可能为null)        |
| count(字段) | 没有not null 约束 : InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给server层，server层判断是否为null，不为null，计数累加。 |
|           | 有not null 约束：InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给server层，直接按行进行累加。                        |
| count(数字) | InnoDB 引擎遍历整张表，但不取值。server层对于返回的每一行，放一个数字“1”进去，直接按行进行累加。                             |
| count(*)  | InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，server层直接按行进行累加。                                    |

**效率对比**

`count(字段)` < `count(id)` < `count(1)` = `count(*)`，所以不要按照字面上意思count(*)将所有字段取出来，其实进行了优化。

# Update优化

InnoDB是支持行锁的，但并不是所有情况都是行锁。

- 根据索引字段更新时是行锁

```sql
# 开启事务后，根据id更新，事务没有提交之前只会锁住id=1这一行，其他行不收影响
start transaction ;
update course set name = 'Golang' where id = 1;
```

- 根据非索引字段更新时则会升级为表锁

```sql
# 开启事务，根据name更新，但name没有创建索引，所以会导致整张表锁住
start transaction ;
update course set name = 'JavaEE' where name = 'Golang';
```

**Update优化原则**

- 使用带索引的字段进行更新，最万全的方法就是采用id更新。
