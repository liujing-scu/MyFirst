

explain显示了MySQL如何使用索引来处理select语句以及连接表。可以由此选择更好的索引和写出更优化的查询语句。

explain可以做什么？

1.表的读取顺序

2.数据读取操作的操作类型

3.哪些索引可以使用

4.哪些索引被实际使用

5.表之间的引用

6.每张表有多少行被优化器查询



基本语法：explain+待执行的SQL

各字段的含义：

## 1.id

决定表的读取顺序

执行select语句查询的序列号，包含一组数字，表示查询中执行select子句或操作表的顺序。有3种情况：

### （1）id相同

执行顺序由上至下

### （2）id不同

如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行。

### （3）id相同和不同都存在

如果id相同，可以认为是一组，从上往下顺序执行。在所有的组中，id值越大，优先级越高，越先执行。

## 2.select_type

查询的类型，也就是数据读取操作的操作类型，有以下5种：

### （1）simple

简单的select查询，查询中不包含子查询或者union。

### （2）primary

查询中若包含任何复杂的子查询，最外层查询则被标记

### （3）subquery

在select或者where列表中包含了子查询

### （4）derived

在from列表中包含的子查询被标记为DERIVED(衍生表)，MySQL会递归执行这些查询，把结果放临时表中

### （5）union

若第二个select出现在union之后，则被标记为union，若union包含在from子句查询中，外层select将被标记为DERIVED

### （6）union result

从union表（即union合并的结果集）中获取select查询的结果

## 3.type

访问类型排序

显示查询使用了何种类型，从最好到最差依次是：system>const>eq_ref>ref>range>index>all

### （1）system

表示只有一行记录（等于系统表），这是const类型的特例，平时不会出现，这个也可以忽略不计

### （2）const

表示通过索引一次就找到了，const用于比较primary key或者unique索引。因为只匹配一行记录，所以很快。如果将主键置于where列表中，MySQL就能将该查询转换为一个常量。

### （3）eq_ref

唯一性索引扫描，对于每一个索引键，表中只有一条记录与之匹配，常用于主键或唯一索引扫描。

### （4）ref

非唯一性索引扫描，返回匹配某个单独值的所有行，本质上也是一种索引访问，它返回所有匹配某个单独值的行，然而，它可能会找到多个符合条件的行，所以它应该属于查找和扫描的混合体。

### （5）range

只检索给定范围的行，使用一个索引来选择行，key列显示使用哪个索引，一般就是在你的where语句中出现between，<，>，in等的查询；这种范围索引扫描比全表扫描要好，因为它只需要开始于索引的某一个点，结束于另一个点，不用扫描全部索引。

### （6）index

index于all的区别为index类型只遍历索引树，这通常比all快，因为索引文件通常比数据文件小；也就是说，虽然all和index都是读写表，但是index是从索引中读取，而all是从硬盘中读取的。

### （7）all

全表扫描

**备注：一般来说，得保证查询至少达到range级别，最好能达到ref**

## 4.possible_keys

显示可能会被应用到这张表的索引，一个或者多个；查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询实际使用到。

## 5.key

实际使用到的索引。如果为null，则没有使用索引；查询中若使用了覆盖索引，则该索引仅出现在key列表中

## 6.key_len

表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度，在不损失精确性的情况下，长度越短越好；key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得到的，不是通过表内检索出的。

## 7.ref

显示索引的哪一列被使用了，如果可能的话，是一个常数，哪些列或常量被用于查找索引列上的值。

## 8.rows

根据表统计信息及索引选用情况，大致估算出找到所需的记录所需要读取的行数。

## 9.Extra

包含不适合在其他列中显示但十分重要的额外信息：

### （1）using filesort

出现这个东西不好，说明MySQL会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取，MySQL中无法利用索引完成的排序操作称为“文件排序”；

### （2）using temporary

出现这个东西更不好，使用到了临时表。使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表，常见于排序order by和分组查询group by。

