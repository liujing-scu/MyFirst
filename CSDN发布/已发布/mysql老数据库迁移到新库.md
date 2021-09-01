# 旧数据迁移到新装的数据库

环境：ubuntu 20-04 mysql 8.0

背景：之前的数据库在另一台机器上不能登录mysql数据库了，也不能使用mysqldump备份数据，只有mysql那个文件包了（里面有原始数据库的数据）。需要将这些数据迁移到一台新机器的mysql数据库。

# 1.新机器上的操作

## 1.1 关闭新的数据库服务

```
service mysql stop
```

## 1.2 获取旧数据库的数据 

在两台linux电脑之间使用远程命令scp传输数据库的数据

```
scp -r /data/mysql/* XXX@192.168.XX.XX:/home/XXX/mysql/
```

不能直接传输到新数据库的/var/lib/mysql，因为我没有用root@。因为不是我装的电脑，root的密码我不知道。

## 1.3 将数据放入对应的路径

一般默认的数据存放路径：/var/lib/mysql

```
mv /home/XXX/mysql/* /var/lib/mysql
```

## 1.4 修改权限

```
chown -R mysql:mysql /var/lib/mysql
```

不修改的话数据库启动不成功

## 1.5 启动mysql

```
service mysql start
```

## 1.6 登录成功

```
mysql -u 用户名 -p 密码
```

尝试登录成功，数据可以正常读取


