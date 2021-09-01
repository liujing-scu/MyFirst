# 1.必知必会，掌握HTTP基本原理

## 1.1 URL和URI

URI：（Uniform Resource Identifier）统一资源标志符

URL：（Uniform Resource Locator）统一资源定位符

例如：

https://github.com/favicon.ico既是一个URL，也是一个URI

访问协议HTTPS，访问路径（即根目录）和资源名称favicon.ico

URL是URI的一个子集。

URI还包括URN（Universal Resource Name）统一资源名称

URI只命名资源而不指定如何定位资源

比如：

URN：isbn：042324343指定了一本书的ISBN，可以唯一标识这本书，但是没有指定到哪里定位这本书

![image-20210713143259205](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713143259205.png)



目前网络URN使用少，一般URI和URL指的是一个东西。

## 1.2 超文本（Hypertext）

浏览器里看到的网页就是超文本解析而成的，其网页源代码是一系列HTML代码，里面包含了一系列标签。

比如：

img显示图片

p指定显示段落等

浏览器解析这些标签后，便形成了我们平常看到的网页，而网页的源代码HTML就可以称作**超文本**

## 1.3 HTTP和HTTPS

https://www.taobao.com/中URL的开头会有http或https

这就是访问资源需要的协议类型，有时还会看到ftp、sftp、smb开头的URL

ftp，sftp，smb都是指的协议类型

**HTTP**（Hyper Text Transfer Protocol）中文名叫超文本传输协议

用于从网络传输超文本数据到本地浏览器的传送协议，能保证高效而准确地传送超文本文档。

由万维网协会（World Wide Web Consortium）和Internet工作小组IETF（Internet Engineering Task Force）共同合作制定的规范。

**HTTPS**（Hyper Text Transfer Protocol over Secure Socket Layer）

是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，简称HTTPS安全基础是SSL，因此通过它传输的内容都是经过SSL加密的

主要作用可以分为两种：

（1）建立一个信息安全通道，来保证数据传输的安全

（2）确认网站的真实性，凡是使用了HTTPS的网站，都可以通过点击浏览器地址栏的锁头标志来查看网站认证之后的真实信息，也可以通过CA机构颁发的安全签章来查询。

越来越多的网站和APP都已经向HTTPS方向发展。

## 1.4 HTTP请求过程

在浏览器中输入一个URL，回车之后便可以在浏览器中观察到页面内容。在这个过程是浏览器向网站所在的服务器发送了一个请求，网站服务器收到这个请求之后进行处理和解析，然后返回对应的响应，接着传回给浏览器。

![image-20210713145432252](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713145432252.png)

打开Chrome浏览器，右击并选择“检查”项即可以打开浏览器的开发者工具。

## 1.5 请求

![image-20210713145920867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713145920867.png)

常见请求方法：

**GET**和**POST**

（1）GET请求

在浏览器中直接输入URL并回车，便发起了一个GET请求，请求的参数会直接包含在URL里

例如：在百度中搜索python，这就是一个GET请求，链接为https://www.baidu.com/s?wd=Python

URL中包含了请求的参数信息，这里参数wd表示要搜索的关键字。

（2）POST请求

POST请求大多在表单提交时发起

例如：对于一个登录表单，输入用户名和密码后，点击“登录”按钮，这时通常会发起一个POST请求，其数据通常以表单的形式传输，而不会体现在URL中。

（3）GET和POST请求的区别

GET请求中的参数是包含在URL中的，数据可以在URL中看到，而POST请求的URL不会包含这些数据，数据都是通过表单的形式传输的，会包含在请求体重

GET请求提交的数据最多只有1024字节，而POST请求没有限制。



![image-20210713150907740](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713150907740.png)

### 1.5.1 请求头

用来说明服务器要使用的附加信息，比较重要的信息有Cookies，Referer、User-Agent等。

常用头信息

（1）Accept：请求报头域，用于指定客户端可接受哪些类型的信息。

举例：

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9

（2）Accept-Language：指定客户端可接受的语言类型。

Accept-Encoding: gzip, deflate, br

（3）Accept-Encoding：指定客户端可接受的内容编码。

Accept-Language: zh-CN,zh;q=0.9

（4）Host：用于指定请求资源的主机IP和端口号，其内容为请求URL的原始服务器或网关的位置。从HTTP1.1版本开始，请求必须包含此内容。

Host: www.baidu.com

（5）Cookie：也常用复数形式Cookies，这是网站为了辨别用户进行会话跟踪而存储在用户本地的数据。它的主要功能是维持当前访问会话。

Cookie: PSTM=1610440905; BAIDUID=BBB8DFB5AD764BB4DF303FEA1E38C5F1:FG=1; BIDUPSID=8CA5AD67B5FCD539E0783B233565E56B; __yjs_duid=1_3db21fc10ef4d3fc1687c4c9e79d7f4f1622527575662; BAIDUID_BFESS=BBB8DFB5AD764BB4DF303FEA1E38C5F1:FG=1; Hm_lvt_aec699bb6442ba076c8981c6dc490771=1623115434; COOKIE_SESSION=4923390_0_9_6_17_7_1_0_9_4_5_0_5936492_0_6_0_1623115437_0_1623115431%7C9%2385854_34_1616642635%7C9; BDRCVFR[SatCmkszMoT]=vEecRnDhO2muA-9uhN8uz4WU6; delPer=0; BD_CK_SAM=1; PSINO=1; H_PS_PSSID=31253; BD_UPN=12314753;H_PS_645EC=59dekBpp%2BAkiikfz0ksjqRwHltS%2BgjufHyW2P3190QtyVl5iBrdRdyHJwkWrtJoUdTw2EkNzcnw9; BA_HECTOR=8h04ak852l21852lmg1geqfre0q; BDORZ=FFFB88E999055A3F8A630C64834BD6D0

（6）Referer：此内容用来标识这个请求是从哪个页面发过来的，服务器可以拿到这个信息并做相应的处理，如做来源统计、防盗链处理等。

4399的图片链接需要Referer才能访问

（7）User-Agent：简称UA，它是一个特殊的字符串头，可以使用服务器识别客户使用的操作系统及版本、浏览器及版本等信息。在做爬虫时加上此信息，可以伪装为浏览器。

user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.77 Safari/537.36

（8）Content-Type：也叫互联网媒体类型（Internet Media Type）或者MIME类型 ，在HTTP消息头中，他用来表示具体请求中的媒体类型信息。例如：text/html代表HTML格式，image/gif 代表GIF图片，application/json代表JSON类型，更多对应关系可以查看对照表：

http://tool.oschina.net/commons

### 1.5.2 请求体

请求体一般承载的内容是POST请求中的表单数据，而对于GET请求，请求体则为空。

![image-20210713155114481](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713155114481.png)



在爬虫时，如果要构造POST请求，需要使用正确的Content-Type，并了解各种请求库的各个参数设置时使用的是哪种Content-Type，不然可能导致POST提交后不能正常响应。

## 1.6 响应

响应，由服务端返回给客户端，可以分为3部分：响应状态码（Response Status Code）、响应头（Response Header）、响应体（Response Body）

### 1.6.1 响应状态码

常见的错误码和原因：

![image-20210713160527797](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713160527797.png)

![image-20210713160543836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713160543836.png)

![image-20210713160557923](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713160557923.png)

### 1.6.2 响应头

