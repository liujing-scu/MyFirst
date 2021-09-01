MySQL高级：

# 1.mysql的架构介绍

## 1.1 Mysql简介

### 1.1.1 概述

关系型数据库管理系统RMDBMS

### 1.1.2 高级MySQL

高级MySQL包含以下内容：

（1）mysql内核

（2）sql优化工程师

（3）mysql服务器的优化

（4）各种参数常量设定

（5）查询语句优化

（6）主从复制

（7）软硬件升级

（8）容灾备份

（9）sql编程

以上都是DBA的工作。

完整的mysql优化需要很深的功底，大公司甚至有专门的DBA

## 1.2 MysqlLinux版的安装

mysql5.5

### 1.2.1 下载地址

下载：GA版（稳定发布版）

​			linux-Generic

​			server

​			client

第三方软件按照标准放在opt目录下

![image-20210520112220208](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520112220208.png)

rpm -ivh

### 1.2.2 检查当前系统是否安装过mysql

有几种方式：

方式一：安装过的话，肯定启动过mysql，ps -ef|grep mysql看端口或服务有就是安装过。

方式二：以rpm来检查是否安装过，用命令rpm -qa|grep -i mysql，装过就有软件名，没装过就是空的

![image-20210520112805778](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520112805778.png)

![image-20210520112820216](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520112820216.png)

### 1.2.3 安装mysql服务端（注意提示）

rpm -ivh MySQL-server-XXXXX.rpm

i:install安装

v:日志

h:hash安装的进度条

### 1.2.4 安装mysql客户端

rpm -ivh MySQL-client-XXXXX.rpm

安装过程没有提示

### 1.2.5 查看MySQL安装时创建的mysql用户和mysql组

（1）方法一：

如果安装成功了，在linux下应该有mysql的用户和组

除了root用户，其他用户都在home。

如果查看linux用户组

cat /etc/passwd|grep mysql

cat /etc/group|grep mysql

![image-20210520140739453](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520140739453.png)

（2）方法二

查看mysql的版本，安装成功了会有内容返回

mysqladmin --version

![image-20210520141155643](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520141155643.png)



### 1.2.6 mysql服务的启+停

service 软件名字 start/stop/restart

service mysql start

ps -ef|grep mysql

![image-20210520141314839](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520141314839.png)

### 1.2.7 mysql服务启动后，开始连接

（1）无密码

首次连接成功，因为MySQL默认没有密码，所以这里我们没有输入密码就直接连上了，不安全。在命令行输入：mysql回车

（2）按照安装Server中的提示修改登录密码

/usr/bin/mysqlsdmin -u root password 123456

![image-20210520142026789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520142026789.png)

再次连接：

mysql -uroot -p回车

输入密码

### 1.2.8 自启动mysql服务

chkconfig mysql on

chkconfig --list|grep mysql

cat /etc/inittab

![image-20210520143524125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520143524125.png)

在看一个原始的命令

ntsysv

![image-20210520143727664](E:\work\data\image-20210520143727664.png)

开机启动的服务名前有*

### 1.2.9 修改配置文件位置

因为默认的配置文件一般我们不要去修改，做法是拷贝一个配置文件再用来修改。

首先进入配置文件目录：/user/share/mysql，然后拷贝 my-huge.cnf到/etc/my.cnf，最后要重启一下mysql服务。

不同版本的my-huge.cnf可能名字不同

5.5版本是my-huge.cnf，5.6版本是my-default.cnf

### 1.2.4 修改字符集和数据存储路径

以下4个数据库是出厂默认自带的

![image-20210521170323943](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521170323943.png)

create database db01;

use db01；

show tables;

create table user(id int not null,name varchar(20));

insert into user values(1,'z3');

insert into user values(2,'张三');

![image-20210521170646688](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521170646688.png)

（1）显示的中文不正确，需要先查看字符集编码。

show variables like ‘character%’;

或

show variables like '%char%';

![image-20210521170831641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521170831641.png)

