





# json库

## 1.\_\_all\_\_查看json库的全部方法

```
import json
print(json.__all__)
```

['dump', 'dumps', 'load', 'loads', 'JSONDecoder', 'JSONDecodeError', 'JSONEncoder']

## 2. load和dump

json.load()用来将读取json文件，json.dump()用来将数据写入json文件

### 2.1 读取json数据

```
with open('1.json', 'r') as f:
    data = json.load(f)
    
```

### 2.2 写入json数据

```
with open('2.json', 'w') as f:   
    json.dump(data, f)
```



## 3. loads和dumps

json.dumps 将 Python 对象编码成 JSON 字符串
json.loads 将已编码的 JSON 字符串解码为 Python 对象

### 3.1 参数介绍

（1）sort_keys是告诉编码器按照字典key排序(a到z)输出。
（2）indent参数根据数据格式缩进显示，读起来更加清晰, indent的值，代表缩进空格式。
（3）separators参数的作用是去掉‘，’ ‘：’后面的空格，在传输数据的过程中，越精简越好，冗余的东西全部去掉。
（4）输出真正的中文需要指定ensure_ascii=False;默认使用的ascii编码。
（5）skipkeys参数，在encoding过程中，dict对象的key只可以是string对象，如果是其他类型，那么在编码过程中就会抛出ValueError的异常。skipkeys可以跳过那些非string对象当作key的处理。


### 3.2 把python字典数据格式化成json字符串输出

```
data = [{ 'a' : 1, 'b' : 2, 'c' : 3, 'd' : 4, 'e' : 5 }]
jsondata = json.dumps(data[0],sort_keys=True,indent=4,separators=(',',':'))
print(type(jsondata))  #<class 'str'>
print(jsondata)
```

### 3.3 把json数据格式化成python对象

```
text = json.loads(jsondata) 
print(type(text))  #<class 'dict'> 
print(text)
```

# sys库

sys.argv[0]：表示代码本身的文件路径

sys.argv[]说白了就是一个从程序外部获取参数的桥梁

sys.argv其实可以看作是一个列表

其第一个元素是程序本身，随后才依次是外部给予的参数

(1)第一个元素

```
import sys
a=sys.argv[0]
print("a:",a)
```

windows的cmd命令：

```
E:\work\my_study\sp\pachong1\pachong1\spiders>python tianqi.py
a: tianqi.py

E:\work\my_study\sp\pachong1\pachong1\spiders>python tianqi.py aaa
a: tianqi.py
```

（2）第二个元素

```
import sys
a=sys.argv[1]
print("a:",a)
```

windows的cmd命令：

```
E:\work\my_study\sp\pachong1\pachong1\spiders>python tianqi.py aaa
a: aaa
```

（3）第二个元素及之后的元素

```
import sys
a=sys.argv[1:]
print("a:",a)
```

windows的cmd命令：

```
E:\work\my_study\sp\pachong1\pachong1\spiders>python tianqi.py aaa b c
a: ['aaa', 'b', 'c']
```

# re库

1.匹配结尾用“$”

```
import re
chat="盘点一些你意想不到的冷知识_ESO_这里画棠此贴(年啊)为转侵权即删(我啊)"
img_partern = re.sub("\(\w\w\)$","",chat)
print(img_partern)
```

盘点一些你意想不到的冷知识_ESO_这里画棠此贴(年啊)为转侵权即删

2.匹配开始用“^‘

^符会强制要求表达式从字符串开头匹配

2.match

3.search

# 爬虫相关

1.爬取

urllib，request

2.解析

beautiful soup，xpath，pyquery

## xpath

1.使用变量的方法

```
response.xpath('//div[@id=$val]/a/text()', val='images').extract_first()
```

2.contains的使用方法

```
data_list = obj.xpath('.//div[contains(@class,"d_post_content j_d_post_content")]/text()')
```

