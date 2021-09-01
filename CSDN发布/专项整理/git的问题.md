# 1.git push出错：403

$ git push remote: Permission to liujing-scu/MyFirst.git denied to nanabice. fat

unable to access 'https://github.com/liujing-scu/MyFirst.git/': The requested URL returned error: 403

解决办法：

![image-20210531113421994](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531113421994.png)

![image-20210531113436145](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210531113436145.png)

修改成自己的账号密码。

# 2.git push错误：10054

$ git push
fatal: unable to access 'https://github.com/liujing-scu/MyFirst.git/': OpenSSL SSL_read: Connection was reset, errno 10054

解决办法：

$ git config --global http.sslVerify "false"