（1）Date：标识响应产生的时间

date: Tue, 13 Jul 2021 02:16:03 GMT

（2）Last-Modified：指定资源的最后修改时间

（3）Content-Encoding：指定响应内容的编码

content-type: text/html; charset=UTF-8

（4）Server：包含服务器的信息，比如名称、版本号等。

（5）Set-Cookie：设置Cookies。响应头中的Set-Cookie告诉浏览器需要将此内容放在Cookies中，下次请求携带Cookies请求。

（6）Expires：指定响应的过期时间，可以使代理服务器或浏览器将加载的内容更新到缓存中。如果再次访问时，就可以直接从缓存中加载，降低服务器负载，缩短加载时间。

### 1.6.3 响应体

最重要的当属响应体内容了。响应的正文数据都在响应体中，比如请求网页时，它的响应体就是网页的HTML代码；请求一张图片时，它的响应体就是图片的二进制数据。我们做爬虫请求网页后，要解析的内容就是响应体。

# 2. 夯实根基，Web网页基础

## 2.1 网页的组成

网页可以分为三大部分：HTML、CSS、JavaScript。如果把网页比作一个人的话，HTML相当于骨架，JavaScript相当于肌肉，CSS相当于皮肤，三者结合起来才能形成一个完善的网页。

（1）HTML

Typer Text Markup Language，即超文本标记语言。

不同类型的元素用不同标签表示，比如图片用img标签，视频用video标签，段落用p标签……它们之间的布局又通过布局标签div嵌套组合而成。

整个网页由各种标签嵌套组合而成，这些标签定义的节点元素相互嵌套和组合形成了复杂的层次关系，就形成了网页的架构。

（2）CSS

Cascading Style Sheets，即层叠样式表。

层叠：是指在HTML中引入了数个样式文件，并且样式发生冲突时，浏览器能依据层叠顺序处理。

样式：是指网页中文字的大小、颜色、元素间距、排列等格式。

CSS是目前唯一的网页页面排版样式标准，有了它的帮助，页面才会变得更为美观。

![image-20210713164602329](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713164602329.png)



这是一个CSS样式，大括号前面是一个CSS选择器，此选择器的意思是首先选中id为head_wrapper且class为s-ps-islite的节点，然后再选中其内部的class为s-p-top的节点。大括号内部写的就是一条条样式规则，例如：position指定了这个元素的布局方式为绝对布局，bottom指定元素的下边距为40像素，width指定了宽度为100%占满父元素，height则指定了元素的高度。也就是说，我们将位置

宽度、高度等样式配置统一写成这样的形式，然后用大括号括起来，接着在开头再加上CSS选择器，这就代表这个样式对CSS选择器选中的元素生效，元素就会根据此样式来展示了。

在网页中，一般会统一定义整个网页的样式规则，并写入CSS文件中（其后缀为css）。在HTML中，只需要用**link标签**即可引入写好的CSS文件，这样整个页面就会变得美观。

（3）JavaScript

简称JS，是一种脚本语言。HTML和CSS配合使用，提供给用户的只是一种静态信息，缺乏交互性。我们在网页里可能会看到一些交互和动画效果，如下载进度条，提示框、轮播图等，这通常是JS的功劳。它的出现使得用户与信息之间不只是一种浏览与显示的关系，而是实现了一种实时、动态、交互的页面功能。

JS通常也是以单独的文件形式加载的，后缀为js，在HTML中通过**script标签**即可引入，例如：

![image-20210713170206928](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713170206928.png)

总结：HTML定义了网页的内容和结构，CSS描述了网页的布局，JavaScript定义了网页的行为。

## 2.2 网页的结构

![image-20210713170703482](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713170703482.png)

这是一个最简单的HTML实例。开头用DOCTYPE定义了文档的类型，最外层是html标签，最后还有对应的结束标签来表示闭合。内部是head和body标签。

head：页面的配置和引用

body：定义网页的正文

## 2.3 节点树和节点间的关系

在HTML中，所有标签定义的内容都是节点，它们构成了一个HTML DOM树。

DOM： Document Object Model，即文档对象模型。它定义了访问HTML和XML文档的标准。

HTML DOM将HTML文档视作树结构，这种结构被称为节点树。

![image-20210713172056850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713172056850.png)

![image-20210713172214562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713172214562.png)

## 2.4 选择器

在CSS中，我们使用CSS选择器来定位节点。

常用的三种表示：

（1）id

div节点的id为container，可以表示为：#container，其中"#"开头代表选择id，其后面紧跟id的名称。

（2）class

class为wrapper的节点，可以表示为：.wrapper，其中"."开头代表选择class，其后面紧跟class的名称。

（3）标签名

还有一种是根据标签名筛选，例如想选二级标题，直接用h2即可。

另外，CSS选择器支持嵌套选择，各个选择器之间加上**空格分隔**开便可以代表**嵌套关系**，如：

#container .wrapper p

代表选择id为container的节点，然后选中其内部class为wrapper的节点，然后再进一步选中其内部的p节点。

另外，如果不加空格，代表并列关系。

div#container .wrapper p.text

代表选择id为container的div节点，然后选中其内部class为wrapper的节点，然后再进一步选中其内部的class为text的p节点。

CSS的其他语法如下：

**![image-20210713173730734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713173730734.png)**

![image-20210713173750287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713173750287.png)

![image-20210713173803153](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210713173803153.png)



另外，还有一种比较常用的选择器是XPath。

# 3. 原理探究，了解爬虫的基本原理

## 3.1 爬虫概述

简单来说，爬虫就是获取网页并提取和保存信息的自动化程序，下面概要介绍一下。

（1）获取网页

爬虫首先要做的工作就是获取网页，即网页源代码。python提供了这个操作的库：urllib，requests等。

（2）提取信息

分析网页源代码，从中提取我们需要的数据。

最通用的是正则表达式，但是构造正则表达式复杂容易出错。于是就有了CSS选择器，XPath选择器。

python提供了这个操作的库：Beautiful Soup、pyquery、lxml等。

（3）保存数据

TXT文本、JSON文本、数据库（MySql、MongoDB等）、远程服务器（借助SFTP）

（4）自动化程序

爬虫代替人工完成爬取工作，它可以进行各种异常处理、错误重试等操作，确保爬取持续高效地运行。

## 3.2 能抓怎样的数据

（1）HTML代码

（2）JSON字符串（其中API接口大多采用这样的形式）

（3）二进制数据（如：图片、视频、音频等）

（4）各种扩展名的文件（如：CSS、JavaScript和配置文件）

上述内容其实都对应各自的URL，是基于HTTP或HTTPS协议的，只要是这种数据，爬虫都可以抓取。

## 3.3 JavaScipt渲染页面

抓取到的源代码和浏览器中看到的不一样。现在越来越多网页采用Ajax、前端模块化工具来构建，整个网页可能由JavaScript渲染出来的，也就是说原始的HTML代码就是一个空壳，例如：

![image-20210715161752352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210715161752352.png)



body节点里面只有一个id为container的阶段，但是需要注意在body节点引入了app.js，它便负责整个网站的渲染。

在浏览器中打开这个页面时，首先会加载这个HTML内容，接着浏览器会发现其中引入了一个app.js文件，然后便会接着去请求这个文件，获取到该文件后，便会执行其中的JavaScript代码，而JavaScript则会改变HTML中的节点，向其添加内容，最后得到完整的页面。

