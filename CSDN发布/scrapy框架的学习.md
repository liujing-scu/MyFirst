scrapy框架学习

# 1.scrapy的基础概念

scrapy是一个为了爬取网站数据，提取结构性数据而编写的应用框架，我们只需要实现少量的代码，就能够快速的抓取。

scrapy使用了twisted**异步**网络框架，可以加快我们下载的速度。

异步：调用在发出之后，这个调用就直接返回，不管有无结果

非阻塞：关注的是程序在等待调用结果（消息，返回值）时的状态，指在不能立刻得到结果之前，该调用不会阻塞当前线程。

# 2.scrapy的工作流程

![image-20210531144125287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531144125287.png)

scrapy engine（引擎）：总指挥，负责数据和信号在不同模块间的传递。scrapy已经实现。

scheduler（调度器）：一个队列，存放引擎发过来的request请求。scrapy已经实现。

downloader（下载器）：下载把引擎发过来的requests请求，并返回给引擎。scrapy已经实现。

spider（爬虫）：处理引擎发过来的response，提取数据，提出url，并交给引擎。**需要手写**

item pipeline（管道）：处理引擎传过来的数据，比如存储。**需要手写**

downloader middlewares（下载中间件）：可以自定义的下载扩展，比如设置代理。一般不用手写

spider middlewares（爬虫中间件）：可以自定义requests请求和进行response过滤。一般不用手写

![image-20210531155118212](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531155118212.png)

# 3.scrapy的入门使用

## 3.1 创建一个scrapy项目

scrapy startproject mySpider

![image-20210531150935470](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531150935470.png)

（1）scrapy.cgc配置文件

![image-20210531150751382](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531150751382.png)

deploy：发布到本机上执行

（2）items.py

要爬取哪些字段，在这里写出来

（3）

## 3.2 生成一个爬虫

scrapy genspider itcast “itcast.cn”

## 3.3 提取数据

完善spider，使用xpath等方法

## 3.4 保存数据

pipeline中保存数据

# 4.scrapy框架的使用

全局命令

![image-20210531155900352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531155900352.png)

![image-20210531155754904](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531155754904.png)

项目命令

http://fang.5i5j.com/bj/loupan/

![image-20210531161021729](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531161021729.png)

# 5. Selector选择器

对于爬取信息的解析，我们在之前已经介绍了正则re、Xpath、beautiful soup和PyQuery。

而scrapy还给我们提供了自己的数据解析方法，即selector（选择器）。

selector（选择器）是基于lxml来构建的，支持xpath、css选择器以及正则表达式，功能全面，解析速度和准确度非常高。

## 5.1 直接使用：

selector（选择器）是一个可以独立使用模块。直接导入模块，就可以实例化使用，如下所示：

from scrapy import Selector

```
E:\work\fangdemo>python
Python 3.6.8 (tags/v3.6.8:3c6b436a57, Dec 24 2018, 00:16:47) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
```

解析content

content="<html><head><title>My html</title><body><h3>Hello World!</h3></body></head></html>"

（1）用xpath

>>> ```
>>> from scrapy import Selector
>>> content="<html><head><title>My html</title><body><h3>Hello World!</h3></body></head></html>"
>>> selector = Selector(text=content)
>>> selector.xpath("//title/text()").extract()
>>> ['My html']
>>> selector.xpath("//title/text()").extract_first()
>>> 'My html'
>>> ```
>>>
>>> 

用extract（）是老的方式，现在可以直接用get（）

>>> ```
>>> selector.xpath("//title/text()").get()
>>> 'My html'
>>> selector.xpath("//title/text()").getall()
>>> ['My html']
>>> ```
>>>
>>> 

（2）用css

>>> selector.css("title::text").get()
>>> 'My html'

（3）用正则表达式

>>> ```
>>> selector.re("<title>(.*?)</title>")[0]
>>> 'My html'
>>> selector.re_first("<title>(.*?)</title>")
>>> 'My html'
>>> ```
>>>
>>> 

