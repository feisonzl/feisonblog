---
title: '[原]I2C协议'
tags: []
date: 2016-09-18 09:49:16
---

### 1.协议

    1.空闲状态：
        SCL=H&amp;&amp;SDA=H
    2.起始位和停止位:
        start bit:
            SCL=H&amp;&amp;SDA↓
        stop bit:
            SCL=H&amp;&amp;SDA↑
    3.ACK
        发送器每发送完一个字节，就在第9个CLK释放数据线，由接收器反馈一个应答信号。 应答信号为低电平时，规定为有效应答位（ACK简称应答位），表示接收器已经成功地接收了该字节；应答信号为高电平时，规定为非应答位（NACK），一般表示接收器接收该字节没有成功。

    4.数据有效性
        I2C总线进行数据传送时，时钟信号为高电平期间，数据线上的数据必须保持稳定，只有在时钟线上的信号为低电平期间，数据线上的高电平或低电平状态才允许变化。 

    5.主从设备
        系统中的所有外围器件都具有一个7位的"从器件专用地址码"，其中高4位为器件类型，由生产厂家制定，低3位为器件引脚定义地址，由使用者定义。所以同一厂家的同一器件最多只能在同一个I2C总线上挂在8片。
    `</pre>

    ### 2.读写流程

    #### 2.1写流程

    <pre class="prettyprint">`1.start bit
    2.send slave address+write bit
    3.check ack
    4.send reg address
    5.check ack
    6.send data
    7.check ack
    8.stop bit`</pre>

    #### 2.2读流程

    <pre class="prettyprint">`1.start bit
    2.send slave address+write bit
    3.check ack
    4.send reg address
    5.check ack
    6.start bit
    7.send slave address+read bit
    8.check ack
    9.read data
    10.stop bit

            <div>
                作者：WEINILUO 发表于2016/9/18 9:49:16 [原文链接](http://blog.csdn.net/weiniluo/article/details/51790017)
            </div>
            <div>
            阅读：86 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51790017#comments)
            </div>
