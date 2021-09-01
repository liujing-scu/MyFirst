## 1.Mysql为什么要分区

数据量（记录数）超过600万之后，如果不分区，读取数据很有可能是全表扫描，这样会很耗时，而且还可能会遇到磁盘问题（可能索引被破坏），从而导致数据表不能正常工作了。所以，当数据量太大的时候（>600万条），建议最好是给表分区。

## 2.表分区的SQL语句

这里用的hash+字段名（需要字段名是整数）的方法分区的。

```
alter table 表名 partition by hash (字段名) partitions 分区数;
```



## 3.查询表分区的SQL语句

```
SELECT PARTITION_NAME,PARTITION_METHOD,PARTITION_EXPRESSION,PARTITION_DESCRIPTION,TABLE_ROWS,SUBPARTITION_NAME,SUBPARTITION_METHOD,SUBPARTITION_EXPRESSION
FROM information_schema.PARTITIONS WHERE TABLE_SCHEMA=SCHEMA() AND TABLE_NAME='表名';
```



## 4.使用分区的SQL语句

使用举例

```
select count(*) from 表名 partition (p0) where 条件
```

如果需要一次扫描多个区

select count(*) from 表名 partition (p0,p1,……) where 条件

## 5.Mysql和python结合使用

如果分区的数量很多，要遍历所有的分区，如果手动一个一个地去读取，太麻烦了。需要借助python的循环语句遍历分区。

```
for i in range(100):
    sql_s = "select * from 表名 partition (p{}) ".format(i)
```

6.使用存储过程和函数