## 5.2 scrapy shell

我们借助于scrapy shell来模拟请求的过程，然后把一些可操作的变量传递给我们，如request、response等。

>>> ```
>>> >>> quit()
>
>>> E:\work\fangdemo>scrapy shell http://www.baidu.com
>>>
>>> ```

```
In [1]: response.url
Out[1]: 'http://www.baidu.com'

In [2]: response.status
Out[2]: 200
```

![image-20210601100217390](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210601100217390.png)

xpath和css的selector是可以省略的，re的selector不能省略：

```
In [3]: response.selector.xpath("//title/text()").get()
Out[3]: '百度一下，你就知道'

In [4]: response.xpath("//title/text()").get()
Out[4]: '百度一下，你就知道'

In [5]: response.selector.css("title::text").get()
Out[5]: '百度一下，你就知道'

In [6]: response.css("title::text").get()
Out[6]: '百度一下，你就知道'

In [7]: response.selector.re_first("<title>(.*?)</title>")
Out[7]: '百度一下，你就知道'

In [8]: response.re_first("<title>(.*?)</title>")
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-8-213541fbc5ed> in <module>
----> 1 response.re_first("<title>(.*?)</title>")

AttributeError: 'HtmlResponse' object has no attribute 're_first'

```

提取所有链接信息：

```
In [9]: alist = response.xpath("//a")

In [10]: for vo in alist:
    ...:     print(vo.css("::attr(href)").get(),":",vo.css("::text").get())
    ...: 
http://news.baidu.com : 新闻
http://www.hao123.com : hao123
http://map.baidu.com : 地图
http://v.baidu.com : 视频
http://tieba.baidu.com : 贴吧
http://www.baidu.com/bdorz/login.gif?login&tpl=mn&u=http%3A%2F%2Fwww.baidu.com%2f%3fbdorz_come%3d1 : 登录
//www.baidu.com/more/ : 更多产品
http://home.baidu.com : 关于百度
http://ir.baidu.com : About Baidu
http://www.baidu.com/duty/ : 使用百度前必读
http://jianyi.baidu.com/ : 意见反馈
```

## 5.3 xpath选择器

response.selector属性返回内容相当于response的body构造了一个selector对象。

selector对象可以调用xpath()方法实现信息的解析提取。

（1）在xpath()后使用extract()可以返回所有元素结果

（2）若xpath()有问题，那么extract()会返回一个空列表

（3）在xpath()后使用extract_first()可以返回第一个元素结果

使用scrapy shell爬取“淘宝网”-》“商品分类”-》“特色市场”的信息。

https://www.taobao.com/tbhome/page/special-markets?spm=a21bo.7723600.8219.2.2df95ec9645ARL

