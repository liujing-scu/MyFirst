常见异常的解决

| 报错内容          | 含义                       |
| ----------------- | -------------------------- |
| SyntaxError       | 语法错误                   |
| NameError         | 尝试访问一个没有申明的变量 |
| ZeroDivisionError | 除数为 0 错误（零除错误）  |
| ValueError        | 数值错误                   |
| TypeError         | 类型错误                   |
| AttributeError    | 访问对象的不存在的属性     |
| IndexError        | 索引越界异常               |
| KeyError          | 字典的关键字不存在         |

# ValueError: port should be of type int	

不记得pymysql.connect的参数顺序，就把名字加=都写出来，不然顺序错了返回port类型不对

```
self.db = pymysql.connect(host=self.host, port=self.port, user=self.user, password=self.passwd,
                       database=self.database)
```

# (1054, "Unknown column '***' in 'field list'") 或

# pymysql.err.ProgrammingError: (1064, "You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 1")

sql语句有问题

下面这种才正确，最好用三引号。

```
self.cursor.execute(""" insert into my_test (id, site_id, chat_type, chat_content, check_status, row_status) values(%s, %s, %s, %s, %s, %s) """ ,\
      (item['id'], item['site_id'], item['chat_type'], item['chat_content'], item['check_status'], item['row_status']))
self.db.commit()
```

