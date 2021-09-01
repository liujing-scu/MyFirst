mysql整型数据导出到excel最后几位变成0了

问题：数据直接导出到excel或者csv，当字段类型是bigint的时候，会显示成科学计数格式。双击单元格后，发现数据的后面几位都显示为0了，显然已经和原来数据库的数据不一致了。

解决方法：

1.需要将bigint数据转换为char类型，并且前面加一个逗号，方便excel直接显示全部数字

```
select concat("'",cast(id as char)) from 表名 
```

上面concat就是拼接逗号和id，cast是将id转换为char类型（之前是bigint类型）

2.导出数据到excel

（1）可以借助pycharm

![image-20210714172155597](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210714172155597.png)

![image-20210714172240057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210714172240057.png)

（2）登录mysql，直接使用SQL语言导出

```
select concat("'",cast(id as char)) from 表名 into outfile '/var/lib/mysql-files/XXX.xlsx'
```

这里注意，文件的存放路径不能随意写，不然会报错：

 ERROR 1290 (HY000): running with the --secure-file-priv

原因是secure_file_priv 设置了指定目录，需要在指定的目录下进行数据导出

文件路径需要查询

```
mysql> show variables like '%secure%';
```

![image-20210714172923784](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210714172923784.png)

如果想修改路径，需要在mysql的配置文件（一般默认位置/etc/mysql/mysql.conf.d/mysqld.cnf）中添加：

```
secure_file_priv=文件存放路径
```

然后重启mysql服务，注意存放文件的目录权限需要和mysql的权限一致。不一致的话需要修改权限：

```
chmod mysql：mysql 目录
```

