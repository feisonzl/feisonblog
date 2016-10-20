---
title: '[原]linux字符设备驱动初识'
tags: []
date: 2016-05-16 21:09:43
---

# 1.设备号申请

    字符设备号申请和注销函数
    register_chrdev_region/alloc_chrdev_region
    unregister_chrdev_region`</pre>

    # 2.字符设备注册

    <pre class="prettyprint">`字符设备添加函数
    cdev_init/cdev_add/cdev_del
    `</pre>

    # 3.创建/dev设备文件

    <pre class="prettyprint">`1.创建class结构，以便device_create使用，同时在/sys/class/添加设备目录
    class_create/class_destroy
    2.在/dev/下添加设备目录,相当于mknod命令
    #if LINUX_VERSION_CODE &gt;= KERNEL_VERSION(2,6,26) 
        #define CLASS_DEV_CREATE(class, devt, device, name) \ 
                device_create(class, device, devt, name) 
    #else
        #define CLASS_DEV_CREATE(class, devt, device, name) \ 
                class_device_create(class, device, devt, name) 
    #endif

    #if LINUX_VERSION_CODE &gt;= KERNEL_VERSION(2,6,26) 
        #define CLASS_DEV_DESTROY(class, devt) \ 
                device_destroy(class, devt) 
    #else
        #define CLASS_DEV_DESTROY(class, devt) \ 
                class_device_destroy(class, devt)
    #endif`</pre>

    # 4.创建设备属性

    <pre class="prettyprint">`在device_create创建的目录下创建设备属性，作为属性和函数的对应关系
    device_create_file
    `</pre>

    # 5.创建/proc设备文件

    <pre class="prettyprint">`在/proc/目录下创建设备文件
    create_proc_entry
    `</pre>

    # 6.平台驱动注册

    <pre class="prettyprint">`platform_driver_register`</pre>

    # 7.平台设备文件注册

    <pre class="prettyprint">`在/sys/platform/下注册设备文件
    platform_device_register

            <div>
                作者：WEINILUO 发表于2016/5/16 21:09:43 [原文链接](http://blog.csdn.net/weiniluo/article/details/51261676)
            </div>
            <div>
            阅读：141 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51261676#comments)
            </div>