默认的客户端和服务器都用了latin1，所以会乱码。

（2）修改字符集编码

cd /etc/

vim my.cnf进行编辑，修改3个地方，然后重启mysql服务

a.【client】

输入‘o’光标直接移动在下一行等待输入。

default-character-set=utf8

![image-20210521174152837](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521174152837.png)

b.【mysqld】

![image-20210521173550579](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521173550579.png)

c.【mysql】

![image-20210521173955316](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210521173955316.png)

**注意：这个字符集修改之前建立的数据库依然是乱码。需要新建一个库才有效。**

### 1.2.10 MySQL的安装位置

（1）数据库的用户建的数据库的存放位置

在linux下查看安装目录：ps -ef|grep mysql

![image-20210520144159926](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520144159926.png)

这里/var/lib/mysql存放了mysql用户建的数据库数据。一般变量都放在/var下面。

（2）四大目录

![image-20210520144655421](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520144655421.png)

关服务的两种方法

一：service mysql stop

二：/etc/init.d/mysql stop



## 1.3 Mysql配置文件

主要配置文件：

### 1.3.1 二进制日志log-bin

作用：用于主从复制

![image-20210526104136203](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526104136203.png)

### 1.3.2 错误日志log-error

默认是关闭的，记录严重的警告和错误信息，每次启动和关闭的详细信息等。

### 1.3.3 查询日志log

默认关闭，记录查询的sql语句，如果开启会降低mysql的整体性能，因为记录日志也是需要消耗系统资源。

### 1.3.4 数据文件

（1）两系统

windows：D:\devSoft\MySQLServer5.5\data目录下可以挑选很多库

linux：

a.看看当前系统中的全部库后再进去

![image-20210526105005644](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526105005644.png)

b.默认路径：/var/lib/mysql

（2）frm文件

存放表结构

（3）myd文件

存放表数据

（4）myi文件

存放表索引

### 1.3.5 如何配置

配置文件：

windows：my.ini文件

Linux：/etc/my.cnf文件

## 1.4 Mysql逻辑架构介绍

### 1.4.1 总体概述

![image-20210526110246345](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526110246345.png)

和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎的架构上，**插件式的存储引擎架构将查询处理和其它系统任务以及数据的存储提取相分离**。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。

（1）连接层

最上层是一些客户端和连接服务，包含本地sock通信和大多数基于客户端/服务端工具实现的类似于tcp/ip的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。

（2）服务层

第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化及部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定查询表的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部缓存。如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

（3）引擎层

存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API于存储引擎进行通信。不同的存储引擎具有的功能不同，这样我们可以根据自己的实际需要进行选取。后面介绍MyISAM和InnoDB。

（4）存储层

数据存储层，主要是将数据存储在运行于裸设备的文件系统之上，并完成与存储引擎的交互。

### 1.4.2 查询说明

## 1.5 Mysql存储引擎

### 1.5.1 查看命令

2种命令查看存储引擎

（1）show engines;

![image-20210526163015515](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526163015515.png)

（2）show variables like '%storage_engine%';

![image-20210526163052969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526163052969.png)



### 1.5.2 MyISAM和InnoDB

![image-20210526163114058](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526163114058.png)

### 1.5.3 阿里巴巴、淘宝用哪个

![image-20210526163527324](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210526163527324.png)

# 2.索引优化分析

## 2.1 性能下降SQL慢（执行时间长，等待时间长）

### 2.1.1 查询语句写得烂

### 2.1.2 索引失效

（1）单值

create index idx_user_name on user(name)

（2）复合

### 2.1.3 关联查询太多join（设计缺陷或不得已的需求）

### 2.1.4 服务器调优及各个参数设置（缓冲、线程数等）

## 2.2 常见通用的Join查询

### 2.2.1 SQL执行顺序

（1）手写顺序

![image-20210527170112854](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527170112854.png)

（2）机读顺序



![image-20210527165953270](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527165953270.png)

（3）总结

