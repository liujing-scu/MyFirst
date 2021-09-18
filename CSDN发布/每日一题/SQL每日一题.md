# 2021-9-15

有如下一张表T0915A

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qHC6MvgmUPy5e02eQRYXpEHViapYPibHlUaicFxA3k99ExDURQA10DZdAjK6w3QY8lficvHlwe8EpBsIg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

T0915B

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qHC6MvgmUPy5e02eQRYXpEHlzmV6UUqrdibqbic3jV2Ewm35A3raicWIzHEL67whBqiavsVMdCD6YiaHdg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

希望得到如下结果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qHC6MvgmUPy5e02eQRYXpEHzCMvx2gt2Mek9QONTdFZ0pcfzhkzYibXxeWF2tnbDOfazWGibP9TwGYg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



该如何写这个SQL？





```
CREATE TABLE T0915A 
(
用户编号 VARCHAR(10),
用户地址 VARCHAR(20),
金额 INT
)

INSERT INTO T0915A VALUES
('001','花开村1号',10),
('002','花开村2号',15),
('003','花开村6号',15),
('004','花开村5号',16),
('005','花开村12号',8)

CREATE TABLE T0915B 
(
微信用户订单号 VARCHAR(10),
用户编号 VARCHAR(50)
)

INSERT INTO T0915B VALUES
('90002','001,002,004'),
('90003','005')
```



答案：

```
select 用户编号,用户地址,金额,(select t2.微信用户订单号 from T0915B t2 where t2.用户编号 like concat('%',t1.用户编号,'%' ))as 微信用户订单号 from T0915A t1;
```

# 2021-9-16

有如下一张表T0916

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qF6cNSpryWbETU36Yke6l6OdOX0tofxayonEnCPrLuO19j5UfHgF5dnjPkTAtfuB80O5oRUgkPQFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

希望按工号查找日期(RecDate)相同的记录数>1，且Rectime=Time4

预计结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qF6cNSpryWbETU36Yke6l6ONib5LdcibeMSuQDozqgiblVPGvfVLPYiboJXg79n5gUicIr5dsgKOWIqCtw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



**测试数据**



```
CREATE TABLE T0916 (
workid VARCHAR(20),
RecDate DATE,
RecTime VARCHAR(20),
Time4 VARCHAR(20)
)
INSERT INTO T0916 VALUES ('161181','2021-05-03','13:01','13:01');
INSERT INTO T0916 VALUES ('161181','2021-05-03','14:01','15:01');
INSERT INTO T0916 VALUES ('161181','2021-05-06','13:01','13:01');
INSERT INTO T0916 VALUES ('161181','2021-05-07','13:01','13:01');
INSERT INTO T0916 VALUES ('161181','2021-05-08','13:01','13:01');
INSERT INTO T0916 VALUES ('161181','2021-05-09','13:01','13:01');
```

我的答案：

```
select * from T0916 where RecTime=Time4 and RecDate in (select RecDate from T0916 group by RecDate having count(RecDate)>1) ;
```

给的答案：

```
select a.* from T0916 a join(select workid,RecDate from T0916 GROUP BY workid, RecDate HAVING count(1)>1) b on a.workid=b.workid and a.RecDate=b.RecDate where a.RecTime=a.Time4;
```

# 2021-9-17

有如下一张员工表T0917

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qFzzRp8jiaFUXC1yoWtk0GXFaTNf74RXjAfuicC5icDCLQhUDbffa8eeZp5fG8Gz7Vxe9D1cB2w7ECnQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

部门表T0917B

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qFzzRp8jiaFUXC1yoWtk0GXF0iacfleQeMRZxC0eXgTCiaDA6MAGwMDY069FzvdXUcmphfofVibBSg65Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

编写一个 SQL 查询，找出每个部门获得前三高工资的所有员工。预计得到的结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qFzzRp8jiaFUXC1yoWtk0GXFhguKlB5ud0mm0ZZicnnrYm9CMOXNAHrviahBQxIBrte4M407pC2K7crw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



要求：至少两种方法求解！



**测试数据**



```
CREATE TABLE T0917
(
ID INT,
NAMES VARCHAR(20),
Salary INT,
DepartmentID INT
)

INSERT INTO T0917 VALUES
(1,'曹操',85000,1),
(2,'关羽',80000,2),
(3,'刘备',60000,2),
(4,'吕布',90000,1),
(5,'张飞',69000,1),
(6,'赵云',85000,1),
(7,'黄忠',70000,1)

CREATE TABLE T0917B
(
ID INT,
Name VARCHAR(20)
)

INSERT INTO T0917B VALUES
(1,'IT'),(2,'Sales')
```