### （3）using index

表示相应的select操作中使用了覆盖索引（Covering Index），避免了访问了表的数据行，效率不错！

如果同时出现using index和using where表明索引被用来执行索引键值的查找。

如果没有同时出现using where，表明索引用来读取数据而非执行查找操作。



覆盖索引的理解。

理解方式一：

就是select的数据列只用从索引列中就能取得，不必读取数据行，MySQL可以利用索引返回select列表中的字段，而不必根据索引再次读取数据文件，换句话说查询列要被所建的索引列覆盖；

理解方式二：

索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行，毕竟索引的叶子节点存储了索引数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了；一个索引包含了（或者覆盖了）满足查询结果的数据，就叫做覆盖索引。

注意：如果要使用覆盖索引，一定要注意select列表中只读取出需要的列，不可select*；因为如果将所有的字段一起做索引会导致索引文件过大，查询性能下降；

### （4）using where

使用了where

### （5）using join buffer

使用了链接缓存

### （6）impossible where

where子句的值总是false，不能用来获取任何元素

### （7）select tables optimized away

在没有group by子句的情况下，基于索引优化MIN/MAX操作或者对于MyISAM存储引擎优化count(*)操作，不必等到执行阶段再进组计算，查询执行计划生成的阶段即完成优化。

### （8）distinct

去掉重复的数据

以上便是explain解析sql后表头各个字段的描述。

建立索引

![image-20210508173228328](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210508173228328.png)

删除索引：

drop index 索引名 on 表名

查询索引

show index from 表名



## 5.B树与索引

### 4.索引

#### 分类：

主键索引：不能重复。主键索引不能为null，唯一索引可以是null。

单值索引：

单列（age），一个表可以有多个单值索引，比如再有个（name）

唯一索引：

不能重复。（一般用id，唯一标识符）

复合索引：

多列构成的索引（相当于二级目录：z:zhao）(name,age)

#### 创建索引

##### 方式一：

create 索引类型 索引名 on 表（字段名）

单值索引：默认上面那种创建就是。

例子：create index dept_index on tb(dept)

唯一索引：

create **unique** index name_index on tb(name)

复合索引：

create index  dept_name_index on tb**(dept,name)**

##### 方式二：

alter table 表名 索引类型  索引名（字段）

单值索引：

alter table tb add index dept_index(dept)

唯一索引：

alter table tb add **unique** index name_index(name)

复合索引：

alter table tb add index  dept_name_index **(dept,name)**

注意：方式二是DDL语句，会自动提交，不用commit。

DML不会自动提交，需要commit。

注意：如果一个字段是primary key，则该字段默认是主键索引

#### 删除索引

drop index 索引名 on 表名

drop index name_index on tb

#### 查询索引

show index from 表名

还可以使用：show create table 表名

![image-20210517165227477](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210517165227477.png)

### 5.SQL性能问题

a.分析SQL的执行计划：explain，可以模拟SQL优化器执行SQL语句，从而让开发人员知道自己编写的代码的质量

b.MySQL查询优化器会干扰我们的优化

优化方法，官网的https://dev.mysql.com/doc/refman/8.0/en/optimization.html

查询执行计划：

explain+SQL语句

explain select * from tb

id：编号

select_type：查询类型

table：表名

type：类型

possible_keys：预测用到的索引

key：实际用的索引

key_len：实际使用索引的长度

ref：表之间的引用

rows：通过索引查询到的数据量

Extra：额外的信息



course（课程表）

| cid  | cname | tid(外键) |
| ---- | ----- | --------- |
| 1    | java  | 1         |
| 2    | html  | 1         |
| 3    | sql   | 2         |
| 4    | web   | 3         |

teacher

| tid  | tname | tcld(外键) |
| ---- | ----- | ---------- |
| 1    | tz    | 1          |
| 2    | zw    | 2          |
| 3    | tl    | 3          |

teacherCard