![image-20210527170322934](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527170322934.png)

### 2.2.2 Join图

![image-20210527170840759](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527170840759.png)

![image-20210527171003416](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527171003416.png)

![image-20210527171112522](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527171112522.png)           

### 2.2.3 建表SQL

![image-20210527173215394](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527173215394.png)

![image-20210527173241676](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527173241676.png)

### 2.2.4 7种JOIN

![image-20210527173603127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527173603127.png)

![image-20210527173613580](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527173613580.png)

（1）AB共有：inner join

![image-20210527173747107](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527173747107.png)

（2）A全有：left join

![image-20210527174230372](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527174230372.png)

（3）B全有：right join

![image-20210527174100986](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527174100986.png)

（4）A独有：left join+where

![image-20210527175201458](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527175201458.png)

（5）B独有：right join+where

![image-20210527175309235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527175309235.png)

（6）AB全有，full outer join

![image-20210527175556821](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527175556821.png)

mysql不支持这种语法（oracle支持）

所以，只能变化一种思路，使用union（天生自带去重）

![image-20210527175900754](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527175900754.png)

（7）AB的独有

![image-20210527180222529](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210527180222529.png)

## 2.3 索引简介

### 2.3.1 索引是什么

（1）MySQL官方对索引的定义为：索引（Index）是帮助MySQL高效获取数据的数据结构。

可以得到索引的本质：索引是数据结构。

索引的目的在于提高查询效率，可以类比字典。

如果要查“mysql”这个单词，我们肯定需要定位到m字母，然后从上往下找到字母y，再找到剩下的sql。

如果没有索引，那么你可能需要a~z，如果我想找到Java开头的单词呢？或者Oracle开头的单词呢？

是不是觉得如果没有索引，这个事情根本无法完成？

（2）**你可以简单理解为“排好序的快速查找数据结构”**。

**详解（重要）**

在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。下图就是一种可能的索引方式示例：

![image-20210528100020843](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528100020843.png)

为了加快Col2的查找，可以维护一个右边所示的二叉查找数，每个节点分别包含索引键值和一个指向对应数据记录地址指针，这样就可以运用二叉查找在一定复杂度内获取到相应数据，从而快速的检索出符合条件的记录。

**结论**

数据本身之外，数据库还维护着一个满足特定查找算法的数据结构，这些数据结构以某种方式指向数据，这样就可以在这些数据结构的基础之上实现高级查找算法，这种数据结构就是索引。

常见的是BTree索引。

（3）一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。

（4）我们平常所说的索引，如果没有特别指明，都是指B树（多路搜索树，并不一定是二叉的）结构组织的索引。其中聚集索引，次要索引，覆盖索引，复合索引，前缀索引，唯一索引摩尔那你都是使用B+树索引，统称索引。当然，除了B+树这种类型的索引之外，还有哈希索引（hash index）等。

### 2.3.2 优势

（1）类似大学图书馆建书目索引，提高数据检索的效率，降低数据库的IO成本。

（2）通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗。

### 2.3.3 劣势

（1）实际上索引也是一张表，该表保存了主键与索引字段，并指向实体表的记录，所以索引列也是要占用空间的。

（2）虽然索引大大提高了查询速度，同时却会降低更新表的速度，如对表进行insert，updata、delete。因为更新表时，MySQL不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，指向等，都会调整因为更新所带来的键值变化后的索引信息

索引只是提高效率的一个因素，如果你的MySQL有大数据量的表，就需要花时间研究建立最优秀的索引，或优化查询语句。

### 2.3.4 mysql索引分类

（1）单值索引

即一个索引只包含单个列，一个表可以有多个单列索引。

（2）唯一索引

索引列的值必须唯一，但允许有空值

（3）复合索引

即一个索引包含多个列

（4）基本语法

![image-20210528105311840](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528105311840.png)

![image-20210528105617709](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528105617709.png)

### 2.3.5 mysql索引结构

（1）BTree索引

检索原理：