```
E:\work\fangdemo>scrapy shell https://www.taobao.com/tbhome/page/special-markets?spm=a21bo.7723600.8219.2.2df95ec9645ARL

In [1]: response.url
Out[1]: 'https://www.taobao.com/tbhome/page/special-markets?spm=a21bo.7723600.8219.2.2df95ec9645ARL'

In [2]: response.status
Out[2]: 200

In [3]: response.xpath("//dt/text()").getall()
Out[3]: ['时尚爆料王', '品质生活家', '特色玩味控', '实惠专业户']

In [5]: dlist = response.xpath("//dl[@class='market-list']")

In [6]: for dl in dlist:
   ...:     print(dl.xpath("./dt/text()").get())
   ...:     print("="*50)
   ...:     alist = dl.xpath(".//a")
   ...:     for a in alist:
   ...:         print(a.xpath("./@href").get(),end=":")
   ...:         print(a.xpath("./span[@class='market-list-title']/text()").get())
   ...: 

时尚爆料王
==================================================

https://if.taobao.com/:潮流从这里开始
https://guang.taobao.com/:外貌协会の逛街指南
https://mei.taobao.com/:妆 出你的腔调
https://g.taobao.com/:探索全球美好生活
//star.taobao.com/:全球明星在这里
https://mm.taobao.com/:美女红人集中地
https://www.taobao.com/markets/designer/stylish:全球创意设计师平台

品质生活家
==================================================

https://chi.taobao.com/chi/:食尚全球 地道中国
//q.taobao.com:懂得好生活
https://www.jiyoujia.com/:过我想要的生活
https://www.taobao.com/markets/sph/sph/sy:尖货奢品品味选择
https://www.taobao.com/markets/qbb/index:享受育儿生活新方式
//car.taobao.com/:买车省钱，用车省心
//sport.taobao.com/:爱上运动每一天
//zj.taobao.com:匠心所在  物有所值
//wt.taobao.com/:畅享优质通信生活
https://www.alitrip.com/:比梦想走更远

特色玩味控
==================================================

https://china.taobao.com:地道才够味！
https://www.taobao.com/markets/3c/tbdc:为你开启潮流新生活
https://acg.taobao.com/:ACGN  好玩好看
https://izhongchou.taobao.com/index.htm:认真对待每一个梦想。
//enjoy.taobao.com/:园艺宠物爱好者集中营
https://sf.taobao.com/:法院处置资产，0佣金捡漏
https://zc-paimai.taobao.com/:超值资产，投资首选
https://paimai.taobao.com/:想淘宝上拍卖
//xue.taobao.com/:给你未来的学习体验
//2.taobao.com:让你的闲置游起来
https://ny.taobao.com/:价格实惠品类齐全

实惠专业户
==================================================

//tejia.taobao.com/:优质好货 特价专区
https://qing.taobao.com/:品牌尾货365天最低价
https://ju.taobao.com/jusp/other/mingpin/tp.htm:奢侈品团购第一站
https://ju.taobao.com/jusp/other/juliangfan/tp.htm?spm=608.5847457.102202.5.jO4uZI:重新定义家庭生活方式
https://qiang.taobao.com/:抢到就是赚到！
https://ju.taobao.com/jusp/nv/fcdppc/tp.htm:大牌正品 底价特惠
https://ju.taobao.com/jusp/shh/life/tp.htm?:惠聚身边精选好货
https://ju.taobao.com/jusp/sp/global/tp.htm?spm=0.0.0.0.biIDGB:10点上新 全球底价
https://try.taobao.com/index.htm:总有新奇等你发现
```

## 5.4 css选择器：

同xpath()一样

使用scrapy shell爬取“淘宝网”-》“商品分类”-》“特色市场”的信息。

```
In [7]: dlist = response.css("dl.market-list")

In [10]: for dl in dlist:
    ...:     print(dl.css("dt::text").get())
    ...:     print("="*50)
    ...:     alist = dl.css("a")
    ...:     for a in alist:
    ...:         print(a.css("::attr(href)").get(),end=":")
    ...:         print(a.css("span.market-list-title::text").get())
    ...: 

时尚爆料王
==================================================

https://if.taobao.com/:潮流从这里开始
https://guang.taobao.com/:外貌协会の逛街指南
https://mei.taobao.com/:妆 出你的腔调
https://g.taobao.com/:探索全球美好生活
//star.taobao.com/:全球明星在这里
https://mm.taobao.com/:美女红人集中地
https://www.taobao.com/markets/designer/stylish:全球创意设计师平台

品质生活家
==================================================

https://chi.taobao.com/chi/:食尚全球 地道中国
//q.taobao.com:懂得好生活
https://www.jiyoujia.com/:过我想要的生活
https://www.taobao.com/markets/sph/sph/sy:尖货奢品品味选择
https://www.taobao.com/markets/qbb/index:享受育儿生活新方式
//car.taobao.com/:买车省钱，用车省心
//sport.taobao.com/:爱上运动每一天
//zj.taobao.com:匠心所在  物有所值
//wt.taobao.com/:畅享优质通信生活
https://www.alitrip.com/:比梦想走更远

特色玩味控
==================================================

https://china.taobao.com:地道才够味！
https://www.taobao.com/markets/3c/tbdc:为你开启潮流新生活
https://acg.taobao.com/:ACGN  好玩好看
https://izhongchou.taobao.com/index.htm:认真对待每一个梦想。
//enjoy.taobao.com/:园艺宠物爱好者集中营
https://sf.taobao.com/:法院处置资产，0佣金捡漏
https://zc-paimai.taobao.com/:超值资产，投资首选
https://paimai.taobao.com/:想淘宝上拍卖
//xue.taobao.com/:给你未来的学习体验
//2.taobao.com:让你的闲置游起来
https://ny.taobao.com/:价格实惠品类齐全

实惠专业户
==================================================

//tejia.taobao.com/:优质好货 特价专区
https://qing.taobao.com/:品牌尾货365天最低价
https://ju.taobao.com/jusp/other/mingpin/tp.htm:奢侈品团购第一站
https://ju.taobao.com/jusp/other/juliangfan/tp.htm?spm=608.5847457.102202.5.jO4uZI:重新定义家庭生活方式
https://qiang.taobao.com/:抢到就是赚到！
https://ju.taobao.com/jusp/nv/fcdppc/tp.htm:大牌正品 底价特惠
https://ju.taobao.com/jusp/shh/life/tp.htm?:惠聚身边精选好货
https://ju.taobao.com/jusp/sp/global/tp.htm?spm=0.0.0.0.biIDGB:10点上新 全球底价
https://try.taobao.com/index.htm:总有新奇等你发现
```



