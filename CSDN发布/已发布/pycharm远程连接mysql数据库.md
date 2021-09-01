# pycharm远程连接mysql数据库

# 1.直接连接失败

![image-20210713101201202](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713101201202.png)

![image-20210708174038356](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708174038356.png)

![image-20210713103649122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713103649122.png)

# 2.mysql需要修改配置

配置文件一般默认在：/etc/mysql/mysql.conf.d/mysqld.cnf 

![image-20210713101853463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713101853463.png)

bind-address默认是127.0.0.1，只能localhost访问，需要注释掉或者改成0.0.0.0。

# 3.检查一下网络是不是通的

（1）首先尝试从pycharm机器ping数据库机器，没有成功。这时候从pycharm机器ssh是可以连接到数据库服务器的。原因是没有开启ping服务。

（2）开启linux的ping服务

开启     /sbin/sysctl -w net.ipv4.icmp_echo_ignore_all=0

关闭    /sbin/sysctl -w net.ipv4.icmp_echo_ignore_all=1

这时候可以ping成功了，说明网络没有问题的

![image-20210708175039437](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708175039437.png)



# 4.检查3306端口是不是能telnet

```
telnet 192.168.XX.XX 3306
```

返回连接失败，这个地方花了好多时间，最后发现是mysql配置的问题。还是打开配置文件/etc/mysql/mysql.conf.d/mysqld.cnf 

就红色框里这句话，导致端口不能用。删掉后就重启mysql服务。

![image-20210708175201618](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708175201618.png)

看这个skip_networking是OFF就可以了。

![image-20210708175326109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708175326109.png)

# 5.pycharm测试连接返回拒绝访问

![image-20210708175508729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708175508729.png)

这是mysql的授权问题

问题出在mysql数据库下面的user表。

（1）首先是要确认root用户的host是不是“%”，如果是localhost就不能远程访问，需要改成%；

（2）然后看下plugin，不能是soketXXX那种，是的话也要改一下

![image-20210708180159620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708180159620.png)

（3）然后修改授权：

```
mysql> alter user root identified with caching_sha2_password by '密码';
```

（4）修改user表之后需要执行这个命令生效：

```
mysql> flush privileges;
```

终于，pycharm可以正常连接远程的数据库了

![image-20210708180439024](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210708180439024.png)


