# Ubuntu20-04安装mysql

## 1.安装DEB文件

（1）打开https://www.mysql.com按步骤获取下载链接

![image-20210705100650117](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100650117.png)

![image-20210705100727789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100727789.png)

![image-20210705100743757](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100743757.png)

https://dev.mysql.com/doc/refman/8.0/en/installing.html

![image-20210705100800396](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100800396.png)

![image-20210705100822371](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100822371.png)

![image-20210705100850767](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100850767.png)

![image-20210705100905735](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100905735.png)

![image-20210705100924475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100924475.png)

![image-20210705100946415](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705100946415.png)

![image-20210705101022815](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101022815.png)

右键选择复制链接：

![image-20210705101042786](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101042786.png)

（2）开始下载

sudo wget 链接

![image-20210705101148232](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101148232.png)

（3）开始安装，运行这个deb文件

sudo dpkg -i 文件名

![image-20210705101328396](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101328396.png)

table键选择OK

![image-20210705101453810](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101453810.png)

![image-20210705101505593](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101505593.png)

![image-20210705101521307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101521307.png)

按方向键向下，选择OK

![image-20210705101604051](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101604051.png)

（4）运行updata

sudo apt-get updata

![image-20210705101701355](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101701355.png)

## 2.安装mysql-server

sudo apt-get install mysql-server

![image-20210705101830348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101830348.png)

设置数据库服务器的root密码

![image-20210705101930662](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705101930662.png)

![image-20210705102025528](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102025528.png)

最好选第二项

![image-20210705102141649](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102141649.png)

## 3.安装完成测试

（1）检查数据库服务器的状态

sudo systemctl status mysql

![image-20210705102423158](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102423158.png)

（2）使用mysql的客户端工具连接数据库

Navicat

![image-20210705102605989](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102605989.png)

（3）登录mysql服务器查看权限，并修改权限

登录：mysql -u root -p回车输入密码

查看数据库：show database

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102749275.png)

use mysql;

![image-20210705102839068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705102839068.png)

show tables;

![image-20210705103016802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103016802.png)

![image-20210705103029805](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103029805.png)

![image-20210705103044530](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103044530.png)

host字段代表可以登录的主机的名字，代表只有本机才能登录

user字段代表用户名

修改root的host为可以远程登录

update user set host='%' where user='root';

flush privileges;

![image-20210705103243959](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103243959.png)

**![image-20210705103345911](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103345911.png)**

![image-20210705103430922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705103430922.png)

## 4.遇到的问题

（1）mysql access denied for user

在vim /etc/mysql/mysql.conf.d/mysqld.cnf

![image-20210705155129787](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210705155129787.png)

重启mysql服务，就好用了

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

# 新建数据库

create database 数据库名;

# 计算数据库所占的空间

```
SELECT sum(DATA_LENGTH)+sum(INDEX_LENGTH)
FROM information_schema.TABLES where TABLE_SCHEMA='HXCrawl';
```

# 快速删除一个partition的数据，partition结构不变

```
alter table crawl_raw_chat_test truncate partition p7;
```

# 新建数据表



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

# 查看版本

```
select version();
```

8.0.23-0ubuntu0.20.04.1

# 导出数据到文件

1.在mysql命令模式下用SQL语句导出：

select 字段 from 表名 into outfile 导出的路径（需要是指定路径）

```
select id,chat_content from my_test partition (p8)  into outfile '/var/lib/mysql-files/ddddd.xlsx'
```

2.需要去linux下面提取

3.MySQL 报错 ERROR 1290 (HY000): running with the --secure-file-priv

报错原因： 
secure_file_priv 设置了指定目录，需要在指定的目录下进行数据导出

```
mysql> show variables like '%secure%';
```

![image-20210706152556874](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210706152556874.png)

如果没有设置的话，需要在配置文件中设置

![image-20210706152626384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210706152626384.png)

# 数据库修复，备份

1. sudo su - root 切换root用户
2. vim /etc/mysql/mysql.conf.d/mysqld.cnf 编辑mysql配置文件
3. innodb_force_recovery = 0 -> 1 变更innodb_force_recovery参数并退出:(:wq)
4. service mysql restart 重启服务
5. mysqldump -udev -pdevpass HXCrawl crawl_raw_chat > raw_bak.sql 导出数据
6. *innodb_force_recovery = 1 -> 0 变更innodb_force_recovery参数并退出:(:wq) --不调回会导致数据无法入库
7. service mysql restart 重启服务
8. mysql -udev -pdevpass -f HXCrawlBak < raw_bak.sql 导入备库

手动改XXX.sql中的表名

​           更推荐用：（pv final_bak.sql | mysql -udev -pdevpass HXCrawl ）可以看导入进度

​                               pv raw_bak.sql | mysql -udev -pdevpass HXCrawlBak 

1. 如果是修复其他表（如：crawl_final_chat）,需要修改5,8改成：

​           mysqldump -udev -pdevpass HXCrawl crawl_final_chat > final_bak.sql导出数据

mysql -udev -pdevpass -f HXCrawl < final_bak.sql 导入备库

​      10.加条件：mysqldump -udev -pdevpass HXCrawl crawl_raw_chat --where="DATE(create_time) > '2021-4-14' " > raw_bak.sql

# binlog删除

1.在mysql命令下

```
SHOW MASTER LOGS;//查看
RESET MASTER;//全删
mysql> PURGE MASTER LOGS BEFORE '2020-11-11 11:11:11';//删除指定时间之前
mysql> PURGE MASTER LOGS TO 'binlog.000860';//删除指定编号之前
```

2.修改配置，过期自动删除

超过3天自动清除

```
expire_logs_days = 3 
```

3.使用上面删除方法不好用的时候

```
purge master logs to 'mysql-bin.000085'
```

这个可以删除mysql-bin.000085之前的binlog

# 当前时间的函数

SQL语言：now()

# 登录远程数据库的命令

mysql -u用户名 -p密码 -h远程数据库的ip

![image-20210803113155320](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210803113155320.png)