因此，使用基于HTTP请求库得到的源代码可能跟浏览器中的页面源代码不太一样。对于这样的情况，我们可以分析其后台Ajax接口，也可以使用Selenium、Splash这样的库来实现模拟JavaScript渲染。

# 4.基础探索，Session与Cookies

经常遇到有的网页需要登录才能访问，而且登录了之后可以连续访问很多次，但是有时候过一段时间后就需要重新登录。还有些网站，在打开时就自动登录了，而且很长时间都不会失效。这些就是涉及Session（会话）和Cookies的相关知识了。

## 4.1 静态网页和动态网页

静态网页：HTML代码编写的，文字和图片等内容均通过写好的HTML代码来指定，它的加载速度快，编写简单，但是维护性差，不能根据URL灵活多变地显示内容（比如，我们想要给这个网页的URL传入一个name参数，让其在网页中显示，静态网页无法实现）

动态网页：可以动态解析URL中参数的变化，关联数据库并动态呈现不同的页面内容。我们现在遇到的大多数网站都是动态网站，他们不再是一个简单的HTML，而是可能由JSP，PHP，Python等语言编写的，其功能比静态网页强大和丰富太多了。

## 4.2 无状态HTTP

HTTP无状态是指：HTTP协议对事务处理是没有记忆能力的，也就是说服务器不知道客户端是什么状态。当我们向服务器发送请求后，服务器解析此请求，然后返回对应的响应，服务器负责完成这个过程，而且这个过程是完全独立的，服务器不会记录前后状态的变化，也就是缺少状态记录。这意味着如果后续需要处理前面的信息，则必须重传，这导致需要额外传递一些前面的重复请求，才能获取后续的响应，然而这种效果显然不是我们想要的。为了保持前后状态，我们肯定不能将前面的请求全部重传一次，这太浪费资源了，对于这种需要用户登录的页面来说更是棘手。

这时两个用于保持HTTP连接状态的技术就出现了，它们分别是Session（会话）和Cookies。会话在服务端，也就是网站的服务器，用来保存用户的会话信息；Cookies在客户端，也可以理解为浏览器端，有了Cookies，浏览器在下次访问网页时会自动附带上它发送给服务器，服务器通过识别Cookies并鉴定出是哪个用户，然后再判断用户是否是登录状态，然后返回对应响应。

### 4.2.1 会话

在web中，会话对象用来存储特定用户会话所需的属性及配置信息。这样，当用户在应用程序的Web页之间跳转时，存储在会话对象中的变量将不会丢失，而是在整个用户会话中一直存在下去。

### 4.2.2 Cookies

Cookies是指某些网站为了辨别用户身份、进行会话跟踪而存储在用户本地终端上的数据。

（1）会话维持

怎么利用Cookies保持状态呢？当客户端第一次请求服务器时，服务器会返回一个请求中带有Set-Cookies字段的响应给客户，用来标记是哪一个用户，客户端浏览器会把Cookies保存起来。当浏览器下一次请求该网站时，浏览器会把次Cookies放到请求头一起提交给服务器，Cookies携带了会话ID信息，服务器检查该Cookies即可找到对应的会话是什么，然后再判断会话来以此辨认用户状态。

Cookies和会话需要配合，一个处于客户端，一个处于服务端，二者共同协作，就实现了登录会话控制。

（2）属性结构

Cookies有哪些内容呢？

点击chrome检查-》Application-》左侧有Cookies选项

![image-20210716092758571](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210716092758571.png)

**Name**：该Cookie的名称。一旦创建，该名称便不可更改。

**Value**：该Cookie的值。如果值为Unicode字符，需要为字符编码。如果值为二进制数据，则需要使用BASE64编码。

**Domain**：可以访问该Cookie的域名。例如，如果设置为.zhihu.com，则所有以zhihu.com结尾的域名都可以访问该Cookie。

**Max-Age**：该Cookie失效的时间，单位为秒。也常和Expires一起使用，通过它可以计算出其有效时间。Max Age如果为正数，则该Cookie在Max Age秒之后失效。如果为负数，则关闭浏览器时Cookie即失效，浏览器也不会以任何形式保存该Cookie。

**Path**：该Cookie的使用路径。如果设置为/path/，则只有路径为/path/的页面可以访问该Cookie。如果设置为/，则本域名下的所有页面都可以访问该Cookie。

**Size字段**：此Cookie的大小。

**HTTP字段**：Cookie的httponly属性。若此属性为true，则只有在HTTP头中会带有此Cookie的信息，而不能通过document.cookie来访问此Cookie。

**Secure**：该Cookie是否仅被使用安全协议传输。安全协议有HTTPS和SSL等，在网络上传输数据之前先将数据加密。默认是false。



（3）会话Cookie和持久Cookie

从表面意思来说，

会话Cookie：就是把Cookie放在浏览器内存里，浏览器在关闭之后该Cookie即失效；

持久Cookie：则会保存到客户端的硬盘中，下次还可以继续使用，用于长久保持用户登录状态。

更严格来说，

没有会话Cookie和持久Cookie之分，只是由Cookie的Max Age或Expire字段决定了过期时间。

## 4.3 常见误区

只要关闭浏览器，会话就消失了。-------这是错误的。

正确的是：关闭浏览器不会导致会话被服务器删除，服务器会为会话设置一个失效时间，当距离客户端上次使用会话的时间超过这个这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把会话删除以节省存储空间。

# 5. 书上有视频没有的，代理的基本原理

服务器会检查某个IP在单位时间内的请求次数，超过了这个阈值，就会直接拒绝服务，返回一些错误信息，这种情况可以成为封IP。

既然服务器检测的是某个IP单位时间的请求次数，那么借助某种方式来伪装我们的IP，让服务器识别不出是由我们本机发起的请求，就可以防止封IP了。

一种有效的方式就是使用代理，后面会详细说明代理的用法。在这之前，需要先了解下代理的基本原理。

## 5.1 基本原理

代理指的就是代理服务器（proxy server），它的功能就是代理网络用户去取得网络信息。

本机——>代理服务器——>Web服务器

Web服务器——>代理服务器——>本机

这就成功地实现了伪装用户的本机IP

## 5.2 代理的作用

（1）突破自身IP的限制，访问一些平时不能访问的站点。

（2）访问一些单位或者团体内部资源：比如使用教育网内地址段免费代理服务器，就可以用于对教育网开放的各类FTP下载上传，以及各类资料查询共享等服务。

（3）提高访问速度：通常代理服务器都设置一个较大的硬盘缓冲区，当有外界的信息通过时，同时也将其保存到缓冲区，当其他用户再访问相同的信息时，则直接由缓冲区中取出信息，传给用户，以提高防伪速度。

（4）隐藏真实IP：上网者也可以通过这种方式隐藏自己的IP，免受攻击。对于爬虫来说，我们用代理就是为了隐藏自身IP，防止自身的IP被封。

## 5.3 爬虫代理

对于爬虫来说，由于爬虫爬取速度过快，在爬取过程中可能遇到同一个IP访问过于频繁的问题，此时网站就会让我们输入验证码登录或者世界封锁IP，这样会给爬取带来极大的不便。

使用代理隐藏真实的IP，让服务器误以为是代理服务器在请求自己。这样在爬取过程中不断更换代理，就不会被封锁，可以达到很好的爬虫效果。

