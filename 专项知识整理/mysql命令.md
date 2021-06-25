# 查看mysql的线程

`show processlist`

![image-20210521154147095](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521154147095.png)



# 修改分区

hash

```
alter table crawl_raw_chat_test partition by hash (id) partitions 20;
```

# 查看分区情况

```
SELECT PARTITION_NAME,PARTITION_METHOD,PARTITION_EXPRESSION,PARTITION_DESCRIPTION,TABLE_ROWS,SUBPARTITION_NAME,SUBPARTITION_METHOD,SUBPARTITION_EXPRESSION
FROM information_schema.PARTITIONS WHERE TABLE_SCHEMA=SCHEMA() AND TABLE_NAME='my_test';
```

# 查看mysql的变量

```
show global variables like '%timeout%';
show global variables like '%packet%';
```

# 查看有哪些数据库

```
show databases
```

# 计算数据库所占的空间

```
SELECT sum(DATA_LENGTH)+sum(INDEX_LENGTH)
FROM information_schema.TABLES where TABLE_SCHEMA='HXCrawl';
```

# 快速删除一个partition的数据，partition结构不变

```
alter table crawl_raw_chat_test truncate partition p7;
```

# 建立索引

```
方法一：create 索引类型 索引名 on 表（字段名）
方法二：alter table 表名 索引类型  索引名（字段）
```

# 删除索引

```
drop index 索引名 on 表名
```

# 查询索引

show index from 表名

# 查看表建立的情况

`show create table crawl_final_chat`

# 快速删除表中的数据，表结构不变

```
truncate table my_test
```

# 添加/删除主键

alter table 表名 add constraint 主键名 primary key(字段名1,字段名2……)

alter table 表名 drop constraint 主键名

# 修改字段

![image-20210608113544331](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210608113544331.png)
