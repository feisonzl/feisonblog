---
title: '[原]ubuntu下添加用户'
tags: []
date: 2016-02-25 17:30:18
---

    1.$sudo useradd -m -s /bin/bash [username]  //创建登入目录和指定用户的shell
    2.$sudo passwd [username]   //创建登入密码
    3.$sudo uermod -g [groupname] [username] //将username加入groupname组
    例：$sudo usermod -a -G admin username 
    4.$sudo visudo  /etc/sudoers  //为username添加sudo权限（不安全方式）
    root   ALL=&lt;ALL:ALL&gt; ALL
    username  ALL=&lt;ALL:ALL&gt; ALL
    :wq!
    5.$sudo vi /etc/group     //为username添加sudo权限（安全方式）
    sudo:x:27:username1,username2,username3
    :wq

            
                作者：WEINILUO 发表于2016/2/25 17:30:18 [原文链接](http://blog.csdn.net/weiniluo/article/details/50739367)
            
            
            阅读：85 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/50739367#comments)
            