| tcid | tcdesc |
| ---- | ------ |
| 1    | tzdesc |
| 2    | twdesc |
| 3    | tldesc |

准备数据：

create table course

(

cid int(3),

cname varchar(20),

tid int(3)

);

create table teacher

(

tid int(3),

tname varchar(20),

tcid int(3)

);

create table teacherCard

(

tcid int(3),

tcdesc varchar(200)

);

插入数据

insert into course values(1，'java',1);

insert into course values(2，'html',1);

insert into course values(3，'sql',2);

insert into course values4，'web',3);



insert into teacher values(1，'tz',1);

insert into teacher values(2，'tw',2);

insert into teacher values(3，'tl',3);



insert into teacherCard values(1，'tzdesc');

insert into teacherCard values(2，'twdesc');

insert into teacherCard values(3，'tldesc');



查询课程编号为2 或 教师证标号为3 的语句

select t.* from teacher t, course c,teacherCard tc where t.tid=c.tid and t.tcid=tc.tcid and (c.cid=2 or tc.tcid=3) 

![image-20210510112150027](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510112150027.png)

#### （1）id

**id值相同，从上往下，顺序执行**

t3-tc3-c4

如果给teacher表加点数据

insert into teacher values(4，'ta',4);

insert into teacher values(5，'tb',5);

insert into teacher values(6，'tc',6);

![image-20210510112426922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510112426922.png)

顺序执行tc3-c4-t6

表的执行顺序，因数量的个数改变而改变的原因：笛卡尔积

​												a    b     c

​												2    3      4
$$
2*3*4=24
$$
改成

​												a   c     b

​												2    4      3

总值不变，但是中间值从6，变成了8，计算机存储中间结果变大了，性能就差一点了。所以SQL自己优化成小的值优先查询。

**id值不同，id值越大优先查询**

(在嵌套子查询时，先查询内层，再查询外层。)

查询教授SQL课程的老师的描述（desc）

select tc.desc from teacherCard tc,course c,teacher t  where c.tid=t.cid and t.tcid=tc.tcid and c.name='sql'

![image-20210510114002132](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510114002132.png)

将以上多表查询转化为子查询形式：

select tc.desc from teacherCard tc where tc.tcid=

(select tc.tcid from teacher t where t.tid =

 (select c.tid from course c where c.cname='sql'))

![image-20210510121419658](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510121419658.png)

子查询+多表

select t.tname,tc.tcdesc from teacher t,teacherCard tc where t.tcid=tc.tcid and t.tid=(select c.tid from course c where cname='sql')

![image-20210510141425864](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510141425864.png)

**id值有相同，也有不同的：id值大的优先，id同的从上往下，顺序执行。**

#### （2）select_type

primary：包含子查询SQL中的主查询（最外层）

subquery：包含子查询SQL中的子查询（非最外层）

simple：简单查询（不包含子查询、union）

derived：衍生查询（在查询时使用了临时表）

​				**a.在from子查询中只有一张表**

​				select cname from（select * from course where tid in(1,2)）cr

![image-20210510142451412](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510142451412.png)

​				**b.在from子查询中，如果有table1 union table2，则table1就是derived**

​					select cr.cname from (select * from course where tid=1 union select * from course where tid=2) cr

​				![image-20210510143303970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510143303970.png)

union：上面的例子

union result：上面的例子（告知开发人员哪些表之间存在union查询）

#### （3）type

索引类型

system>const>eq_ref>ref>range>index>all

要对type进行优化的前提是要有索引

其中：system，const只是理想情况，实际能达到ref>range

**system：**只有一条数据的系统表；或衍生表只有一条数据的主查询

create table test01

(

 tid int(3),

tname varchar(20)

)

insert into test01 values(1,'a')

commit;

增加索引(主键索引)

alter table test01 add constraint tid_pk primary key(tid)

explain select * from(select * from test01) t where tid=1;

![image-20210510151300694](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510151300694.png)