## 5.4 代理分类

可以根据协议区分，也可以根据其匿名程度区分。

### 5.4.1 根据协议区分

（1）FTP代理服务器：主要用于访问FTP服务器，一般有上传、下载以及缓存功能，端口一般为21,2121等。

（2）HTTP代理服务器：主要用于访问网页，一般有内容过滤和缓存功能，端口一般为80、8080、3128等。

（3）SSL/TLS代理：主要用于访问加密网站，一般有SSL或TLS加密功能（最高支持128位加密强度），端口一般为443。

（4）RTSP代理：主要用于访问Real流媒体服务器，一般有缓存功能，端口一般为554。

（5）Telenet代理：主要用于telnet远程控制（黑客入侵计算机时常用于隐藏身份），端口一般为23。

（6）POP3/SMTP代理：主要用于POP3/SMTP方式收发邮件，一般有缓存功能，端口一般为110/25。

（7）SOCKS代理：只是单纯传递数据包，不关心具体协议和用法，所以速度快很多，一般有缓存功能，端口一般为1080。

### 5.4.2 根据匿名程度区分

（1）高度匿名代理：会将数据包原封不动地转发，在服务端看了就好像真的是一个普通客户端在访问，而记录的IP是代理服务器的IP。

（2）普通匿名代理：会在数据包上做一些改动，服务端上可能发现这是个代理服务器，也有一定几率追查到客户端的真实IP。代理服务器通常会加入的HTTP头有HTTP_VIA和HTTP_X_FORWARDED_FOR。

（3）透明代理：不但改动了数据包，还会告诉服务器客户端的真是IP。这种代理除了能用缓存技术提高浏览速度，能用内存过滤提高安全性之外，并无其他显著作用，最常见的例子就是内网中的硬件防火墙。

（4）间谍代理：指组织或个人创建的用于记录用户传输的数据，然后进行研究、监控等目的的代理服务器。



## 5.5 常见代理设置

（1）使用网上免费代理：最好使用高匿代理，另外可用的代理不多，需要在使用前筛选一下可用代理，也可以进一步维护一个代理池。

（2）使用付费代理服务：互联网上存在许多代理商，可以付费使用，质量比免费代理好很多。

（3）ADSL拨号：拨一次号换一次IP，稳定性高，也是一种比较有效的解决方案。



# 6. 多路加速，了解多线程基本原理

## 6.1 多线程的含义

进程：可以理解为一个可以独立运行的程序单位

比如：

打开一个浏览器，就开启了一个浏览器进程

打开一个文本编辑器，就开启了一个文本编辑器进程

一个进程中可以同时处理很多事情

比如：

浏览器中可以在多个选项卡中打开多个页面

有的页面在播放音乐，有的页面在播放视频，有的页面在播放动画

可以同时运行，互不干扰。

为什么能做到同时运行这么多任务呢？

任务对应着线程的执行

**进程**：是线程的集合，是由一个或多个线程构成的

**线程**：是操作系统进行运算调度的最小单位，是进程中的一个最小运行单元。

（1）并发concurrency

指同一时刻只能有一条指令执行，但多个线程的对应指令被快速地轮换执行，宏观上看起来多个线程在同时运行，但微观上只是这个处理器在连续不断地在多个线程之间切换和执行。

并发在单处理器和多处理器系统中都可以存在，仅靠一个核，就可以实现并发。

（2）并行parallel

指同一时刻有多条指令在多个处理器上同时执行，并行必须依赖于多个处理器，不论宏观还是微观上，多个线程都是在同一时刻一起执行的。

并行只能在多处理器系统中存在，如果计算机处理器只有一个核，就不能实现并行。

## 6.2 多线程的适用场景

在一个程序进程中，有些操作时比较耗时或者需要等待的

如：

等待数据库的查询结果的返回

等待网页结果的响应

使用多线程，处理器就可以在某个线程等待时去执行其他线程，从而从整体上提高执行效率。

网络爬虫就是一个非常典型的例子：

爬虫在向服务器发起请求之后，有一段时间必须要等待服务器的响应返回，这种任务就属于IO密集型任务。

不是所有的任务都是IO密集型任务

有一种任务叫做计算密集型任务，也可称之为CPU密集型任务，就是任务的运行一直需要处理器的参与。这种不适合多线程。

如果任务不全是计算密集型任务，可以使用多线程来提高程序整体的执行效率。尤其对于网络爬虫这种IO密集型任务来说，使用多线程会大大提高程序整体的爬取效率。

## 6.3 Python实现多线程

在Python中实现多线程的模块叫做threading，是Python自带的模块。

```
import threading
import time
def target(second):
    print(f'Threading{threading.current_thread().name} is running')
    print(f'Threading{threading.current_thread().name} sleep {second}s')
    time.sleep(second)
    print(f'Treading {threading.current_thread().name} is ended')
print(f'Treading {threading.current_thread().name} is running')
for i in [1,5]:
    t=threading.Thread(target=target,args=[i])
    t.start()
print(f'Threading {threading.current_thread().name} is ended')


```



打印结果：

Treading MainThread is running
ThreadingThread-1 is running
ThreadingThread-1 sleep 1s
ThreadingThread-2 is running
Threading MainThread is ended
ThreadingThread-2 sleep 5s
Treading Thread-1 is ended
Treading Thread-2 is ended

主线程先结束了。

修改加上join()

```
import threading
import time
def target(second):
    print(f'Threading{threading.current_thread().name} is running')
    print(f'Threading{threading.current_thread().name} sleep {second}s')
    time.sleep(second)
    print(f'Treading {threading.current_thread().name} is ended')
print(f'Treading {threading.current_thread().name} is running')
for i in [1,5]:
    t=threading.Thread(target=target,args=[i])
    t.start()
    t.join() #修改加上join()
print(f'Threading {threading.current_thread().name} is ended')


```

打印结果：

Treading MainThread is running
ThreadingThread-1 is running
ThreadingThread-1 sleep 1s
Treading Thread-1 is ended
ThreadingThread-2 is running
ThreadingThread-2 sleep 5s
Treading Thread-2 is ended
Threading MainThread is ended

主线程等子线程结束后再结束

另外，可以通过继承thread类来创建一个线程，该线程写在run方法执行即可-----下面有例子涉及了

守护线程，意味着不重要的，如果主线程结束了，守护线程还没执行完成，那么守护线程将被强制结束。

```
import threading
import time
def target(second):
    print(f'Threading{threading.current_thread().name} is running')
    print(f'Threading{threading.current_thread().name} sleep {second}s')
    time.sleep(second)
    print(f'Treading {threading.current_thread().name} is ended')
print(f'Treading {threading.current_thread().name} is running')
t1=threading.Thread(target=target,args=[2])
t1.start()
t2=threading.Thread(target=target,args=[5])
t2.setDaemon(True)
t2.start()
print(f'Threading {threading.current_thread().name} is ended')
```

打印结果：可以看到守护线程thread-2没有结束打印

Treading MainThread is running
ThreadingThread-1 is running
ThreadingThread-1 sleep 2s
ThreadingThread-2 is running
ThreadingThread-2 sleep 5s
Threading MainThread is ended
Treading Thread-1 is ended

如果调用join()方法，主线程仍然会等待各线程执行完毕再结束，无论该线程是不是守护线程。

## 6.4 互斥锁

