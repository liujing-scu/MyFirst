chrome的版本和Selenium支持的版本不一致，报错如下：

selenium.common.exceptions.SessionNotCreatedException: Message: session not created: This version of ChromeDriver only supports Chrome version 86
Current browser version is 92.0.4515.131 with binary path C:\Users\Administrator\AppData\Local\Google\Chrome\Application\chrome.exe

解决方法：

# 1.查询Chrome的版本

报错里面其实也有说明版本是92.0.4515.131。如果报错没有说明Chrome版本，可以在Chrome浏览器地址栏输入chrome://version/

![image-20210827103737536](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210827103737536.png)

# 2.下载对应的Chrome版本driver

http://chromedriver.storage.googleapis.com/index.html

![image-20210827103847640](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210827103847640.png)

我是在windows环境下的，选择对应的下载资源。

![image-20210827103914457](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210827103914457.png)

# 3.解压后放入Chrome安装路径下

![image-20210827104234784](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210827104234784.png)

# 4.配置PATH或者使用的时候加上ChromeDriver的路径

## （1）方法一：配置PATH

还没解决

## （2）方法二：使用的时候加上ChromeDriver的路径

```
import selenium
from selenium import webdriver
path =r"C:\Users\Administrator\XXX\Google\Chrome\Application\chromedriver.exe"
brower = webdriver.Chrome(executable_path=path)
brower.get("https://www.baidu.com/")
```

`