# 6.spider的使用

在scrapy中，要抓取网站的链接配置、抓取逻辑、解析逻辑里其实都是在spider中配置的。

spider要做的事情就是有两件：**定义抓取网站的动作**和**解析爬取下来的网页**。

## 6.1 spider运行流程

整个抓取循环过程如下所述：

（1）以初始的URL初始化Request，并设置回调函数。请求成功时Request生成并作为参数传给该回调函数。

（2）在回调函数内分析返回的网页内容。返回结果两种形式，一种为字典或item数据对象；另一种是解析到下一个链接。

（3）如果返回的是字典或item对象，我们可以将结果存入文件，也可以使用pipeline处理并保存。

（4）如果返回request，response会被传递给request中定义的回调函数参数，即再次使用选择器来分析生成数据item。

## 6.2 spider类分析

## 6.3 实战案例

任务：使用scrapy爬取关键字为python信息的百度文库搜索信息（每页10条信息）

（1）设置ROBOTSTXT_OBEY = False，否则不能爬取

```
ROBOTSTXT_OBEY = False
```

![image-20210601141344435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210601141344435.png)

（2）设置CONCURRENT_REQUESTS，表示一次同时支持几个start_urls爬虫，默认是16个，多线程的，这里改成1，成了单线程，一次一个.

```
# Configure maximum concurrent requests performed by Scrapy (default: 16)
CONCURRENT_REQUESTS = 1
```



![image-20210601143018004](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210601143018004.png)



（3）设置DOWNLOAD_DELAY，间隔3s发起一个请求。

```
# Configure a delay for requests for the same website (default: 0)
# See https://docs.scrapy.org/en/latest/topics/settings.html#download-delay
# See also autothrottle settings and docs
DOWNLOAD_DELAY = 3
```

（4）爬取代码

```
import scrapy


class WenkuSpider(scrapy.Spider):
    name = 'wenku'
    allowed_domains = ['wenku.baidu.com']
    start_urls = ['https://wenku.baidu.com/search?word=python&pn=0']
    p = 0
    def parse(self, response):
        print("============正在爬取第%d页============"%(self.p+1))
        print(response.url)
        alist = response.css("dl dt a.tiaoquan")
        for a in alist:
            print(a.css("::attr(title)").get())
        #爬取下一页
        self.p += 1
        if self.p<10:
            next_url = "/search?word=python&pn=" + str(self.p*10)
            url = response.urljoin(next_url)#构建绝对地址
            yield scrapy.Request(url=url,callback=self.parse)
```

