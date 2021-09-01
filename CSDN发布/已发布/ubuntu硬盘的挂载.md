# Ubuntu-20.04.1磁盘的挂载

# 1.df -h查看磁盘的信息

![image-20210709170530141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709170530141.png)

如果不想这样挂载了，需要先解开挂载点

```
sudo umount /dev/sda5
```

# 2.查看磁盘的挂载详情lsblk

## ![image-20210709171729232](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709171729232.png)

可以到红框这种后面没有挂载点的就是没有挂载的磁盘。

# 3.确认好需要挂载的地方

比如/home/XXX/下建一个local文件夹，并将local这个文件夹作为挂载点

# 4.拿到盘符的信息

sudo blkid /dev/sda5  

UUID和TYPE这2个很重要

![image-20210709172043130](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709172043130.png)

# 5.修改/etc/fstab配置

```
sudo vim /etc/fstab
```

在最后一行添加UUID=XXX  挂载的路径 TYPE defaults 0 2（一定不要 01,因为01是启动时候用的 ）

![image-20210709172212219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709172212219.png)

# 6.进行挂载

```
sudo mount -a
```

注意：这一步可能出错如下

The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
Could not mount read-write, trying read-only

修复方法：

```
sudo apt-get install ntfs-3g
sudo ntfsfix /dev/sda4
```

# 7.修改权限 

chmod 777 路径

# 8.重启电脑

没有重启时我遇到这个错误

mkdir: 无法创建目录 “mysql”: 只读文件系统

我什么也没改，重启了一下电脑，就好用了。
