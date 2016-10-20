---
title: '[原]SSH端口转发与内网穿透'
tags: []
date: 2016-08-03 13:54:33
---

1.内网系统配置：

    localhost$sudo ssh -R 0.0.0.0:port_vps:127.0.0.1:port_local username@vps_ip -p vps_ssh_port
    port_vps:vps端的转发端口
    port_local：本地ssh登录端口
    vps_ssh_port：vps的ssh登录端口
    `</pre>

    2.vps端配置

    <pre class="prettyprint">`vps_host$vi /etc/ssh/ssh_config
    +  GatewayPorts yes
    vps_host$service ssh restart

3.帮助 

[https://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/](https://www.ibm.com/developerworks/cn/linux/l-cn-sshforward/)

            <div>
                作者：WEINILUO 发表于2016/8/3 13:54:33 [原文链接](http://blog.csdn.net/weiniluo/article/details/51946721)
            </div>
            <div>
            阅读：108 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51946721#comments)
            </div>