# 7. downloader middleware的使用

在downloader middleware的功能十分强大：可以修改user-agent、处理重定向、设置代理、失败重试、设置cookies等

downloader middleware在整个架构中起作用的位置是以下两个。

（1）在scheduler调度出队列的request发送给downloader下载之前，也就是我们可以在request执行下载前对其进行修改。

（2）在下载后生成的response发送给spider之前，也就是我们可以生成response被spider解析之前对其进行修改。

## 7.1 使用说明

在scrapy中已经提供了许多downloader middleware，如：负责失败重试，自动重定向等中间件：

他们都被定义带DOWNLOADER_MIDDLEWARE_BASE变量中。

C:\Users\Administrator\AppData\Local\Programs\Python\Python36\Lib\site-packages\scrapy\settings\default_settings.py

```
DOWNLOADER_MIDDLEWARES_BASE = {
    # Engine side
    'scrapy.downloadermiddlewares.robotstxt.RobotsTxtMiddleware': 100,
    'scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware': 300,
    'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware': 350,
    'scrapy.downloadermiddlewares.defaultheaders.DefaultHeadersMiddleware': 400,
    'scrapy.downloadermiddlewares.useragent.UserAgentMiddleware': 500,
    'scrapy.downloadermiddlewares.retry.RetryMiddleware': 550,
    'scrapy.downloadermiddlewares.ajaxcrawl.AjaxCrawlMiddleware': 560,
    'scrapy.downloadermiddlewares.redirect.MetaRefreshMiddleware': 580,
    'scrapy.downloadermiddlewares.httpcompression.HttpCompressionMiddleware': 590,
    'scrapy.downloadermiddlewares.redirect.RedirectMiddleware': 600,
    'scrapy.downloadermiddlewares.cookies.CookiesMiddleware': 700,
    'scrapy.downloadermiddlewares.httpproxy.HttpProxyMiddleware': 750,
    'scrapy.downloadermiddlewares.stats.DownloaderStats': 850,
    'scrapy.downloadermiddlewares.httpcache.HttpCacheMiddleware': 900,
    # Downloader side
}
```

字典格式，其中数字为优先级，越小的优先调用。

## 7.2 自定义downloader middleware中间件

我们可以通过项目的DOWNLOADER_MIDDLEWARE变量设置来添加自定义的downloader middleware。需要把setting.py的这个变量开启。

```
DOWNLOADER_MIDDLEWARES = {
    'middletest.middlewares.MiddletestDownloaderMiddleware': 543,
}
```

其中有3个核心方法：

（1）process_request(request,spider)

加代理

request.meta['proxy'] = "http://183.162.167.155:4216"

使用代理池：

```
import random
```

```
plist = [
    "http://183.162.161.155:4211"
    "http://183.162.162.155:4212"
    "http://183.162.163.155:4213"
    "http://183.162.164.155:4214"
    "http://183.162.165.155:4215"
]
request.meta['proxy'] = random.choice(plist)
```



（2）process_response(request,response，spider)

（3）process_exception(request,exception，spider)

## 7.3 实战案例

任务测试：通过http请求及响应的网站：http://httpbin.org/测试中间件的使用

网址：http://httpbin.org/get get请求

使用shell命令直接爬取报403/404错误

![image-20210601171926689](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210601171926689.png)

![image-20210601171932421](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210601171932421.png)

# 8.spidermiddleware中间件

spider中间件是介入到scrapy的spider处理机制的钩子框架，你可以添加代码来处理发送给spider的response及spider产生的item和request。

## 8.1 激活spider中间件

要启用spider中间件，你可以将其加入到SPIDER_MIDDLEWARE设置中。

```
SPIDER_MIDDLEWARES = {
    'middletest.middlewares.MiddletestSpiderMiddleware': 543,
}
```

# 9. ItemPipeline的使用

当item在spider中被收集之后，它将会被传递到item pipeline，一些组件会按照一定的顺序执行对item的处理。

