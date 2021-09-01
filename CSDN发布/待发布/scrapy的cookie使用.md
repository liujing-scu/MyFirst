有的网站是需要登录才能看见具体的内容，这时候我们爬虫就需要涉及到cookie了。

# 1.获取cookie

以http://my.4399.com/forums/为例

![image-20210618172247926](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210618172247926.png)

（1）登录

（2）打开“检查”工具

（3）刷新抓包

（4）获取cookies

# 2.scrapy框架中使用cookie

## 2.1 COOKIES_ENABLED的设置

（1）可以在settings.py中的设置

```
# COOKIES_ENABLED = True
```

这一句注释掉也不行，需要设置为False，

```
COOKIES_ENABLED = False
```

（2）在settings.py中修改会影响全局的爬虫，如果不想这样，可以在各个爬虫下面进行个性化设置，方法如下：

XXX_spider.py:

```
custom_settings = {
    'COOKIES_ENABLED': False
}
```

## 2.2 重写start_requests函数

因为默认的start_requests是不带cookie的。这里需要加上headers，headers中要加上cookie

```
headers = {
    "user-agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36",
    "cookie": "_4399tongji_vid=162329613976912; _4399stats_vid=16232961398313293; _gprp_c=""; _4399tongji_st=1623381696; USESSIONID=6337adae-f7e8-4fca-ace4-2c639bf66e10; Hm_lvt_5c9e5e1fa99c3821422bf61e662d4ea5=1623296139,1623381697; Hm_lvt_e5a07b5994f78634294b9c347a5be7d2=1623296139,1623381697; Uauth=ext|%E5%88%98%E9%9D%99|2021611|my.|1623381715919|c081934f07170fae5b44cabad34e1aa1; Pauth=3499832115|2317532513|t3ce7m00004ad524aa9ce2be034f5ebd|1623381715|10002|bbb836cbc57c860236d527be05d113c7|0; ck_accname=2317532513; Puser=2317532513; Xauth=7d3b50813dc9839e228be22bc572ccda; ptusertype=my.weixin_login; Qnick=%E5%88%98%E9%9D%99; zone_guide_date=1623427200; smidV2=20210611112158b565fbc4812741a469f380d607d1582600dcf6872965faaa0; zone_guide_limit=-1; zone_guide_time=0; zone_guide=-1; Hm_lpvt_e5a07b5994f78634294b9c347a5be7d2=1623382010; Hm_lpvt_5c9e5e1fa99c3821422bf61e662d4ea5=1623382010; Pnickset=1; ol=1; Pmtime=079146384397bc5a9e74%7C1623392619"
}
```

```
def start_requests(self):
	for url in start_urls:
        yield scrapy.Request(url, callback=self.parse, headers=self.headers)
```

