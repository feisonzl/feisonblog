---
title: '[转]MIPI 小结'
tags: []
date: 2016-04-25 16:03:25
---

# 1.MIPI联盟

MIPI (Mobile Industry Processor Interface) 是2003年由ARM, Nokia, ST ,TI等公司成立的一个联盟，目的是把手机内部的接口如摄像头、显示屏接口、射频/基带接口等标准化。MIPI联盟下面有不同的WorkGroup，分别定义了一系列的手机内部接口标准，比如摄像头接口CSI、显示接口DSI、射频接口DigRF、麦克风 /喇叭接口SLIMbus等。

# 2.MIPI camera WorkGroup

## 2.1 CSI

    摄像头串行接口
    `</pre>

    # 3.MIPI display WorkGroup

    ## 3.1 DCS

    <pre>`显示接口命令集
    `</pre>

    ## 3.2 DBI

    <pre>`MCU并行接口,lcd的ic包含GRAM的情况下，使用该接口
    `</pre>

    ## 3.3 DPI

    <pre>`RGB并行接口，lcd的ic没有GRAM的情况下，使用该接口
    `</pre>

    ## 3.4 DSI

    ### 3.4.1 DSI分层结构

    <pre>`DSI分四层，对应DCS,DSI,D-PHY规范。
    `</pre>

    ### 3.4.2 command和video模式

    <pre>`command模式类似MCU接口的串行模式，适用于有GRAM的LCD。
    video模式类似RGB接口的串行模式，适用于无GRAM的LCD。
    `</pre>

    ### 3.4.3 D-PHY介绍

    <pre>`1.传输模式
    LP(LOW POWER)低功耗模式，用于控制。
    HS(HIGH SPEED)高速模式，用于图像数据传输。
    2.操作模式
    Escape mode
    High-Speed(Burst) mode
    Control mode    
    `</pre>

    ### 3.4.4DSI介绍

    <pre>`1.传输模式
    高速信号传输模式
    低功耗信号传输模式，只使用lane0
    2.操作模式
    &lt;1&gt;command mode：
    &lt;2&gt;video mosde(需使用高速传输模式)：
        Non-Burst 同步脉冲模式
        Non-Burst 同步事件模式
        Burst模式

            <div>
                作者：WEINILUO 发表于2016/4/25 16:03:25 [原文链接](http://blog.csdn.net/weiniluo/article/details/51242526)
            </div>
            <div>
            阅读：113 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51242526#comments)
            </div>