![image-20210528105937814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528105937814.png)

【初始化介绍】

一颗b+树，浅蓝色的块我们统称为一个磁盘块，可以看到每个磁盘块包含几个数据项（深蓝色所示）和指针（黄色所示），如磁盘块1包含数据项17和35，包含指针P1、P2、P3，P1表示小于17的磁盘块，P2表示在17和35之间的磁盘块，P3表示大于35的磁盘块。

真实的数据存在于叶子节点，即3、5、9、10、13、15、28、29、36、60、75、79、90、99.

非叶子节点不存储真实的数据，只存储指引搜索方向的数据项，如17、35并不是真实存在于数据表中。

【查找过程】

如果要查找数据项29，那么收下你会把磁盘块1由磁盘加载到内存，此时发生一次IO，在内存中用二分查找确定29在17和35之间，锁定磁盘块1的P2指针，内存时间因为时间非常短（相比磁盘的IO）可以忽略不计，通过磁盘块1的P2指针的磁盘地址把磁盘块3 由磁盘加载到内存，发生第二次IO，29在26和30之间，锁定磁盘块3的P2指针，通过指针加载磁盘块8到内存，发生第三次IO，同时内存中做二分查找找到29，结束查询，总计三次IO。

真实的情况是，3层的b+树可以表示上百万的数据，如果上百万的数据查找只需要三次IO，性能提高将是巨大的，如果没有索引，每个数据项都要发生一次IO，那么总共需要百万次的IO，显然成本非常非常高。

（2）Hash索引

（3）full-text全文索引

（4）R-Tree索引

### 2.3.6 哪些情况需要创建索引

（1）主键自动建立唯一索引

（2）频繁作为查询条件的字段应该创建索引

（3）查询中与其他表关联的字段，外键关系建立索引

（4）频繁更新的字段不适合创建索引

因为每次更新不单单是更新了记录还会更新索引

（5）where条件里用不到的字段不创建索引

（6）单键/组合索引的选择问题，who？

在搞并发下倾向创建组合索引

（7）查询中排序的字段，排序字段若通过索引去访问将大大提高排序速度

（8）查询中统计或者分组字段

### 2.3.7 哪些情况不要创建索引

（1）表记录太少

（2）经常增删改的表

因为：提高了查询速度，同时却会降低更新表的速度，如对表进行insert、updata和delete。由于更新表时，MySQL不仅要保存数据，还要保存一下索引文件。

（3）数据重复且分布平均的表字段，因此应该只为最经常查询和最经常排序的数据列建立索引。

注意，如果某个数据列包含许多重复的内容，为它建立索引就没有太大的实际效果。

## 2.4 性能分析

### 2.4.1 MySQL Query Optimizer

![image-20210528144713819](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528144713819.png)

### 2.4.2 MySQL常见瓶颈

（1）CPU：CPU在饱和的时候一般发生在数据装入内存或从磁盘上读取数据的时候

（2）IO：磁盘I/O瓶颈发生在装入数据远远大于内存容量的时候

（3）服务器硬件的性能瓶颈：top，free，iostat和vmstat来查看系统的性能状态

### 2.4.3 Explain

（1）是什么（查看执行计划）

使用Explain关键字可以模拟优化器执行SQL查询语句，从而知道MySQL是如何处理你的SQL语句的。分析你的查询语句或是表结构的性能瓶颈。

官网介绍

![image-20210528145425511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528145425511.png)

（2）能干嘛

表的读取顺序

数据读取操作的操作类型

哪些索引可以使用

哪些索引被实际应用

表之间的引用

每张表有多少行被优化器查询

（3）怎么玩

Explain + SQL语句

执行计划包含的信息：id，select type，table，type，possible_keys，key，key_len，ref，rows，Extra

（4）各字段解释

**<1>id****

select查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序

三种情况：

-id相同：执行顺序由上至下

-id不同：如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行

-id相同不同同时存在：id如果相同可以认为是一组，从上到下顺序执行；在所有组中id值越大，优先级越高，越先执行。

