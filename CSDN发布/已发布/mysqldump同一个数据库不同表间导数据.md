# 1.mysqldump的基本语法

mysqldump：是mysql数据库备份用的命令。也可以用于数据表之间数据的导入导出。

```
导出：mysqldump -u用户名 -p密码 数据库 数据表  > XXX.sql

导入：mysql -u用户名 -p密码 数据库< XXX.sql
```

这样导出XXX.sql中带有drop和create数据表的sql语句。

# 2.参数 --no-create-info

如果不想覆盖导入数据的表，要在导出的时候加一个参数 --no-create-info，这样就不会有drop和create的语句，只执行inser into。

```
mysqldump -u用户名 -p密码 --no-create-info 数据库 数据表  > XXX.sql
```

# 3.实例

将表tb1的数据导入tb2中，tb2原来的数据不被覆盖。

```
#导出tb1的数据到my_bak.sql
mysqldump -udev -pdevppp --no-create-info my_db tb1 > my_bak.sql
```

这样my_bak.sql中的SQL全部都是基于tb1的表名，我们需要手动将my_bak.sql中de "tb1"改成"tb2"

```
vim my_bak.sql

:%s/tb1/tb2/g
```

为了以防万一，全文检查一下还有没有tb1

```
:?tb1
```

确认好之后执行下面这句：

```
#将tb1的数据以追加的方式导入tb2
mysql -udev -pdevppp my_db< my_bak.sql
```

