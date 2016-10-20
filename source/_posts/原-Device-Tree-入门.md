---
title: '[原]Device Tree 入门'
tags: []
date: 2016-08-26 14:15:06
---

1.写在前面 

本文是本人在阅读了蜗窝科技的Device Tree三篇文章后的笔记，他的这几篇文章是我最近了解到的在Device Tree方面讲的比较深入细致的，非常感谢蜗窝科技为我们提供的学习机会，如果有需要，请你们去[蜗窝科技](http://www.wowotech.net/device_model/why-dt.html)。 

2.Device Tree结构

    kernel/arch/arm/boot/dts/skeleton.dtsi:
    / {
        #address-cells = &lt;1&gt;;
        #size-cells = &lt;1&gt;;
        cpus { };
        soc { };
        chosen { };
        aliases { };
        memory { device_type = "memory"; reg = &lt;0 0&gt;; };
    };
    kernel/arch/arm/boot/dts/qcom/skeleton.dtsi:
    #include "skeleton64.dtsi"
    #include &lt;dt-bindings/clock/msm-clocks-8953.h&gt;
    #include &lt;dt-bindings/regulator/qcom,rpm-smd-regulator.h&gt;
    #include &lt;dt-bindings/gpio/gpio.h&gt;
    / {
        model = "Qualcomm Technologies, Inc. MSM 8953";
        compatible = "qcom,msm8953";
        qcom,msm-id = &lt;293 0x0&gt;;
        interrupt-parent = &lt;&amp;intc&gt;;

        chosen {
            bootargs = "sched_enable_hmp=1 sched_enable_power_aware=1";
        };

        reserved-memory {
            #address-cells = &lt;2&gt;;
            #size-cells = &lt;2&gt;;
            ranges;

            other_ext_mem: other_ext_region@0 {
                compatible = "removed-dma-pool";
                no-map;
                reg = &lt;0x0 0x83300000 0x0 0x3800000&gt;;
            };

            modem_mem: modem_region@0 {
                compatible = "removed-dma-pool";
                no-map-fixup;
                reg = &lt;0x0 0x86c00000 0x0 0x5600000&gt;;
            };

            reloc_mem: reloc_region@0 {
                compatible = "removed-dma-pool";
                no-map;
                reg = &lt;0x0 0x8c200000 0x0 0x1800000&gt;;
            };

            venus_mem: venus_region@0 {
                compatible = "shared-dma-pool";
                reusable;
                alloc-ranges = &lt;0x0 0x80000000 0x0 0x10000000&gt;;
                alignment = &lt;0 0x400000&gt;;
                size = &lt;0 0x0800000&gt;;
            };

            secure_mem: secure_region@0 {
                compatible = "shared-dma-pool";
                reusable;
                alignment = &lt;0 0x400000&gt;;
                size = &lt;0 0x09800000&gt;;
            };

            qseecom_mem: qseecom_region@0 {
                compatible = "shared-dma-pool";
                reusable;
                alignment = &lt;0 0x400000&gt;;
                size = &lt;0 0x1000000&gt;;
            };

            adsp_mem: adsp_region@0 {
                compatible = "shared-dma-pool";
                reusable;
                size = &lt;0 0x400000&gt;;
            };

            dfps_data_mem: dfps_data_mem@90000000 {
                   reg = &lt;0 0x90000000 0 0x1000&gt;;
                   label = "dfps_data_mem";
            };

            cont_splash_mem: splash_region@0x90001000 {
                reg = &lt;0x0 0x90001000 0x0 0x13ff000&gt;;
                label = "cont_splash_mem";
            };

            gpu_mem: gpu_region@0 {
                compatible = "shared-dma-pool";
                reusable;
                alloc-ranges = &lt;0x0 0x80000000 0x0 0x10000000&gt;;
                alignment = &lt;0 0x400000&gt;;
                size = &lt;0 0x800000&gt;;
            };
        };

        aliases {
            /* smdtty devices */
            smd1 = &amp;smdtty_apps_fm;
            smd2 = &amp;smdtty_apps_riva_bt_acl;
            smd3 = &amp;smdtty_apps_riva_bt_cmd;
            smd4 = &amp;smdtty_mbalbridge;
            smd5 = &amp;smdtty_apps_riva_ant_cmd;
            smd6 = &amp;smdtty_apps_riva_ant_data;
            smd7 = &amp;smdtty_data1;
            smd8 = &amp;smdtty_data4;
            smd11 = &amp;smdtty_data11;
            smd21 = &amp;smdtty_data21;
            smd36 = &amp;smdtty_loopback;
            sdhc1 = &amp;sdhc_1; /* SDC1 eMMC slot */
            sdhc2 = &amp;sdhc_2; /* SDC2 for SD card */
            i2c1 = &amp;i2c_1;
            i2c2 = &amp;i2c_2;
            i2c3 = &amp;i2c_3;
            i2c5 = &amp;i2c_5;
            spi3 = &amp;spi_3;
        };

        soc: soc { };

    };
    由上面的例子看出Device Tree有一下特点：
    1.节点node
        1.1每个node可以包含sub node和property/value；
        1.2每个DTS都只有一个ROOT node；
        1.3一个node可以有多个sub node，但只能有一个parent node；
        1.4一个node可以包含多个property/value描述该node的具体信息；
        1.5每个node用节点名字（node name）标识，节点名字的格式是node-name@unit-address。如果该node没有reg属性（后面会描述这个property），那么该节点名字中必须不能包括@和unit-address。
    2.属性/值property/value
        2.1一般属性：
        属性值表示了该设备节点的具体特性，它的值是多样性的：
        1)可能为空，即没有值的定义；
        2)可能为32bit或64bit整数值(#size-cells = &lt;2&gt;)或者数组(reg = &lt;0x0 0x86c00000 0x0 0x5600000&gt;)；
        3）可能为string(label = "cont_splash_mem")或string list(compatible = "qcom,msm8953")；
        2.2特殊属性：
        #address-cells = &lt;1&gt;;
        #size-cells = &lt;0&gt;;
        #类似number，#address-cells表示子节点中reg中的地址元素需要用多少个32bit值来表示，#size-cells表示子节点中reg中的大小元素需要用多少个32bit值来表示。`</pre>

    3.Device Tree source file语法

    <pre class="prettyprint">`1.节点定义：
        [label:] node-name[@unit-address] { 
           [properties definitions];
           [child nodes];
        };
    2.节点引用：
        1)直接引用：绝对路径，/node-name-1/node-name-2/node-name-N；
        2)别名引用：所有别名被定义在节点aliases中；
        3)lable引用：&amp;lable；
    3.属性定义：
        property ＝ value;
        再次注意value的三种方式：
        1)32bit unsigned integers：#size-cells = &lt;2&gt;;
        2)binary data：binary-property = [0x01 0x23 0x45 0x67];
        3)string/string list：device_type = "memory";

            <div>
                作者：WEINILUO 发表于2016/8/26 14:15:06 [原文链接](http://blog.csdn.net/weiniluo/article/details/52326801)
            </div>
            <div>
            阅读：108 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52326801#comments)
            </div>