在一个进程中的多个线程是共享资源的

```
import time
import threading
count=0
class MyThread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
        global count
        temp=count+1
        time.sleep(0.001)
        count=temp
threads=[]
for _ in range(1000):
    thread=MyThread()
    thread.start()
    threads.append(thread)
for thread in threads:
    thread.join()
print(f'Final count:{count}')
```

打印结果不是1000，是12

原因：count这个值是每个线程共享的，如果线程并发或并行执行时，不同线程拿到的count值可能是同一个值，导致有的线程的加1操作并没有生效，最后导致结果偏小。

这就涉及到对共享资源t进行加锁保护：

某个线程在对数据进行操作前，需要先加锁

这样其他的线程发现被加锁了之后，就无法继续向下执行，会一直等待锁被释放

只有加锁的线程把锁释放了，其他的线程才能继续加锁并对数据做修改，修改完了再释放锁。

这样可以确保同一时间只有一个线程操作数据

多个线程不会再同时读取和修改同一个数据了

```
import threading
import time
count=0
class MyThread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)
    def run(self):
        global count
        lock.acquire()#加锁
        temp=count+1
        time.sleep(0.001)
        count=temp
        lock.release()#释放锁
lock=threading.Lock()
threads=[]
for _ in range(1000):
    thread=MyThread()
    thread.start()
    threads.append(thread)
for thread in threads:
    thread.join()
print(f'Final count:{count}')
```

这样打印出：1000

Final count:1000



GIL全称为Global Interpreter Lock，翻译为全局解释器锁

在Python多线程下，每个线程的执行方式如下：

获取GIL

执行对应线程的代码

释放GIL

由于GIL的限制，Python的多线程不能发挥多核的优势，最好还是使用多进程

# 7.多路加速，了解多进程的基本原理

## 7.1 多进程的含义

进程（Process）

是具有一定独立功能的程序关于某个数据集合上的一次运行活动

是系统进行资源分配和调度的一个独立单位

多进程就是启用多个进程同时运行

Python多进程的优势

由于进程GIL的存在

Python中的多线程并不能很好地发挥多核优势

一个进程中的多个线程，同一时刻只能有一个线程运行

对于多进程来说

每一个进程都有属于自己的GIL

所以在多核处理器下，多进程的运行是不会受GIL的影响的

多进程能更好地发挥多核的优势

Python的多进程整体来说是比多线程更有优势

在条件允许的情况下，能用多进程就尽量用多进程。

由于进程是系统进行资源分配和调度的一个独立单位，所以各个进程之间的数据是无法共享的

## 多进程的实现

![image-20210716155324930](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210716155324930.png)

直接使用Process类

在multiprocessing中，每个进程都用一个Process类来表示

