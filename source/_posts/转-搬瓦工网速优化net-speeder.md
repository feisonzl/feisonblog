---
title: '[转]搬瓦工网速优化net-speeder'
tags: []
date: 2016-07-08 11:04:53
---

### 下载并解压源文件：

    :~# wget https://github.com/snooda/net-speeder/archive/master.zip
    :~# unzip master.zip`

    ### 安装编译环境：

    apt-get install libnet1-dev
    :~# apt-get install libpcap0.8-dev `

    ### 编译：

    `Linux Cooked interface使用编译（venetX，OpenVZ）：
    :~# sh build.sh -DCOOKED

    普通网卡使用编译（Xen，KVM，物理机）：
    :~# sh build.sh`

    ### 使用：

    `需要root权限启动
    #参数：./net_speeder 网卡名 加速规则（bpf规则）
    #ovz用法(加速所有ip协议数据)： 
    :~# ./net_speeder venet0 "ip"`

    ### 添加到开机启动项：

    `:~# cp ./net_speeder /usr/bin
    :~# echo -e 'nohup /usr/bin/net_speeder venet0 "ip" &gt;/dev/null 2&gt;&amp;1 &amp;\nexit 0' &gt;&gt; /etc/rc.local

### 参考文章：

[https://github.com/snooda/net-speeder](https://github.com/snooda/net-speeder) 

[http://www.jianshu.com/p/f136b30ca3ba](http://www.jianshu.com/p/f136b30ca3ba)

            
                作者：WEINILUO 发表于2016/7/8 11:04:53 [原文链接](http://blog.csdn.net/weiniluo/article/details/51859247)
            
            
           阅读：165 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51859247#comments)
            