**const**：仅仅能查到一条数据的SQL，用于primary key或unique索引（类型与索引类型有关）

explain select tid from test01 where tid=1

![image-20210510151729027](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510151729027.png)

现在把主键索引删除：alter table test01 drop primary key

加一个其他索引：create index test01_index on test01(tid)

然后就不满足const的条件了

![image-20210510152146275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510152146275.png)

**eq_ref**：唯一性索引，对于每个索引键的查询，返回匹配唯一行数据（有且只有1个，不能多，不能0）

select …… from……where name=……

此种情况**常见**于唯一索引和主键索引

alter table teacherCard add constraint pk_tcid primary key(tcid);

alter table teacher add constraint uk_tcid unique index(tcid);

select t.tcid from teacher t,teacherCard tc where t.tcid=tc.tcid

![image-20210510153016970](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510153016970.png)

因为查询有0的，所以不是eq_ref

现在给teacher的数据删到只剩3个，

![image-20210510153247753](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510153247753.png)

**ref**

非唯一性索引，对于每个索引键查询，返回匹配的所有行（0或多）

准备数据：

![image-20210510154224117](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510154224117.png)

给tname加上索引

alter table teacher add index index_tname(tname);

select * from teacher where tname='tz'

返回2行

![image-20210510154510841](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510154510841.png) 

**range**:检索指定范围的行，where后面是一个范围查询（between，in,>,< 特殊：in有时候会失效，从而是all）

alter table teacher add index tid_index(tid)

explain select t.* from teacher t where t.tid in(1,2)

![image-20210510154829125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510154829125.png)

explain select t.* from teacher t where t.tid<3

![image-20210510154836962](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510154836962.png)

explain select t.* from teacher t where t.tid>3

![image-20210510154856472](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510154856472.png)

explain select t.* from teacher t where t.tid between 1 and 2

![image-20210510155247452](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510155247452.png)

**index**

查询全部索引中的数据

tid是索引，只需要扫描索引表，不需要扫描表的所有数据

![image-20210510155346532](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510155346532.png)

**all**

查询全部表中的数据

cid不是索引，需要扫描全表的数据

![image-20210510155449326](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510155449326.png)

总结：

system和const：结果只有一条数据

eq_ref结果多条，但是每条数据是唯一的

ref：结果多条，但是每条数据是0或者多

#### （4）possible_keys

可能用到的索引，是一种预测，不准

NULL：没有用索引

#### （5）key

实际使用到的索引

#### （6）key_len

索引的长度，用于判断复合索引是否被完全被使用（a,b,c）

create table test_kl

(

name char(20) not null defult ' '

);

alter table test_kl add index index_name(name);

explain select * from test_kl where name='';

![image-20210510163345025](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510163345025.png)

key_len:60，原因，utf8一个字符占3个字节

alter table test_kl add column name1 char(20);

name1可以为null，name不能为空

alter table test_kl add index index_name1(name1);

![image-20210510163710825](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510163710825.png)

61比60多了1，就是为空造成的

如果索引字段可以是NULL，则会使用1个字节用于标识。

drop index index_name on test_kl

drop index index_name1 on test_kl

增加一个复合索引

alter table test_kl add index index_name_name1(name,name1);

explain select * from test_kl where name1='';

![image-20210510164008914](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510164008914.png)

explain select * from test_kl where name='';

![image-20210510164319341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510164319341.png)

用varchar

alter table test_kl add colum name2 varchar(20);

alter table test_kl add index index_name2(name2)

explain select * from test_kl where name2='';-----63

![image-20210510164404198](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510164404198.png)

20*3, 1(标识NULL) ,2(标识可变长度)

所以是63

utf8:1个字符3个字节

gbk：一个字符2个字节

latin：一个字符1个字节

#### （7）ref

注意与type中的ref值区分

作用：指明当前表所参照的字段

select …… where a.c=b.x(其中b.x可以是常量const)

explain select * from course c,teacher t where c.tid=t.tid and t.tname='tw'

