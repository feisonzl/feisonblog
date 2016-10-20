---
title: '[原]ubuntu 定时任务'
tags: []
date: 2016-08-24 13:58:32
---

1.编辑/etc/crontab文件

    # /etc/crontab: system-wide crontab
    # Unlike any other crontab you don't have to run the `crontab'
    # command to install the new version when you edit this file
    # and files in /etc/cron.d. These files also have username fields,
    # that none of the other crontabs do.

    SHELL=/bin/sh
    PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

    # m h dom mon dow user  command
    17 *    * * *   root    cd / &amp;&amp; run-parts --report /etc/cron.hourly
    25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.daily )
    47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.weekly )
    52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / &amp;&amp; run-parts --report /etc/cron.monthly )
    #
    10 6    * * *   zhanglei run-parts /home/zhanglei/zlwork/script/

    注：
    1.注释
        m h dom mon dow user  command
        m 0-59的数值，*表示任何数值都执行
        h 0-23的数值，*表示任何数值都执行
        dom 1-31的数值，*表示任何数值都执行
        mon 1-12的数值，*表示任何数值都执行
        dow 0-7的数值，*表示任何数值都执行,0/7代表周日
        user 表示以某个用户身份执行命令
        command 需要执行的脚本或命令（直接为命令或脚本文件或者run-parts 脚本文件目录）

    2.或者用crontab -e 为当前用户创建cron任务   
    3.脚本文件注意PATH引用
        由于cron是系统进程，脚本的执行经常会受到环境变量的影响，因此需要格外小心，例如我的环境变量：
        export PATH=~/bin:$PATH:/sbin

2.service cron restart

参考文章： 

[http://blog.csdn.net/liu414226580/article/details/16339935](http://blog.csdn.net/liu414226580/article/details/16339935) 

[http://blog.csdn.net/wide288/article/details/8765951](http://blog.csdn.net/wide288/article/details/8765951)

            <div>
                作者：WEINILUO 发表于2016/8/24 13:58:32 [原文链接](http://blog.csdn.net/weiniluo/article/details/52299236)
            </div>
            <div>
            阅读：90 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52299236#comments)
            </div>
