---
title: '[原]windows下的adb连接调试'
tags: []
date: 2016-02-26 10:18:38
---

1.若输入adb shell出现：

    adb server is out of date.  killing...
    * daemon started successfully *
    error: device offline`</pre>

    可能是adb版本较低，请换用较新的adb 

    2.若输入adb shell出现：

    <pre class="prettyprint">`error: device unauthorized. Please check the confirmation dialog on your device.

因为手机端没有允许adb调试，需要在手机端确认

            <div>
                作者：WEINILUO 发表于2016/2/26 10:18:38 [原文链接](http://blog.csdn.net/weiniluo/article/details/50747737)
            </div>
            <div>
            阅读：106 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/50747737#comments)
            </div>