**<2>select type****

![image-20210528153244885](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528153244885.png)

-simple：简单的select查询，查询中不包含子查询或union

-primary：查询中若包含任何复杂的子部分，最外层查询则被标记为primary

-subquery：在select或where列表中包含了子查询，非最外层的查询被标记为subquery。

-derived：在from列表中包含的子查询被标记为DERIVED（衍生）MySQL会递归执行这些子查询，把结果放在临时表里。这个临时表被标记为DERIVED。

-union：若第二个select出现在union之后，则被标记为union；

若union包含在from子句的子查询中，外层select将被标记为：DERIVED。

-union result：从union表获取结果的select。

**<3>table**

显示这一行的数据是关于那张表的

**<4>type**

访问类型排列

显示查询使用了何种类型，从最好到最差：

常用的（system>const>eq_ref_ref_range_index_ALL）

**一般来说，得保证查询至少达到ref或range级别**

【system】

表只有一行记录（等于系统表，这是const类型的特例，平时不会出现，这个也可以忽略不计）

【const】

表示通过索引一次就找到了，const用于比较primary key或unique索引。因为只匹配一行数据，所以很快就将主键置于where列表中，MySQL就能将该查询转换为一个常量。

【eq_ref】

唯一性索引扫描，对于每一个索引键，表中只有一条记录与之匹配，常用于主键或唯一索引扫描。

【ref】

非唯一性索引扫描，返回匹配某个单独值的所有行，本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体。

【range】

只检索给定范围的行，使用一个索引来选择行，key列显示使用哪个索引，一般就是在你的where语句中出现between，<，>，in等的查询；这种范围索引扫描比全表扫描要好，因为它只需要开始于索引的某一个点，结束于另一个点，不用扫描全部索引。

【index】

index于all的区别为index类型只遍历索引树，这通常比all快，因为索引文件通常比数据文件小；也就是说，虽然all和index都是读写表，但是index是从索引中读取，而all是从硬盘中读取的。

【all】

全表扫描，将遍历全表以找到匹配的行。

**<5>possible_keys**

可能用到的索引，是一种预测，但不一定被查询实际使用

NULL：没有用索引

**<6>key**

实际使用到的索引。

NULL：没有使用索引

查询中若使用了覆盖索引，则该索引仅出现在key列表中

覆盖查询：查询的数据中字段和所建的索引个数和顺序一致。

**<7>key_len**

表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度。在不损失精确性的情况下，长度越短越好。

key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过内检索出的

用于判断复合索引是否被完全被使用（a,b,c）

**<8>ref**

显示索引的哪一列被使用了，如果可能的话，是一个常数。哪些列或者常量被用于查找索引列上的值

**<9>rows**

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需读取的行数。越小越好。

**<10>Extra**

包含不适合在其他列显示，但是很重要的信息

【Using filesort】这个不好

说明MySQL会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取。MySQL中无法利用索引完成的排序操作称为“文件排序”。

![image-20210528174039307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528174039307.png)

【Using temporary】这个不好

新建了一个临时表，更槽糕了。

使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序order by和分组查询group by。

![image-20210528174755769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528174755769.png)

【Using index】这个好

表示相应的select操作中使用了覆盖索引（covering index），避免访问了表的数据行，效率不错！

如果同时出现using where，表明索引被用来执行索引键值的查找；

如果没有出现using where，表明索引用来读取数据而非执行查找动作。

![image-20210528175414311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210528175414311.png)

覆盖索引

理解：select的数据列只从索引中就能够取得，不必读取数据行，MySQL可以利用索引返回select列表中的字段，而不必根据索引再次读取数据文件，换句话说查询列要被所建的索引覆盖。

（5）热身Case

## 2.5 索引优化

# 3.查询截取分析

## 查询优化

## 慢查询日志

## 批量数据脚本

## show profile

## 全局查询日志

# 4.MySql锁机制



# 5.主从复制

