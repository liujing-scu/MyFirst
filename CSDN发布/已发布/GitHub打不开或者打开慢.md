问题：GitHub（https://github.com/）经常打开失败，或者非常慢，传文件的时候返回Connection was reset, errno 10054。原因是网络不稳定，为什么会网络不稳定呢？根本原因是域名解析问题，每次解析出来的IP不是完全一样的。当不好用的时候我们需要去查询一下此时此刻的域名解析出来的IP地址，然后修改HOSTS配置。

# 1.查询github域名解析出来的ip

打开地址http://tool.chinaz.com/dns，输入github.com，然后点检测。

结果显示下面有几个ip，我选择了一个国内的TTL值小的ip加到hosts里。

![image-20210826111652020](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210826111652020.png)

## 2.添加ip到hosts文件

由于权限问题，不能直接在hosts所在的位置（C:\Windows\System32\drivers\etc）直接进行修改。需要先将这个hosts复制到其他位置（比如桌面），然后在最后一行加上这个ip，保存，关闭。最后把这个文件覆盖原来的hosts文件即可。