**API调用**：Process([group[,target[,name[,args[,kwargs]]])

（1）target表示调用对象，你可以传入方法名字

（2）args表示被调用对象的位置参数元组

比如target是函数func，它有两个参数m,n，那么args就传入[m,n]即可

（3）kwargs表示调用对象的字典

（4）name是别名，相当于给这个进程取一个名字

（5）group是分组

直接使用Process类

```
import multiprocessing
def process(index):
    print(f'Process:{index}')
if __name__=='__main__':
    for i in range(5):
        p=multiprocessing.Process(target=process,args=(i,))
        p.start()
```

打印结果：

Process:1
Process:0
Process:2
Process:3
Process:4

注意这里的参数args必须是元组，如果只有一个元素，也要在元素的后面加一个逗号，如果不加逗号就不是元组，就会出问题。

-------我尝试改成[i]序列，也不会出错。

![image-20210716163321277](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210716163321277.png)



multiprocessing还提供了几个比较有用的方法

通过cpu_count方法来获取当前机器CPU的核心数量

通过active_children方法获取当前还在运行的所有进程

```
import multiprocessing
import time
def process(index):
    time.sleep(index)
    print(f'Process:{index}')
if __name__=='__main__':
    for i in range(5):
        p=multiprocessing.Process(target=process,args=[i])
        p.start()
    print(f'CPU number:{multiprocessing.cpu_count()}')
    for p in multiprocessing.active_children():
        print(f'Child process name:{p.name} id:{p.pid}')
    print('Process Ended')
```

打印结果：

CPU number:4
Child process name:Process-2 id:14508
Child process name:Process-5 id:19932
Child process name:Process-4 id:21452
Child process name:Process-1 id:8560
Child process name:Process-3 id:17460
Process Ended
Process:0
Process:1
Process:2
Process:3
Process:4

### 继承Process类

创建进程的方式不止一种

也可以像线程Thread一样通过继承的方式创建一个进程类

进程的基本操作在子类的run方法中实现即可

```
from multiprocessing import Process
import time


class MyProcess(Process):
    def __init__(self, loop):
        Process.__init__(self)
        self.loop = loop

    def run(self):
        for count in range(self.loop):
            time.sleep(1)
            print(f'pid:{self.pid} LoopCount:{count}')


if __name__ == '__main__':
    for i in range(2, 5):
        p = MyProcess(i)
        p.start()
```

打印结果：

pid:20988 LoopCount:0
pid:20008 LoopCount:0
pid:20096 LoopCount:0
pid:20988 LoopCount:1
pid:20096 LoopCount:1
pid:20008 LoopCount:1
pid:20008 LoopCount:2
pid:20096 LoopCount:2
pid:20096 LoopCount:3

## 守护进程

如果一个进程被设置为守护进程

当父进程结束后，子进程会自动被终止

可以通过设置daemon属性来控制是否为守护进程

```
from multiprocessing import Process
import time


class MyProcess(Process):
    def __init__(self, loop):
        Process.__init__(self)
        self.loop = loop

    def run(self):
        for count in range(self.loop):
            time.sleep(1)
            print(f'pid:{self.pid} LoopCount:{count}')


if __name__ == '__main__':
    for i in range(2, 5):
        p = MyProcess(i)
        p.daemon=True
        p.start()
    print('Main process ended')
```

打印结果：

Main process ended

这样就避免了孤立子进程的运行

### 进程等待

让所有子进程都执行完了然后再结束

只需要加入join方法即可

```
if __name__ == '__main__':
    for i in range(2, 5):
        p = MyProcess(i)
        p.daemon=True
        p.start()
        p.join()#加join方法

    print('Main process ended')
```

pid:17752 LoopCount:0
pid:17752 LoopCount:1
pid:17624 LoopCount:0
pid:17624 LoopCount:1
pid:17624 LoopCount:2
pid:15208 LoopCount:0
pid:15208 LoopCount:1
pid:15208 LoopCount:2
pid:15208 LoopCount:3
Main process ended



默认的join是无限期的，如果子进程进入死循环了，主进程也会无限期等待。为了解决这个问题，可以给join加一个参数表示最长等待的时间（秒）

```
if __name__ == '__main__':
    for i in range(2, 5):
        p = MyProcess(i)
        p.daemon=True
        p.start()
        p.join(1) #这里是最长等待1秒
```

pid:19704 LoopCount:0
pid:19476 LoopCount:0
pid:19704 LoopCount:1
Main process ended

### 终止进程

终止进程不止有守护进程这一种做法

通过terminate方法来终止某个子进程

还可以通过is_alive方法判断进程是否还在运行

```
import multiprocessing
import time
def process():
    print('Starting')
    time.sleep(5)
    print('Finished')

if __name__=='__main__':
    p=multiprocessing.Process(target=process)
    print('Before:',p,p.is_alive())

    p.start()
    print('During:',p,p.is_alive())

    p.terminate()
    print('Terminate:',p,p.is_alive())

    p.join()
    print('Joined:',p,p.is_alive())
```

Before: <Process(Process-1, initial)> False
During: <Process(Process-1, started)> True
Terminate: <Process(Process-1, started)> True
Joined: <Process(Process-1, stopped[SIGTERM])> False

可以看到terminate后需要调用join方法才真正终止了。



### 进程互斥锁

![image-20210719094429023](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210719094429023.png)

有的进程没有换行，是由于多个进程并行执行导致的，两个进程同时进行了输出，结果第一个进程的结果还没来得及输出，第二个进程就输出了这结果，所以导致出现了这个现象。

如果能保证多个进程运行期间的任一时间只能一个进程输出，其他进程等待，等刚才那个进程输出完毕之后，另一个进程再进行输出，就不会出现没有换行的现象。

```
from multiprocessing import Process,Lock
import time
class MyProcess(Process):
    def __init__(self,loop,lock):
        Process.__init__(self)
        self.loop=loop
        self.lock=lock

    def run(self):
        for count in range(self.loop):
            time.sleep(0.1)
            #self.lock.acquire()
            print(f'pid:{self.pid} LoopCount:{count}')
            #self.lock.release()
if __name__=='__main__':
    lock=Lock()
    for i in range(10,15):
        p=MyProcess(i,lock)
        p.start()
```

视频中的结果有没换行的输出，我的没有

![image-20210719100002039](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210719100002039.png)

这是我的：

pid:11048 LoopCount:0
pid:11496 LoopCount:0
pid:9268 LoopCount:0
pid:11916 LoopCount:0
pid:11048 LoopCount:1
pid:18020 LoopCount:0
pid:11496 LoopCount:1
pid:9268 LoopCount:1
pid:11916 LoopCount:1
pid:18020 LoopCount:1
pid:11048 LoopCount:2
pid:11496 LoopCount:2
pid:11916 LoopCount:2
pid:9268 LoopCount:2
pid:18020 LoopCount:2
pid:11048 LoopCount:3
pid:11496 LoopCount:3
pid:11916 LoopCount:3
pid:9268 LoopCount:3
pid:11048 LoopCount:4
pid:18020 LoopCount:3
pid:11496 LoopCount:4
pid:11916 LoopCount:4
pid:9268 LoopCount:4
pid:11048 LoopCount:5
pid:18020 LoopCount:4
pid:11496 LoopCount:5
pid:11916 LoopCount:5
pid:9268 LoopCount:5
pid:18020 LoopCount:5
pid:11048 LoopCount:6
pid:11496 LoopCount:6
pid:11916 LoopCount:6
pid:9268 LoopCount:6
pid:11048 LoopCount:7
pid:18020 LoopCount:6
pid:11496 LoopCount:7
pid:9268 LoopCount:7
pid:11916 LoopCount:7
pid:18020 LoopCount:7
pid:11048 LoopCount:8
pid:11496 LoopCount:8
pid:9268 LoopCount:8
pid:11916 LoopCount:8
pid:11048 LoopCount:9
pid:18020 LoopCount:8
pid:11496 LoopCount:9
pid:9268 LoopCount:9
pid:11916 LoopCount:9
pid:18020 LoopCount:9
pid:11496 LoopCount:10
pid:9268 LoopCount:10
pid:11916 LoopCount:10
pid:18020 LoopCount:10
pid:9268 LoopCount:11
pid:11916 LoopCount:11
pid:18020 LoopCount:11
pid:9268 LoopCount:12
pid:18020 LoopCount:12
pid:18020 LoopCount:13



视频中加锁后，全部换行输出了

### 信号量

信号量是进程同步过程中一个比较重要的角色，可以控制临界资源的数据，实现多个进程同时访问共享资源，限制进程的并发量，可以用multiprocessing库中的Semaphore来实现信号量。

```
from multiprocessing import Process,Lock,Semaphore,Queue
import time

buffer=Queue(10)
empty=Semaphore(2)
full=Semaphore(0)
lock=Lock()

class Consumer(Process):
    def run(self):
        global buffer,empty,full,lock
        while True:
            full.acquire()
            lock.acquire()
            buffer.get()
            print('Consumer pop an element')
            #time.sleep(1)
            lock.release()
            empty.release()

class Producer(Process):
    def run(self):
        global buffer,empty,full,lock
        while True:
            empty.acquire()
            lock.acquire()
            buffer.put(1)
            print('Producer append an element')
            # time.sleep(1)
            lock.release()
            full.release()
            break
if __name__=='__main__':
    p=Producer()
    c=Consumer()
    p.daemon=c.daemon=True
    p.start()
    c.start()
    p.join()
    c.join()
    print('Main Process Ended')
```

以上代码运行有问题，可能死锁了，有时间了再看看













未完……



# 8.入门首选，requests库的基本使用

## 8.1 使用urllib

它是Python内置的HTTP请求库，也就是说不需要额外安装就可以使用。它包含以下4个模块：

（1）request：它是最基本的HTTP请求模块，可以用来模拟发送请求。就像在浏览器里输入网址然后回车一样，只需要给库方法传入URL以及额外的参数，就可以模拟实现这个过程了。

（2）error：异常处理模块，如果出现请求错误，我们可以捕获这些异常，然后进行重试或其他操作，以保证程序不会意外终止。

（3）parse：一个工具模块，提供了许多URL处理方法，比如拆分、解析、合并等。

（4）robotparser：主要用来识别网站的robots.txt文件，然后判断哪些网站可以爬，哪些不能爬，它实际用的比较少。

### 8.1.1 发送请求

使用urllib的request模块，我们可以方便地实现请求的发送并得到响应。本节就来看下它的具体用法。

#### 8.1.1.1 urlopen()

urllib.request模块提供了最基本的构造HTTP请求的方法，利用它可以模拟浏览器的一个请求发起过程，同时它还带有授权验证（authentication）、重定向（redirection）、浏览器Cookies以及其他内容。

抓取Python官网为例：

import urllib.request

res = urllib.request.urlopen('http://www.python.org')

print(res.read().decode('utf-8'))

运行结果：

![image-20210726153858892](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210726153858892.png)

用type看下输出的响应是什么类型

![image-20210726154000799](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210726154000799.png)

它是一个HTTPResponse类型的对象，主要包含read()，readinto()，getheader(name)，getheaders()，fileno()等方法，以及msg、version、status、reason、debuglevel、close等属性。

得到这个对象之后，我们把它赋值为res变量，然后就可以调用这些方法和属性，得到返回结果的一系列信息了。

例如，调用read()方法可以得到返回的网页内容，调用status属性可以得到返回结果的状态码，如200代表请求成功，404代表网页未找到。

例如：

import urllib.request

response=urllib.request.urlopen('https://www.python.org')

print(response.status)

print(response.getheaders())

print(response.getheader('Server'))

运行结果如下：

![image-20210726170225950](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210726170225950.png)

如果想给链接传递一些参数，该怎么实现？

首先看一下urlopen()函数的API:

urllib.request.urlopen(url,data=None,[timeout,]*,cafile=None,capath=None,cadefault=False,context=None)

可以发现，除了第一个参数可以传递URL之外，我们还可以传递其他内容，比如data（附加数据）、timeout（超时时间）等。

下面详细说明一下参数的用法：

**（1）data**

data参数是可选的。如果要添加该参数，并且如果它是字节流格式的内容，即bytes类型，则需要通过bytes()方法转化。另外，如果传递了这个参数，则它的请求方式就不再是GET方式了，而是POST方式。

下面用实例来看一下：

```
import urllib.parse
import urllib.request
data=bytes(urllib.parse.urlencode({'word':'hello'}),encoding='utf-8')
response=urllib.request.urlopen('http://httpbin.org/post',data=data)
print(response.read())
```

这里我们传递了一个参数word，值是hello。它需要被转码成bytes(字节流)类型。其中转字节流采用了bytes()方法，该方法的第一个参数需要是str（字符串）类型，需要用urllib.parse模块里的urlencode()方法来讲参数字典转化为字符串；第二个参数是指定编码格式，这里指定为utf-8。

这里请求站点是httpbin.org，它可以提供HTTP请求测试。本次我们请求的URL为http://httpbin.org/post，这个链接可以用来测试post请求，它可以输出请求的一些信息，其中包括传递的data参数。

![image-20210726174117326](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210726174117326.png)

我们传递的参数出现在了form字段中，这表明是模拟了表单提交的方式，以post方式传输数据。

**（2）timeout**

timeout参数用于设置超时时间，单位为秒，意思就是如果请求超出了设置的这个时间，还没有得到响应，就会抛出异常。如果不指定该参数，就会使用全局默认时间。他支持HTTP，HTTPS，FTP请求。

下面是个实例:

```
import socket
import urllib.request
import urllib.error
try:
    response=urllib.request.urlopen('http://httpbin.org/get',timeout=0.1)
except urllib.error.URLError as e:
    if isinstance(e.reason,socket.timeout):
        print('TIME OUT')
```

这里我们请求了http://httpbin.org/get测试链接，设置超时时间是0.1秒，然后捕获了URLError异常，接着判断异常是socket.timeout类型（意思就是超时异常），从而得出它确实是因为超时而报错，打印输出了TIME OUT。

运行结果如下：

TIME OUT

按照常理来说，0.1秒内基本不可能得到服务器相应，因此输出了TIME OUT的提示。

通过设置timeout这个参数来实现超时处理，有时还是很有用的。

**（3）其他参数**

除了data参数和timeout参数外，还有context参数，它必须是ss1.SSLContext类型，用来指定SSL设置。

（[SSL](http://www.vpcv.com/article/wlaq/576.html)英文全称是Secure Sockets Layer，中文含义为安全套接层，SSL用以保障在Internet上数据传输之安全，利用数据加密技术，可确保数据在网络上之传输过程中不会被截取及窃听。简单的说，SSL主要是服务于HTTPS的，而HTTPS是以安全为目标的HTTP通道，简单讲是HTTP的安全版。HTTPS相当于在HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。）

此外，cafile和capath这两个参数分别指定CA证书和它的路径，这个在请求HTTPS链接时会有用。

cadefault参数现在已经弃用了，其默认值为False。

前面讲解了urlopen()方法的用法，通过这个基本的方法，我们可以完成简单的请求和网页抓取。



#### 8.1.1.2 Request

我们知道利用urlopen()方法可以实现最基本请求的发起，但这几个简单的参数并不足以构建一个完整的请求。如果请求中需要加入Headers等信息，就可以利用更强大的Request类构建。

首先，我们用实例来感受一下Request的用法：

```
import urllib.request
request=urllib.request.Request('https://python.org')
response=urllib.request.urlopen(request)
print(response.read().decode('utf-8'))
```

可以发现，我们依然是用urlopen()方法来发送这个请求，只不过这次方法的参数不再是URL，而是一个Request类型的对象。通过构造这个数据结构，一方面我们可以将请求独立成一个对象，另一方面可更加丰富和灵活地配置参数。

下面我们看一下Request可以通过怎样的参数来构造，它的构造方法如下：

class urllib.request.Request(utl,data=None,headers={},origin_req_host=None,unverifiable=False,method=None)

（1）第一个参数url用于请求URL，这是必传参数，其他都是可选参数。

（2）第二个参数data如果要传，必须传bytes（字节流）类型的。如果它是字典，可以先用urllib.parse模块里的urlencode()编码。

（3）第三个参数headers是一个字典，它就是请求头，我们可以在构造请求时通过headers参数直接构造，也可以通过调用请求实例的add_header()方法添加。

添加请求头最常用的方法就是通过修改User-Agent来伪装浏览器，默认的User-Agent是Python-urllib，我们可以通过修改它来伪装浏览器。比如要伪装火狐浏览器，你可以把它设置为：

![image-20210727100239005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210727100239005.png)

（4）第四个参数origin_req_host指的是请求方的host名称或者IP地址。

（5）第五个参数unverifiable表示这请求是否是无法验证的，默认是False，意思是说用户没有足够权限来选择接收这个请求的结果。例如，我们请求一个HTML文档中的图片，但是我们没有自动抓取图像的权限，这时unverifiable的就是True。

（6）第六个参数method是一个字符串，用来指示请求使用的方法，比如GET、POST和PUT等。

下面我们传入多个参数构建请求来看一下：

```
from urllib import request,parse
url='http://httpbin.org/post'
headers={
    'User-Agent':'Mozilla/4.0(compatible;MSIE 5.5;Window NT)',
    'Host':'httpbin.org'
}
dict={
    'name':'Germey'
}
data=bytes(parse.urlencode(dict),encoding='utf-8')
req=request.Request(url=url,data=data,headers=headers,method='POST')
response=request.urlopen(req)
print(response.read().decode('utf-8'))
```

这里通过4个参数构造了一个请求，其中url即请求URL，headers中指定了User-Agent和Host，参数data用urlencode()和byte()方法转成字节流。另外，指定了请求方式为POST。

运行结果：

{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "name": "Germey"
  }, 
  "headers": {
    "Accept-Encoding": "identity", 
    "Content-Length": "11", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "Mozilla/4.0(compatible;MSIE 5.5;Window NT)", 
    "X-Amzn-Trace-Id": "Root=1-60ff6ce3-71e7eb796119ec3d3d439289"
  }, 
  "json": null, 
  "origin": "59.46.230.170", 
  "url": "http://httpbin.org/post"
}

观察结果发现，我们成功设置了data、headers和method。

另外，headers也可以用add_header()方法来添加：

req=request.Request(url=url,data=data,method='POST')

req.add_header('User-Agent','Mozila/4.0(compatible;MSIE 5.5;Windows NT)')

如此一来，我们就可以更加方便地构造请求，实现请求的发送啦。

#### 8.1.1.3 高级用法

在上面的过程中，我们虽然可以构造请求，但是对于一些更高级的操作（比如Cookies处理、代理设置等），我们该怎么办呢？

接下来，就需要更强大的工具Handler登场了。简而言之，我们可以把它理解为各种处理器，有专门处理登录验证的，有处理Cookies的，有处理代理设置的。利用它们，我们几乎可以做到HTTP请求中所有的事情。

首先，介绍一个urllib.request模块里的BaseHandler类，它是所有其他Handler的父类，它提供了最基本的方法，例如default_open()、protocol_request()等。

接下来，就有各种Handler子类继承这个BaseHandler类，举例如下：

（1）HTTPDefaultErrorHandler：用于处理HTTP响应错误，错误都会抛出HTTPError类型的异常。

（2）HTTPRedirectionHandler：用于处理重定向。

（3）HTTPCookieProcessor：用于处理Cookies。

（4）ProxyHandler：用于设置代理，默认代理为空。

（5）HTTPPasswordMgr：用于管理密码，它维护了用户名和密码的表。

（6）HTTPBasicAuthHandler：用于管理认证，如果一个链接打开时需要认证，那么可以用它来解决认证问题。

另外，还有其他的Handler类，可以看官方文档。

关于怎么使用它们，现在先不着急用，后面会有实际演示。

另一个比较重要的类就是OpenerDirector,我们可以称它为Opener。我们之前用过urlopen()这个方法，实际上它就是urllib为我们提供的一个Opener。

那么，为什么要引入Opener呢？因为需要实现更高级的功能。之前抵用的Request和urlopen()相当于类库为你封装好了极其常用的请求方法，利用它们可以完成基本的请求，但是现在不一样了，我们需要实现更高级的功能，所以需要深入一层进行配置，使用更底层的实例来完成操作，所以这里就用到了Opener。

Opener可以使用open()方法，返回的类型和urlopen()如出一辙。那么，它和Handler有什么关系呢？简而言之，就是利用Handler来构建Opener。

下面用几个实例来看看它们的用法。

（1）验证

有些网站在打开的时候就会弹出提示框，直接提示你输入用户名和密码，验证成功后才能查看页面，如图

![image-20210728100354348](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210728100354348.png)

那么，如果要请求这样的页面，该怎么办呢？借助HTTPBasicAuthHandler就可以完成，相关代码如下：

```
from urllib.request import HTTPPasswordMgrWithDefaultRealm,HTTPBasicAuthHandler,build_opener
from urllib.error import URLError
username='username'
password='password'
url='http://localhost:5000/'

p=HTTPPasswordMgrWithDefaultRealm()
p.add_password(None,url,username,password)
auth_handler=HTTPBasicAuthHandler(p)
opener=build_opener(auth_handler)

try:
    result=opener.open(url)
    html=result.read().decode('utf-8')
    print(html)
except URLError as e:
    print(e.reason)
```

以上测试需要自己搭建网站，以后再做……

这里首先实例化HTTPBasicAuthHandler对象，其参数是HTTPPasswordMgrWithDefaultRealm对象，他利用add_password()添加进去用户名和密码，这样就建立了一个处理验证的Handler。

（2）代理

在做爬虫的时候，免不了要使用代理，如果要添加代理，可以这样做：

```
from urllib.error import URLError
from urllib.request import ProxyHandler,build_opener

proxy_handler=ProxyHandler({
    'http':'http://127.0.0.1:9743',
    'https':'https://127.0.0.1:9743'
})
opener=build_opener(proxy_handler)
try:
    response=opener.open('https://www.baidu.com')
    print(response.read().decode('utf-8'))
except URLError as e:
    print(e.reason)
```

这里我们搭建了一个代理，它运行在9743端口上。

这里使用了ProxyHandler，其参数是一个字典，键名是协议类型（比如HTTP或者HTTPS等），键值是代理链接，可以添加多个代理。

然后，利用这个Handler及build_opener()方法构造一个Opener，之后发送请求即可。

（3）Cookies

Cookies的处理就需要相关的Handler了。

我们先用实例来看看怎样将网站的Cookies获取下来，相关代码如下：

```
import http.cookiejar,urllib.request
cookie=http.cookiejar.CookieJar()
handler=urllib.request.HTTPCookieProcessor(cookie)
opener=urllib.request.build_opener(handler)
response=opener.open('http://www.baidu.com')
for item in cookie:
    print(item.name+"="+item.value)
```

首先，我们必须声明一个Cookiejar对象。接下来，就需要利用HTTPCookieProcessor来构建一个Handler，最后利用build_opener()方法构建出Opener，执行open()函数即可。

运行结果如下：

BAIDUID=1E04814D5E6A6B7481E5E405F738EAFE:FG=1
BIDUPSID=1E04814D5E6A6B7472D3E9F5AF9123D0
H_PS_PSSID=34301_33764_34335_31253_34329_34004_34280_34194_26350_34121
PSTM=1627440411
BDSVRTM=0
BD_HOME=1

可以看到，这里输出了每条Cookie的名称和值。

不过既然能输出，那可不可以输出成文件格式呢？我们知道Cookies实际上也是以文本形式保存的。

答案当然是肯定的，这里通过下面的实例来看看：

```
import http.cookiejar,urllib.request
filename='cookie.txt'
cookie=http.cookiejar.MozillaCookieJar(filename)
handler=urllib.request.HTTPCookieProcessor(cookie)
opener=urllib.request.build_opener(handler)
response=opener.open('http://www.baidu.com')
cookie.save(ignore_discard=True,ignore_expires=True)
```

这时CookieJar就需要换成MozillaCookieJar，它在生成文件时会用到，是CookieJar的子类，可以用来处理Cookies 和文件相关的事件，比如读取和保存Cookies，可以将Cookies保存成Mozilla型浏览器的Cookies格式。

运行之后，可以发现生成了一个cookies.txt文件，其内容如下：

```
# Netscape HTTP Cookie File
# http://curl.haxx.se/rfc/cookie_spec.html
# This is a generated file!  Do not edit.

.baidu.com TRUE   /  FALSE  1658978855 BAIDUID    69CFD6D38D9B27556004B75D010001B6:FG=1
.baidu.com TRUE   /  FALSE  3774926502 BIDUPSID   69CFD6D38D9B27554DBF6A42ECB97A96
.baidu.com TRUE   /  FALSE     H_PS_PSSID 34303_34099_34334_34277_33848_34072_34092_34284_26350_34238
.baidu.com TRUE   /  FALSE  3774926502 PSTM   1627442856
www.baidu.com  FALSE  /  FALSE     BDSVRTM    0
www.baidu.com  FALSE  /  FALSE     BD_HOME    1
```

另外，LWPCookieJar同样可以读取和保存Cookies，但是保存的格式和MozillaCookieJar不一样，它会保存成libwww-perl(LWP)格式的Cookies文件。

![image-20210728113735394](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210728113735394.png)

那么，生成了Cookies文件之后，怎样从文件中读取并利用呢？

下面我们以LWPCookiejar格式为例来看一下：

![image-20210728113901705](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210728113901705.png)

这里调用load()方法来读取本地的Cookies文件，获取到了Cookies的内容。不过前提示我们首先生成了LWPCookieJar格式的Cookies，并保存成文件，然后读取Cookies之后使用同样的方法构建Handler和Opener即可。

### 8.1.2 处理异常

前一节我们了解了请求的发送过程，但是在网络不好的情况下，如果出现了异常，该怎么办？

这时如果不处理这些异常，程序很可能因为报错而终止运行，所以异常处理还是十分有必要的。

urllib的error模块定义了由request模块产生的异常。如果出现了问题，request模块便会抛出error模块中定义的异常。

#### 8.1.2.1 URLError

URLError类来自urllib库的error模块，它继承自OSError类，是error异常模块的基类，由request模块产生的异常都可以通过捕获这个类来处理。

