---
title: '[原]驱动中ioctl的使用'
tags: []
date: 2016-09-22 18:01:37
---

### 1 ioctl调用

    用户空间使用：
    int ioctl(int fd,unsigned long cmd,...);
    内核空间声明：
    int (*ioctl)(struct inode *inode,struct file *filp,unsigned int cmd,unsigned long arg);`</pre>

    ### 2 ioctl命令

    <pre class="prettyprint">`内核约定了定义ioctl命令的相应方法，详细介绍见Documentation/ioctl/ioctl-number.txt和include/asm-generic/ioctl.h
    定义ioctl命令的方法使用了4个位字段：type,number,direction,size
    type:幻数字段（8bit）,根据ioctl-number.txt，选择一个号码在整个驱动中使用
    number:序号字段（8bit），表示第几个命令号
    direction:传输方向字段（2bit），表示数据传输方向
    size:数据大小字段（14bit），表示数据传输的大小
    命令构造方法：_IOC(direction,type,number,size)
    常用构造命令的宏：
    #define _IO(type,nr)        _IOC(_IOC_NONE,(type),(nr),0)
    #define _IOR(type,nr,size)  _IOC(_IOC_READ,(type),(nr),(_IOC_TYPECHECK(size)))
    #define _IOW(type,nr,size)  _IOC(_IOC_WRITE,(type),(nr),(_IOC_TYPECHECK(size)))
    #define _IOWR(type,nr,size) _IOC(_IOC_READ|_IOC_WRITE,(type),(nr),(_IOC_TYPECHECK(size))
    构造命令宏的使用：
    #define CMD1 _IO(type,1)
    #define CMD2R _IOR(type,2,int)
    #define CMD3W _IOW(type,3,int)
    `</pre>

    ### 3 预定义命令

    <pre class="prettyprint">`预定义命令有三类：
    1.可用于任何文件的命令；
    2.只用于普通文件的命令；
    3.特定于文件系统类型的命令；
    用于任何文件的常用预定义命令：
    1.FIOCLEX:设置执行时关闭标志
    2.FIONCLEX:清除执行时关闭标志
    2.FIOASYNC:设置或复位文件异步通知
    2.FIOQSIZE:返回文件或目录大小
    2.FIONBIO:文件IO非阻塞型IO`</pre>

    ### 4 ioctl参数使用

    <pre class="prettyprint">`这里主要讨论ioctl的最后一个参数，该参数常常是指向用户空间的指针，但使用用户空间的指针必须保证合法，否则可能导致内核崩溃或其他安全问题。（copy_from_user和copy_to_user内部已经实现了相关的安全验证，所以不用关心这个问题）
    因此，我们需要使用access_ok函数验证地址是否安全：
    int access_ok(int type,const void *addr,unsigned long size);
    type:VERIFY_READ/VERIFY_WRITE
    size:参数大小`</pre>

    ### 5 ioctl返回值

    <pre class="prettyprint">`这里只讨论ioctl命令不匹配时的返回值，通常的做法是返回-EINVAL,但根据POSIX标准，应该返回-ENOTTY.

            <div>
                作者：WEINILUO 发表于2016/9/22 18:01:37 [原文链接](http://blog.csdn.net/weiniluo/article/details/52625486)
            </div>
            <div>
            阅读：132 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52625486#comments)
            </div>