解法1：

```
select a.names as employee,b.name as department,Salary from
(select DENSE_RANK() over (partition by DepartmentID order by Salary desc) r,NAMES,Salary,DepartmentID from T0917) as a
inner join T0917B b on a.DepartmentID=b.ID
where r<=3
order by Department;
```

解法2：

```
select d.name as department,e1.names as employee,e1.salary from T0917 e1
join T0917B d on e1.DepartmentID=d.ID
where 3>(
    select count(distinct e2.salary) from T0917 e2
    where e2.Salary>e1.Salary and e1.DepartmentID=e2.DepartmentID
    )
order by d.Name;
```

假设e1=e2=[6,5,4,3]，则子查询的过程如下：
 1、e1.Salary=3；则e2.Salary可以取4、5、6；COUNT(DISTINCT e2.Salary)=3
 2、e1.Salary=4；则e2.Salary可以取5、6；COUNT(DISTINCT e2.Salary)=2
 3、e1.Salary=5；则e2.Salary可以取6；COUNT(DISTINCT e2.Salary)=1
 4、e1.Salary=6；则e2.Salary无法取值；COUNT(DISTINCT e2.Salary)=0
 则要令COUNT(DISTINCT e2.Salary) < 3 的情况有上述的4、3、2.
 也即是说，这**等价于取e1.Salary最大的三个值。**

# 2021-9-18

有如下一张表T0918

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qHF17d3unQgxJIbNu4R1PJkCmK5nnL4J2rDpZ7kEicdmU0vGNNHEia55JjN7GGslqKdiav6DsBMSapKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

希望得到如下结果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/icbViakEeV5qHF17d3unQgxJIbNu4R1PJkdtsVRiawcy2icu0NBbaGicicibRElJsWhBvVJ8cJLsFxOrlgrttAmfkBxJQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

即对相同的No进行转置



**测试数据**



```
CREATE TABLE T0918 (
No INT,
NAME NVARCHAR(20),
age INT)

INSERT INTO T0918 VALUES (1,'张三','18');
INSERT INTO T0918 VALUES (1,'李四','17');
INSERT INTO T0918 VALUES (1,'王五','23');
INSERT INTO T0918 VALUES (1,'赵六','40');
INSERT INTO T0918 VALUES (2,'Tom','17');
INSERT INTO T0918 VALUES (3,'Bob','19');
INSERT INTO T0918 VALUES (3,'Tony','36');
INSERT INTO T0918 VALUES (3,'Petter','25');
```

答案：

```
with CET AS(
    select *,row_number() over (partition by NO order by (select 1))
    as RID from T0918
)
select NO
,MAX(case when RID=1 THEN NAME ELSE NULL END) AS　NAME1
,MAX(case when RID=1 THEN AGE ELSE NULL END) AS　AGE1
,MAX(case when RID=2 THEN NAME ELSE NULL END) AS　NAME2
,MAX(case when RID=2 THEN AGE ELSE NULL END) AS　AGE2
,MAX(case when RID=3 THEN NAME ELSE NULL END) AS　NAME3
,MAX(case when RID=3 THEN AGE ELSE NULL END) AS　AGE3
,MAX(case when RID=4 THEN NAME ELSE NULL END) AS　NAME4
,MAX(case when RID=4 THEN AGE ELSE NULL END) AS　AGE4
FROM CET AS　A
GROUP BY NO;
```

解释：

**with as 定义**

with A as (select * from class)

也就是将重复用到的大批量 的SQL语句，放到with as 中，加一个别名，在后面用到的时候就可以直接用。对于大批量的SQL数据，起到优化的作用。

**注意点：**

1、with子句只能被select查询块引用

2.with子句的返回结果存到用户的临时表空间中，只做一次查询，反复使用,提高效率。

3.在同级select前有多个查询定义的时候，第1个用with，后面的不用with，并且用逗号隔开。

4.最后一个with 子句与下面的查询之间不能有逗号，只通过右括号分割,with 子句的查询必须用括号括起来

**CASE有两种格式**

简单CASE函数：

```
case sex 
when 1 then '男'
when 0 then '女'
else 其他  end
```



CASE搜索函数：

```
case 
when sex=1 then '男'
when sex=0 then '女'
else 其他  end
```
