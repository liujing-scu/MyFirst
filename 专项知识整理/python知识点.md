



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