每个item pipeline组件（有时称之为“item pipeline”）是实现了简单方法的python类。他们接收到item并通过它执行一些行为，同时也决定此item是否继续通过pipeline，或是被丢弃而不再进行处理。

以下是item pipeline的一些典型应用

（1）清理html数据

（2）验证爬取的数据（检查item包含某些字段）

（3）查重（并丢弃）

（4）将爬取结果保存到数据库中

官方文档：https://docs.scrapy.org/en/latest/topics/item-pipeline.html

## 9.1 如何编写你自己的item pipeline

编写你自己的item pipeline很简单，每个item pipeline组件是一个独立的python类，同时必须实现以下方法：

（1）process_item(item,spider)

每个item pipeline组件都需要调用该方法，这个方法必须返回一个item（或任何继承类）对象，或者抛出DropItem异常，被丢弃的item将不会被之后的pipeline组件所处理。

参数：

item（item对象）-被爬取的item

spider（spider对象）-爬取该item的spider

此外，他们也可以实现以下方法：

（2）open_spider(spider)

当spider被开启时，这个方法被调用

参数：spider（spider对象）-被开启的spider

（3）close_spider(spider)

当spider被关闭时，这个方法被调用

参数：spider（spider对象）-被关闭的spider

## 9.2 样例

启用item pipeline组件，必须要将它的类添加到ITEM_PIPELINE配置，就像这样

![image-20210604174902722](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210604174902722.png)





# 10. scrapy爬虫扩展中的日志处理

## 10.1 如何使用scrapy爬取信息不打印在命令窗口

终端输入scrapy crawl spider_name -s LOG_FILE=all.log

这样的话，log就输出到all.log文件中了，不会打印到屏幕

```
E:\work\learn_scrapy\story>scrapy crawl ppstory -s LOG_FILE=all.log
```

## 10.2 scrapy中的日志处理

scrapy提供了log功能，可以通过logging模块使用

（1）可以修改配置文件setting.py，任意位置添加下面两行

LOG_FILE="myspider.log"

LOG_LEVEL="INFO"

（2）scrapy提供5层logging级别：

CRITICAL-严重错误（critical）

ERROR-一般错误（regular errors）

WARNING-警告信息（warning messages）

INFO-一般信息（informational messas）

DEBUG-调试信息（debugging messas）

（3）logging设置

通过在setting.py中进行以下设置可以被用来配置logging：

LOG_ENABLED 默认：True，启用logging

LOG_ENCODING 默认：“utf-8”，logging使用的编码

LOG_FILE 默认：None，在当前目录里创建logging输出文件的文件名

LOG_LEVEL 默认“DEBUG”，log的最低级别

LOG_STDOUT 默认：False，如果是True，进程所有的标准输出（及错误）将会被重定向到log中。例如，执行 print "xxxxx" ，其将会在Scrapy log中显示

（4）记录信息

下面给出如何使用WARNING级别来记录信息

```
from scrapy import log
log.msg("This is a warning",level=log.WARNING)
```

```
scrapy.log.start(logfile=**None**, loglevel=**None**, logstdout=**None**)
```

启动log功能。该方法必须在记录任何信息之前被调用。否则调用前的信息将会丢失 

参数：

- logfile(str) - 用于保存log输出的文件路径。如果被忽略，LOG_FILE设置会被启用。如果两个参数都是None(默认值)，log会被输出到标准错误流(stderr)，一般都直接打印在终端命令行中。
- loglevel - 记录的最低日志级别，可用的值在上面提到了
- logstdout(boolean) - 如果设置为True，所有的应用的标准输出(包括标准错误)都将记录，例如，如果程序段中有 "print hello"，那么执行到这里时，"hello"也会被记录到日志中。

```
scrapy.log.msg(message,level=INFO,spider=None)
```

参数：

- message(str) - log信息
- level - 该信息对应的级别
- spider(spider 对象) - 记录信息的spider。当记录的信息和特定的spider有关联时，该参数必须使用
