---
title: '[原]android系统重启流程分析'
tags: []
date: 2016-05-31 09:40:46
---

### 1.reboot命令实现

    ./kernel/arch/alpha/kernel/setup.c:
    void __init setup_arch(char **cmdline_p){
    ...
    if (mdesc-&gt;restart_mode)
            reboot_setup(&amp;mdesc-&gt;restart_mode);
    ...
    if (mdesc-&gt;restart)
            arm_pm_restart = mdesc-&gt;restart;
    ...
    }
    ./mediatek/platform/mt6572/kernel/core/core.c:
    MACHINE_START(MT6572, "MT6572")
        .atag_offset    = 0x00000100,
        .map_io         = mt_map_io,
        .init_irq       = mt_init_irq,
        .timer          = &amp;mt6572_timer,
        .init_machine   = mt_init,
        .fixup          = mt_fixup,
        .restart        = arm_machine_restart,
        .reserve        = mt_reserve,
    MACHINE_END
    ./kernel/arch/arm/kernel/process.c:
    void arm_machine_restart(char mode, const char *cmd){
    ...
    arch_reset(mode, cmd);
    ...
    }
    ./mediatek/kernel/kernel/system.c:
    void arch_reset(char mode, const char *cmd)
    {
        char reboot = 0;
        int res=0;
        struct wd_api*wd_api = NULL;

        res = get_wd_api(&amp;wd_api);
        printk("arch_reset: cmd = %s\n", cmd ? : "NULL");

        if (cmd &amp;&amp; !strcmp(cmd, "charger")) {
            /* do nothing */
        } else if (cmd &amp;&amp; !strcmp(cmd, "recovery")) {
            rtc_mark_recovery();
        } else if (cmd &amp;&amp; !strcmp(cmd, "bootloader")){
                rtc_mark_fast();    
        } 
    #ifdef MTK_KERNEL_POWER_OFF_CHARGING
        else if (cmd &amp;&amp; !strcmp(cmd, "kpoc")){
            rtc_mark_kpoc();
        }
    #endif
        else {
            reboot = 1;
            //printk("rtc_mark_powerkey\n");//zhanglei add
            //rtc_mark_powerkey();//zhanglei add

        }

        if(res){
            printk("arch_reset, get wd api error %d\n",res);
        } else {
            wd_api-&gt;wd_sw_reset(reboot);
        }
    }
    arch_reset根据上层传的命令执行不同的动作，最后执行wd_sw_reset(reboot)。
    ./mediatek/kernel/drivers/wdk/wd_api.c:
    static int wd_sw_reset(int type)
    {
       wdt_arch_reset(type);
       return 0;
    }
    ./mediatek/platform/mt6572/kernel/drivers/wdt/mtk_wdt.c:
    void wdt_arch_reset(char mode)
    {
    ...
    DRV_WriteReg32(MTK_WDT_RESTART, MTK_WDT_RESTART_KEY);
    ...
    }
    从上可以产出系统重启动作是通过看门狗完成的，如果关闭IPO（快速开关机功能）后，系统重启动作会导致系统完全掉电，内核关闭，看门狗无法正常工作，从而导致系统无法重启。`

    ### 2.开机boot_mode_check

    ass="prettyprint">`preloader:
    ./mediatek/platform/mt6572/preloader/src/drivers/platform.c:
    void platform_init(void)
    {
    ...
    g_boot_reason = reason = platform_boot_status();
    ...
    }
    static boot_reason_t platform_boot_status(void)
    {
    ...
    #if defined (MTK_KERNEL_POWER_OFF_CHARGING)
        ulong begin = get_timer(0);
        do  {
            if (rtc_boot_check()) {
                print("%s RTC boot!\n", MOD);
                return BR_RTC;
            }
            if(!kpoc_flag)
                break;
        } while (get_timer(begin) &lt; 1000 &amp;&amp; kpoc_flag);
    #else
        if (rtc_boot_check()) {
            print("%s RTC boot!\n", MOD);
            return BR_RTC;
        }

    #endif
    ...
    }
    ./mediatek/platform/mt6572/preloader/src/drivers/mt_rtc.c:
    bool rtc_boot_check(void)
    {
    ...
    if ((pdn1 &amp; 0x0030) == 0x0010) {    /* factory data reset */
            /* keep bit 4 set until rtc_boot_check() in U-Boot */
            return true;
        }
        //zhanglei add
        if (pdn1 &amp; 0x0080) {    /*  power on  time */
            /* keep bit 7 set until rtc_boot_check() in U-Boot */
            return true;
        }
    #if defined (MTK_KERNEL_POWER_OFF_CHARGING)
        if ((pdn1 &amp; 0x4000) == 0x4000) {   
            kpoc_flag = true;
            return false;
        }
    #endif

        return false;
    }
    通过判断pdn1寄存器的标志位返回是否是真正的启动状态。
    lk:
    ./mediatek/platform/mt6572/lk/platform.c:
    void platform_init(void)
    {
    ...
    #ifndef CFG_POWER_CHARGING
        /* NOTE: if define CFG_POWER_CHARGING, will rtc_boot_check() in mt65xx_bat_init() */
        rtc_boot_check(false);
    #endif
    ...
    }
    ./mediatek/platform/mt6572/lk/mt_rtc.c:
    bool rtc_boot_check(bool can_alarm_boot)
    {
    ...
        if ((pdn1 &amp; RTC_PDN1_RECOVERY_MASK) == RTC_PDN1_FAC_RESET) {    /* factory data reset */
            RTC_Write(RTC_PDN1, pdn1 &amp; ~RTC_PDN1_FAC_RESET);
            rtc_write_trigger();
            return true;
        }

        if (pdn1 &amp; 0x0040) {    /* bypass power key detection */
            RTC_Write(RTC_PDN1, pdn1 &amp; ~0x0040);
            rtc_write_trigger();
            return true;
        }
        //zhanglei add
        if (pdn1 &amp; 0x0080) {    /*  power on  time */
            /* keep bit 7 set until rtc_boot_check() in U-Boot */
            return true;
        }

        return false;
    ...
    }
    原理同preloader层。

            
                作者：WEINILUO 发表于2016/5/31 9:40:46 [原文链接](http://blog.csdn.net/weiniluo/article/details/51539783)
            
            
            阅读：322 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51539783#comments)
            
