# 切换到root用户：

sudo su - root



# 修改文件名：

mv 老文件名 新文件名

![image-20210518101728041](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210518101728041.png)



# 查看磁盘空间

df -h

![image-20210520104411178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210520104411178.png)

# 查看当前路径

pwd

# vim

光标下一行插入

```
o
```

行号显示

```
：set nu
```

取消行号显示

```
：set nonu
```

光标处可直接替换

```
r
```

跳到文件开始（结尾）处

```
g（G）
```

查找(n找下一个)

```
:/keyword     //向光标下搜索keyword字符串，keyword可以是正则表达式 

:?keyword     //向光标上搜索keyword字符串
```

恢复键

```
u
```

删除行

命令输入“：32,65d”,回车键，32-65行就被删除了



# 批量杀进程

[root@localhost ~]# kill -KILL `ps aux |grep httpd| awk '{print $2}'`

这样可以杀掉所有Apache进程



**[root@localhost ~]# ps aux |grep httpd| awk '{print $2}'** 
**其中awk print $2分离出里面的第二个字段**

- 方法1

```
pkill 进程名 
```

- 方法2

```
killall 进程名
```

- 方法3

```
kill -9 $(ps -ef|grep 进程名关键字|grep -v grep|awk '{print $2}')
```

这个是利用管道和替换将 进程名对应的进程号提出来作为kill的参数。

- 方法4

```
kill -9 $(pidof 进程名关键字)
```

# ps -ef和ps aux的区别

**ps -ef 是用标准的格式显示进程的、其格式如下**