![image-20210510165744737](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510165744737.png)

c.tid没有加索引，所以是NULL

要给加上索引才能研究这个

alter table course add index index_tid(tid)

![image-20210510165936528](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510165936528.png)

#### (8)rows

被索引优化查询的数据个数

select * from course c,teacher t where c.tid=t.tid and t.tname='tz'

![image-20210510170155603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510170155603.png)

#### (9)Extra

**using filesort：**性能消耗大；需要“额外”的一次排序（需要额外一个查询），常见于order by语句中

create table test02

(

a1 char (3),

a2 char(3),

a3 char(3),

index idx_a1(a1),

index idx_a2(a2),

index idx_a3(a3)

);

explain select * from test02 where a1='' order by  a1

explain select * from test02 where a1='' order by  a2-------------using filesort

排序：先查询

小结：对于单索引，如果排序和查找是同一个字段，则不会出现using filesort。如果排序和查找不是同一个字段，则会出现using filesort。

复合索引：不能跨列（最佳左前缀）

alter table test02 add index index_a1_a2_a3(a1,a2,a3)

![image-20210510174703215](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210510174703215.png)

因为跨列了，没有从左边开始

小结：避免方法-where和order by按照复合索引的顺序使用，不要跨列或无序使用。

**using temporary**：性能损耗大

用到了临时表，一般出现在group by语句中；已经有表了，但是不适用，必须再来一张表

解析过程：

from  ... on ... join ... where ... group by ... having ... select distinct ... order by ... limit ...

例子：

select * from test03 where a2=2 and a4=4 group by a2,a4

没有using temporary

![image-20210511104123184](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511104123184.png)

select * from test03 where a2=2 and a4=4 group by a3

有using temporary

![image-20210511104432783](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511104432783.png)

**using index**：性能提升，索引覆盖。原因：不读取原文件，只从索引文件中获取数据（不需要回表查询）

只要使用到的列全部都在索引中，就是索引覆盖using index

例如：test02表中有一个复合索引（a1,a2,a3）

![image-20210511104943866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511104943866.png)

![image-20210511105201238](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511105201238.png)

![image-20210511105440758](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511105440758.png)

![image-20210511105451899](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511105451899.png)

**using where**：需要回原表查询

假设age是索引列

但是查询语句select age，name from where age=

此语句中必须回原表查询name

![image-20210511105753966](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511105753966.png)

**impossible where**：where子句永远为false

![image-20210511105934435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511105934435.png)

![image-20210511110011286](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210511110011286.png)



### 优化实例

create table test03

(

a1 int(4) not null,

a2 int(4) not null,

a3 int(4) not null,

a4 int(4) not null

);

alter table test03 add index idx_a1_a2_a3_a4(a1,a2,a3,a4);

创建好后执行。

select a1,a2,a3,a4 from test03 where a1=1 and a2=2 and a3=3 and a4=4;--------推荐的写法，因为索引的使用顺序（where后面的顺序）和复合索引的顺序一致

执行是从where开始的，顺序是a1,a2,a3,a4

![image-20210512145521922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210512145521922.png)

select a1,a2,a3,a4 from test03 where a4=1 and a3=2 and a2=3 and a1=4;

![image-20210512145736504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210512145736504.png)

但是和上面一模一样，这里1,2,3,4和4,3,2,1等价的，这是因为SQL的优化器自动给你优化了。虽然编写的顺序和复合索引的顺序不一致，但是被优化成一致的了。

以上2个SQL使用了全部的复合索引（因为key_len=16）

现在分析：

select a1,a2,a3,a4 from test03 where a1=1 and a2=2 and a4=4 order by a3;

![image-20210512150226536](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210512150226536.png)

using index不需要回表

using where需要回表

因为：a1,a2是符合1,2,3,4的顺序的，不需要回表，但是a4跳了一个顺序，需要回表

还能看到这里只用了a1,a2的索引，因为key_len=8

