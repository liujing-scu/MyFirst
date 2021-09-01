# 爬取微信公众号文章

## 获取微信公众号的url

![1618473527007](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1618473527007.png)

## 获取每一篇文章的url

​	选择一个公众号进入，选择一个目录进入后点复制链接，然后去浏览器打开。按F12打开检查的模式，在Console中输入$x('标签路径')找到子文章的目录xpath，然后分离出每篇文章的url，标题。代码如下：

```
url_xpath_list = response.xpath('//section[@style="margin-left: -20px; max-width: 100%; box-sizing: border-box !important; word-wrap: break-word !important;"]/p//span')
for r in url_xpath_list:
	url = r.xpath('./a/@href').extract()[0]
	title_text = r.xpath('./a/text()').extract()[0]
    index1 = r.xpath('./text()').extract()[0]
    index2 = r.xpath('./text()').extract()[1]
    title = index1 + title_text + index2 + ".html"

    yield scrapy.Request(url, callback=self.get_context,meta={'title': title})
```

注意：需要在标题后面加.html，不然格式不对，打不开。

## 获取每一篇文章的html

通过xpath，获取到每篇文章的html，代码如下：

```
html = "".join(response.xpath('//html').extract())    
```

将获取到的每篇文章的标题作为每篇文章的文件名，保存到本地：

```
file_path = f'/home/jingliu/hupu/{title}.html'
with open(file_path, 'a+', encoding='utf-8') as fp:
	fp.write(html)
```

下面是每篇文章的html文件：

![image-20210416151213285](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210416151213285.png)

## html转换为pdf

这个部分比较麻烦，开始的时候没有经验我是抓取的文字出来，发现格式乱七八糟，根本不能看，后来想着直接将html转换为pdf，格式也原封不动，这样就方便阅读了。

### 准备工作

#### 1.pip安装pdfkit 

​	pip install pdfkit

#### 2.下载wkhtmltox工具

​    下载路径：  https://wkhtmltopdf.org/ 

#### 3.环境变量的添加

​    添加wkhtmltox的bin路径到系统环境变量path中

​	![1618475764146](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1618475764146.png)

### 转换的操作

#### 1.必须要将wkhtmltopdf.exe的路径配置好，不然会出错。

```
config_pdf = pdfkit.configuration(wkhtmltopdf=r'E:\downloads\wkhtmltox-0.12.6-1.mxe-cross-win64\wkhtmltox\bin\wkhtmltopdf.exe')
```

#### 2.用pdfkit将html格式的文件转换为pdf

​	pdfkit可以有3种转换的格式，分别是文件、字符串、url，它们的使用方法类似，只是第一个参数不同，file的要放文件的路径，string的放字符串，url放一个url链接：

​	pdfkit.from_file

​	pdfkit.from_string

​	pdfkit.from_url

这里我使用的是file的方式。

（1）首先获取所有file的路径，放在list中

```
import os
files = os.listdir(r'E:\work\natural_language\自然语言学习微信文章')
```

（2）读取html文件，转换成pdf格式

```
wkhtmltopdf_options = {
    '--enable-local-file-access': None
}
for file in files:
    name = re.sub('.html','',file)
    try:
        pdfkit.from_url(rf'E:\work\natural_language\自然语言学习微信文章\{file}',rf'E:\{name}.pdf',configuration=config_pdf,options=wkhtmltopdf_options)
    except:
        print("error")
```

这里文件名用正则表达式处理了一下，直接保存为原文件的名字加.pdf。

有个点需要注意，直接运行转换pdf代码是会有错误抛出的，网上说的是因为html中有css，或者链接之类的杂七杂八的东西，就会转换抛出错误。这错误不好修正，并且发现其实pdf文件已经生成好了的，其实就是可以略过这个错误。如果不加try-except，运行一条循环后就会中断，加上后就可以继续执行。

到这一步已经能够转换出格式和原网页一样的pdf文档了，但是有一个问题，原网页中的图片一张都没有显示。

#### 3.图片的加载（有点麻烦，不知道有没有更简单的办法）

##### 3.1抓取图片保存到本地

在抓取的html中找到图片的url，然后抓取并保存，注意名字中给特殊符号去掉

```
url_list = re.findall(r'<img.+?data-src="(.+?)"', html)
for url in url_list:
    yield scrapy.Request(url, callback=self.get_img)
```

```
def get_img(self,response):
    res = response.body
    url = response.request.url
    p = re.sub(r"[//:.?]", '', url)
    path = r"/home/jingliu/hupu/img/" + p + r".jpg"

    with open(path, 'wb') as f:
        f.write(res)
```

![image-20210423151516797](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423151516797.png)

##### 3.2图片路径换成本地路径

首先获取到所有的html的文件路径，读取内容，匹配出图片路径替换成本地图片路径（注意一点，data-src要替换成src，不然加载不出图片），然后新的html文件保存在一个新的文件夹“洗好的html”里。

```
import os
import re

files = os.listdir(r'E:\微信爬取文章\html')
for file in files:
    f = open(rf'E:\微信爬取文章\html\{file}','r',encoding='utf-8')
    #以字符串的形式读取
    content = f.read()

    img_url = re.findall(r'<img.*?data-src="(http.+?)"',content,re.S)
    for img in img_url:
        img_path_tail = re.sub(r"[//:.?=]",'',img)
        img_path = r'E:\微信爬取文章\img\\' + img_path_tail + ".jpg"
        img = re.sub(r'\?', '\?', img)
        content = re.sub(img,img_path,content)
    content = re.sub('<img data-src','<img src',content)
    print(content)
    f.close()
    fp = open(rf'E:\微信爬取文章\洗好的html\{file}','w',encoding='utf-8')
    fp.write(content)
    fp.close()
```

##### 3.3将新的html转化为pdf

```
wkhtmltopdf_options = {
    '--enable-local-file-access': None
}
config_pdf = pdfkit.configuration(wkhtmltopdf=r'E:\downloads\wkhtmltox-0.12.6-1.mxe-cross-win64\wkhtmltox\bin\wkhtmltopdf.exe')
files = os.listdir(r'E:\微信爬取文章\洗好的html')
for file in files:
    #将文件扩展名.html去掉
    name = re.sub('.html','',file)
    try:
        pdfkit.from_url(rf'E:\微信爬取文章\洗好的html\{file}',rf'E:\微信爬取文章\pdf\{name}.pdf',configuration=config_pdf,options=wkhtmltopdf_options)
    except:
        print("error")
```

这样一批文章就正确转化为pdf了，下面是部分截图，格式很规整，和页面一模一样。

![image-20210423154527195](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423154527195.png)

这是转换好的pdf，现在正在学习自然语言处理，直接下载到本地学习，方便很多。

![image-20210423155839092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423155839092.png)



我也是在不断学习中，这篇文章断断续续写了好多天，今天终于完成了，期间遇到了很多问题，后来都自己一一解决了，这就是进步吧。第一次发文章，不足之处请大家不吝赐教，愿大家一起进步！