![图片](https://mmbiz.qpic.cn/mmbiz_png/SOCnlGsBWDcyPAVqQgS1ybUg1gqxHHccsHtSXUzZKPtEb9nWWVnVt4IYG80gBFbF2xIiaQ9DjGCl9W1lUIXZ1fA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

其中各列的内容意思如下

- UID //用户ID、但输出的是用户名 
- PID //进程的ID 
- PPID //父进程ID 
- C //进程占用CPU的百分比 
- STIME //进程启动到现在的时间 
- TTY //该进程在那个终端上运行，若与终端无关，则显示? 若为pts/0等，则表示由网络连接主机进程。 
- CMD //命令的名称和参数



**ps aux 是用BSD的格式来显示、其格式如下**

![图片](https://mmbiz.qpic.cn/mmbiz_png/SOCnlGsBWDcyPAVqQgS1ybUg1gqxHHccibIYJ4dwlnM1ricafo8iarksmaWFA5MpPRNe6jeQjpuU8WEtSfp3JPwzg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

同ps -ef 不同的有列有

- USER //用户名 
- %CPU //进程占用的CPU百分比 
- %MEM //占用内存的百分比 
- VSZ //该进程使用的虚拟內存量（KB） 
- RSS //该进程占用的固定內存量（KB）（驻留中页的数量） 
- STAT //进程的状态 
- START //该进程被触发启动时间 
- TIME //该进程实际使用CPU运行的时间

# awk的用法

```
jingliu@devSpider:~$ ps -ef | grep scrapy
jingliu   184842  178157  0 10:50 pts/3    00:00:00 grep --color=auto **scrapy**
```

```
jingliu@devSpider:~$ ps -ef |grep scrapy |awk '{print $2}'
184846
jingliu@devSpider:~$ ps -ef |grep scrapy |awk '{print $1}'
jingliu
jingliu@devSpider:~$ ps -ef |grep scrapy |awk '{print $3}'
178157
```

# grep命令

（1）grep name# 表示只查看name这个内容
（2）grep -v name # 表示查看除了name之外的内容

![image-20210618105647538](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210618105647538.png)

（3）按照文件内容查找

grep -r “查找内容” + 查找的路径（引号建议加上）

```
jingliu@devSpider:~/master/CrawlChatting/CrawlChatting/spiders$ grep -r "hupu" ./ 
Binary file ./__pycache__/hupu_spider.cpython-38.pyc matches
./hupu_spider.py:    name = 'hupu_spider'
./hupu_spider.py:    allowed_domains = ['bbs.hupu.com']
./hupu_spider.py:    start_urls = ['https://bbs.hupu.com']
./hupu_spider.py:    url_head = 'https://bbs.hupu.com'
./hupu_spider.py:                    comment_url = 'https://bbs.hupu.com/api/v2/reply/reply?tid=' + tid + '&pid=' + pid + '&maxpid=0'
./hupu_spider.py:        is_page_more = response.xpath('//ul[@class="hupu-rc-pagination"]')
./hupu_spider.py:            page_num_xpath = response.xpath('//ul[@class="hupu-rc-pagination"]/li')[-2]
./hupu_spider.py:        page_num_xpath = response.xpath('//ul[@class="hupu-rc-pagination"]/li')[-2]
jingliu@devSpider:~/master/CrawlChatting/CrawlChatting/spiders$ 
```



# xargs

`xargs` 可以将一个命令的输出作为参数发送给另一个命令。

例如：如果我们通过 `xargs` 将一串文件名的字符串传递给 `touch` 命令，`touch` 将创建这些文件。

```text
$ echo 'one two three' | xargs -p touch
```

# 按文件名字批量删除文件

```
rm -f scrapy_2021_6_{3..18}_zhihu_spider_{0..32}
```

-rw-r--r-- 1 jingliu dev  377463305 Jun 10 17:40 scrapy_2021_6_7_zhihu_spider_17
-rw-r--r-- 1 jingliu dev  426337788 Jun  9 15:20 scrapy_2021_6_7_zhihu_spider_18
-rw-r--r-- 1 jingliu dev  294135804 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_19
-rw-r--r-- 1 jingliu dev  290526430 Jun  9 15:26 scrapy_2021_6_7_zhihu_spider_2
-rw-r--r-- 1 jingliu dev  279951403 Jun  9 15:24 scrapy_2021_6_7_zhihu_spider_20
-rw-r--r-- 1 jingliu dev  251717995 Jun 10 17:36 scrapy_2021_6_7_zhihu_spider_21
-rw-r--r-- 1 jingliu dev  286346711 Jun  9 15:25 scrapy_2021_6_7_zhihu_spider_22
-rw-r--r-- 1 jingliu dev  344362505 Jun  9 15:37 scrapy_2021_6_7_zhihu_spider_23
-rw-r--r-- 1 jingliu dev  180792627 Jun  9 15:22 scrapy_2021_6_7_zhihu_spider_24
-rw-r--r-- 1 jingliu dev  319043142 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_25
-rw-r--r-- 1 jingliu dev  172737650 Jun  9 15:21 scrapy_2021_6_7_zhihu_spider_26
-rw-r--r-- 1 jingliu dev  234351064 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_27
-rw-r--r-- 1 jingliu dev  183868272 Jun  9 15:22 scrapy_2021_6_7_zhihu_spider_28
-rw-r--r-- 1 jingliu dev  516046521 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_29
-rw-r--r-- 1 jingliu dev  324671689 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_3
-rw-r--r-- 1 jingliu dev  210340052 Jun  9 15:25 scrapy_2021_6_7_zhihu_spider_30
-rw-r--r-- 1 jingliu dev  428509033 Jun 10 17:44 scrapy_2021_6_7_zhihu_spider_31
-rw-r--r-- 1 jingliu dev   96034235 Jun  8 18:20 scrapy_2021_6_7_zhihu_spider_32

# nohup command &

用来启动一些后台程序，比如一些java服务

```
 nohup java -jar xxxx.jar &
```

又比如scrapy任务：

```
 nohup scrapy crawl tianya_spider &
```

# \>/dev/null 2>&1

使用/dev/null 
把/dev/null看作"黑洞". 它非常等价于一个只写文件. 所有写入它的内容都会永远丢失

意思是将标准输出和错误输出都丢弃

经常和上面的命令一起用：

```
nohup command >/dev/null 2>&1 &
```

# shell中的for循环语法

```
for 变量名 in 单词表

do

命令表

done
```

# echo

向屏幕输入一串字符，类似C语言的printf()函数。

# find查找文件

（1）按照文件名查找

find + 查找的目录 + -name +  “文件的名字”（引号需要）

```
jingliu@devSpider:~/master/CrawlChatting$ find ./ -name "zhihu.sh"
./zhihu.sh
./CrawlChatting/zhihu.sh
```

（2）按照文件大小查找

find  + 查找目录 + -size + +10k （+10k就是大于10k，k要小写）

find  + 查找目录 + -size + -10k （-10k就是小于10k，k要小写）

find  + 查找目录 + -size + -10M （-10M就是小于10M，M要大写）

find  + 查找目录 + -size + +10M -size -100M（10~100M之间的）

```
jingliu@devSpider:~/master/CrawlChatting$ find ./ -size +100k -size -200k
./CrawlChatting/spiders/aaa
```

（3）按照文件类型查找

find + 查找目录 + -type + 文件类型

文件类型：

f:普通文件

d:目录

l:链接符号

b:块设备

c:字符设备

s:socket文件

p:管道

```
jingliu@devSpider:~/master/CrawlChatting/CrawlChatting/spiders$ find ./ -type d
./
./__pycache__
```

# top命令查看cpu使用情况

运行top 命令后，CPU 使用状态会以全屏的方式显示，并且会处在对话的模式-- 用基于 top 的命令，可以控制显示方式等等。退出 top 的命令为 q （在top 运行中敲 q 键一次）。

```
top - 17:39:45 up 24 days,  7:56,  6 users,  load average: 1.36, 1.00, 0.94
Tasks: 335 total,   2 running, 333 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.8 us,  0.0 sy,  0.0 ni, 93.7 id,  0.1 wa,  0.0 hi,  0.4 si,  0.0 st
MiB Mem :  15798.7 total,    158.8 free,   2857.2 used,  12782.7 buff/cache
MiB Swap:   2048.0 total,   1205.4 free,    842.6 used.  12611.1 avail Mem 
```



    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                                                                                                                                                          


```
208763 jingliu   20   0  561296  93112  16924 S   7.3   0.6  17:51.47 scrapy                                                                                                                                                           
 208776 jingliu   20   0  559528  90832  17124 S   7.0   0.6  17:03.51 scrapy                                                                                                                                                           
 203814 jingliu   20   0  739064 269924  17228 S   6.0   1.7 140:04.47 scrapy                                                                                                                                                           
 208768 jingliu   20   0  559496  90572  16820 S   6.0   0.6  17:35.23 scrapy                                                                                                                                                           
 208775 jingliu   20   0  560768  91080  16724 S   6.0   0.6  17:25.70 scrapy                                                                                                                                                           
 208773 jingliu   20   0  560780  93428  16888 S   5.7   0.6  17:36.27 scrapy                                                                                                                                                           
 208765 jingliu   20   0  560720  92252  17096 S   5.3   0.6  17:43.43 scrapy                                                                                                                                                           
 208766 jingliu   20   0  560584  92044  17100 R   5.3   0.6  17:39.92 scrapy                                                                                                                                                           
 208769 jingliu   20   0  560588  92632  16964 S   5.3   0.6  17:32.34 scrapy                                                                                                                                                           
 208774 jingliu   20   0  560400  91564  17052 S   5.3   0.6  17:25.83 scrapy                                                                                                                                                           
 208772 jingliu   20   0  560804  91784  16712 S   5.0   0.6  17:37.79 scrapy                                                                                                                                                           
 208778 jingliu   20   0  560808  92044  17040 S   4.7   0.6  17:29.89 scrapy                                                                                                                                                           
 208771 jingliu   20   0  559416  90628  17016 S   4.3   0.6  18:00.61 scrapy                                                                                                                                                           
 208770 jingliu   20   0  560128  91636  16972 S   4.0   0.6  17:33.14 scrapy                                                                                                                                                           
 208777 jingliu   20   0  561052  91676  16732 S   4.0   0.6  17:53.95 scrapy                                                                                                                                                           
 208762 jingliu   20   0  561004  92280  16972 S   3.3   0.6  17:36.86 scrapy                                                                                                                                                           
 208764 jingliu   20   0  558640  90272  16708 S   3.3   0.6  17:46.20 scrapy                                                                                                                                                           
 208767 jingliu   20   0  560028  92108  16616 S   3.3   0.6  17:44.44 scrapy                                                                                                                                                           
 197850 mysql     20   0 9156952 605972  35096 S   2.0   3.7  57:59.04 mysqld                                                                                                                                                           
 198393 jingliu   20   0 1176784 191064  16096 S   0.7   1.2 197:42.70 python                                                                                                                                                           
     11 root      20   0       0      0      0 I   0.3   0.0  41:01.16 rcu_sched                                                                                                                                                        
 195522 jingliu   20   0   16368   5740   4020 S   0.3   0.0   2:54.90 sshd                                                                                                                                                             
 203467 jingliu   20   0  408160  26632   9620 S   0.3   0.2   3:12.62 python                                                                                                                                                           
      1 root      20   0  167916   7084   4448 S   0.0   0.0   0:27.46 systemd   
```

# 对大文件的处理

sed：

删除第N~M行
sed -i 'N,Md' filename # file的[N,M]**行都被删除**

# 查找大文件的方法

**du -h / --max-depth=1 | sort -hr | head -n 10**

du的–max-depth=1表示只展示第一个层级的目录和文件，sort的-h选项和du的-h选项一样，-r表示倒叙，默认升序。

# 两台linux互传文件

从本机往另一台linux传文件

```
scp 本机文件路径 远程主机用户@远程主机ip:远程文件存放路径
```

回车输入远程主机用户的密码

举例：

注意文件夹目录需要加-r

```
scp /home/jingliu/a.txt yida-pcg@192.168.75.61:/home/yida-pcg
```

# 修改文件权限

chown 所有者：所属组 文件名

```
chown jingliu:dev ddddd.xlsx
```

# 查看文件夹内文件大小总和

du -sh 文件目录

# 硬盘挂载

如果挂载了需要解开挂载点：sudo umount /dev/sda5
开始挂载：（1）拿到盘符的信息：sudo blkid /dev/sda5  UUID和TYPE这2个很重要（2）输入 sudo gedit /etc/fstab
(3)UUID=XXX  挂载的路径 TYPE defaults 0 2（一定不要 01,01是启动时候用的 ）（4）最后进行挂载：sudo mount -a
（5）修改权限 chmod 777 路径

