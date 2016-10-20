---
title: '[原]Makfile笔记'
tags: []
date: 2016-04-23 18:56:25
---

# 1.Makfile初识

## 1.1Makefile规则

    target ... : prerequisites ...
        command
        ...
        ...
    注：  
    target:目标
    prerequisites:依赖文件
    command:makefile 需要执行的命令

    Makfile规则：
        在执行Makfile时，Makfile会判断target文件是否存在，
        若不存在，则执行下面的command命令，
        若存在，则判断prerequisites的时间戳是否比target的时间戳要新，
        若是，则也执行下面的command命令。`</pre>

    ## 1.2引用其他Makefile文件

    <pre class="prettyprint">`使用方式：
    include &lt;filename&gt;
    例：
    include foo.make $(value) `</pre>

    ## 1.3Makefile变量

    <pre class="prettyprint">`1.
    使用方式：
    obj= a.o b.o c.o \
        d.o e.o
    target : prerequisites
        command $(obj)
    2.CFLAGS,CXXFLAGS 
        CFLAGS 表示用于 C 编译器的选项，CXXFLAGS表示用于 C++ 编译器的选项。
        CFLAGS指定头文件（.h文件）的路径、编译条件宏，如：CFLAGS=-I/usr/include -I/path/include -D_YUQIANG。
        上面两个变量用于gcc的参数中：$(CC) $(CFLAGS)
    3.LDFLAGS：
        gcc 等编译器会用到的一些优化参数，也可以在里面指定库文件的位置。用法：
        LDFLAGS=-L/usr/lib -L/path/to/your/lib。
    4.LIBS：告诉链接器要链接哪些库文件，如LIBS = -lpthread -liconv
    `</pre>

    ## 1.4Makefile工作流程

    <pre class="prettyprint">`1.在敲入make之后，make会在当前目录下寻找Makefile类型文件
    2.读入被include包含的makefile文件
    3.替换变量
    4.推到并分析所有规则
    5.位所有的目标创建依赖关系链
    6.找到文件中的第一个target作为最终的目标文件
    7.找到target后按照规则执行，若target的依赖文件也存在依赖文件，则同样按照规则递归执行，直到生成最终的目标文件后make执行完成`</pre>

    # 2.书写规则

    ## 2.1规则语法

    <pre class="prettyprint">`方式1：
    targets : prereuuisites
        command
        ...
    方式2：
    targets : prereuuisites ; command
        command
        ...
    注意：
    1，若command为独立行，则需以&lt;TAB&gt;开头
    2.换行符：'\'
    3.command默认使用/bash/sh来执行`</pre>

    ## 2.2通配符

    <pre class="prettyprint">`'*':代表『 0 个到无穷多个』任意字符
    '?':代表『一定有一个』任意字符
    '~':代表用户的主目录，~feison/data代表用户feison主目录下的data目录
    注：若需要使用这些特定字符，在该字符前加转义字符'\'`</pre>

    ## 2.3文件搜索路径

    <pre class="prettyprint">`1.VPATH变量
    VPATH:makefile中设定默认搜索文件路径的变量
    例：VPATH= src:../headers
    注：设置多个路径用冒号分隔
    2.vpath关键字
    vpath &lt;pattern&gt; &lt;directories&gt; :为符合模式&lt;pattern&gt;的文件指定搜索目录&lt;directories&gt;
    vpath &lt;pattern&gt; :清除符合模式&lt;pattern&gt;的文件的搜索目录
    vpath :清除所有已设置好的文件搜索目录
    例：vpath %h ../headers`</pre>

    ## 2.4伪目标

    <pre class="prettyprint">`伪目标声明方式：
    隐式声明：
    phonytargets:
        commands
    显式声明：
    .PHONY:phonytargets
    phonytargets:
        commands
    例：生成多个可执行文件
    all:prog1 prog2 prog3
    .PHONY: all

    prog1:prog1.o
        commands1
    prog2:prog2.o
        commands2
    prog3:prog3.o
        commands3`</pre>

    ## 2.5静态模式

    <pre class="prettyprint">`规则：
    &lt;targets&gt;:&lt;target-pattern&gt;:&lt;prereq-pattern&gt;
        &lt;commands&gt;
        ...
    注：
    targets：目标集，多个目标文件
    target-pattern：目标集模式
    prereq-pattern：依赖模式
    例：
    foo.o bar.o abc.m : %.o : %.c
        $(CC) -c $(CFLAGS) $&lt; -o $@
    上述的.o文件的依赖文件为.c文件，当不满足makefile原则时，执行下面的命令`</pre>

    # 3.书写命令

    ## 3.1显示命令

    <pre class="prettyprint">`1.@command和command比较
        @command 该命令不会被make显示
        command 该命令会被make显示
    例：
        @echo this is a test
        make输出：
        this is a test

        echo this is a test
        make输出：
        echo this is a test
        this is a test
    2.make -n 只显示命令，但不执行命令
    3.make -s 全面禁止命令的显示`</pre>

    ## 3.2嵌套执行make

    <pre class="prettyprint">`make嵌套，亦是make包含，以利于模块编译和分段编译
    例：
    ./Makefile:(总控Makefile)
    subsystem:
        cd subdir &amp;&amp; $(MAKE)
    &lt;==&gt;
    sybsystem:
        $(MAKE) -C subdir
    1.makefile中的变量传递
    默认情况下，上级的Makefile中的变量可以传递到下级makefile中，但不会覆盖下级makefile中的变量，除非制定了-e参数。
    export &lt;variable ...&gt; 声明要传递到下级makefile的变量
    unport &lt;variable ...&gt; 声明不能传递到下级makefile的变量
    注：
    SHELL和MAKEFLAGS两个变量始终会被传递到下级makefile中
    2.make -w
    当make进入和离开下级目录时，输出相应的目录变化信息`</pre>

    ## 3.3定义命令块

    <pre class="prettyprint">`规则：
    define cmdblocks-name
    cmd1
    cmd2
    ...
    endef
    例：
    target : prereqs
        $(cmdblocks-name)`</pre>

    # 4.变量

    ## 4.1变量声明

    <pre class="prettyprint">`1.常规变量
    variable = value
    $(variable)==&gt;value
    ${variable}==&gt;value
    2.变量中的变量
    variable ：=value
    1中的声明方式会出现无限的变量展开中，如下
    A=$(B)
    B=$(A)
    :=的声明方式规定前面的变量不能使用后面的变量,若使用的变量前面未被定义，则其值为空
    例：
    x:=foo
    y:=$(x) bar $(m)
    x:=later
    ==&gt;
    y:=foo bar
    x:=later
    3.定义一个其值为空格的变量
    nullstring:=
    space:=$(nullstring) #end of the line
    注：nullstring为Empty变量，space表示一个空格，Empty变量表示一个变量的值开始了，#表示变量的定义终止
    4.操作符?=
    variable ?=value
    表示若variable之前被定义过，则该语句无效，若没有被定义过，其值为value`</pre>

    ## 4.2变量使用

    <pre class="prettyprint">`1.多重变量
    x=y
    y=z
    a:=$($(x))
    ==&gt;
    a:=z

    2.变量值的替换
    $(var:a=b)
    ${var:a=b}
    表示把变量var中所有以a字符串结尾的a替换为b字符串
    例：
    foo := a.o b.o c.o
    bar :=$(foo:.o=.c)

    foo := a.o b.o c.o
    bar :=$(foo:%.o=%.c)

    3.追加变量
    variable +=value
    例：
    a := b
    a +=c
    ==&gt;
    a := b
    a :=$(a) b

    4.其他
    例：
    first_second = Hello
    a = first
    b = second
    c =$($(a)_$(b))
    ==&gt;
    c = Hello`</pre>

    ## 4.3override标识符

    <pre class="prettyprint">`通过make的命令行参数设置的变量，通常在makefile中对这些变量赋值是无效的，但可以通过override标识符重新设置这些变量的值。
    1.
    override &lt;variable&gt;; =&lt;value&gt;
    2.
    override &lt;variable&gt;; :=&lt;value&gt;
    3.
    override &lt;variable&gt;; +=&lt;value&gt;
    4.
    override define foo
    bar
    endef
    `</pre>

    ## 4.4多行变量

    <pre class="prettyprint">`声明方式：
    define two-lines
    echo foo
    echo $(bar)
    endef
    注：参见3.3定义命令块`</pre>

    ## 4.5环境变量

    <pre class="prettyprint">`参见3.2嵌套执行make`</pre>

    ## 4.6目标变量

    <pre class="prettyprint">`目标变量表示为某个特定的目标设定的变量，该变量的值只适用于特殊目标
    声明：
    &lt;target ...&gt;:&lt;variable-assignment&gt;
    &lt;target ...&gt;:override &lt;variable-assignment&gt;
    注：
    &lt;variable-assignment&gt;表示常规赋值表达式
    第二个声明适用于make命令带入的变量或环境变量
    例：
    m = flag
    target：m= flag_target
    target:
        @echo $(m)
    则$(m)的值为flage_target`</pre>

    ## 4.7模式变量

    <pre class="prettyprint">`声明：
    &lt;pattern ...&gt;:&lt;variable-assignment&gt;
    &lt;pattern ...&gt;:override &lt;variable-assignment&gt;
    使用方式同目标变量
    例：
    %.o : CFLAGS = -g
    prog: prog1.o prog2.o
        $(CC) $(CFLAGS ) prog1.o prog2.o
    prog1.o:prog1.c
        $(CC) $(CFLAGS ) prog1.c
    prog2.o:prog2.c
        $(CC) $(CFLAGS ) prog2.c
    以上只有prog1.o和prog2.o下的命令中的CFLAGS的值为-O，其余为系统变量的值`</pre>

    ## 4.8自动化变量

    <pre class="prettyprint">`在模式规则中（将在&lt;隐含规则&gt;一节中讲解），规则的目标和依赖文件名代表了一类文件名。命令是对所有这一类文件重建过程的描述，显然，在命令中不能指定特定的文件名，否则模式规则将没有了意义。那么在模式规则的命令行中该如何表示文件，将成我们这一小节的讨论重点。make中使用了“自动环变量”来实现这个目的，自动化变量的取值是根据具体的规则决定的，就是说对不同的规则其所代表的文件名不同。
    前边我们也看到了很多例子中使用到了自动化变量。下面对所有的自动化变量进行说明：
    $@
    代表规则中的目标文件名。如果目标是一个文档（Linux中，一般称.a文件为文档），那么它代表这个文档的文件名。在多目标的模式规则中，它代表的是哪个触发规则被执行的目标文件名。
    $%
    规则的目标文件是一个静态库文件时，代表静态库的一个成员名。例如，规则的目标是“foo.a(bar.o)”，那么，“$%”的值就为“bar.o”，“$@”的值为“foo.a”。如果目标不是函数库文件，其值为空。
    $&lt;
    规则的第一个依赖文件名。如果是隐含规则，则它代表通过目标指定的第一个依赖文件。
    $?
    所有比目标文件更新的依赖文件列表，空格分割。如果目标是静态库文件名，代表的是库成员（.o文件）的更新情况。
    $^
    规则的所有依赖文件列表，使用空格分隔。如果目标是静态库文件名，它所代表的只能是所有库成员（.o文件）名。一个文件可重复的出现在目标的依赖中，变量“$^”只记录它的一次引用情况。就是说变量“$^”会去掉重复的依赖文件。
    $+
    类似“$^”，但是它保留了依赖文件中重复出现的文件。主要用在程序链接时，库的交叉引用场合。
    $*
    在模式规则和静态模式规则中，代表“茎”。“茎”是目标模式中“%”所代表的部分（当文件名中存在目录时，“茎”也包含目录（斜杠之前）部分）。例如：文件“dir/a.foo.b”，当目标的模式为“a.%.b”时，“$*”的值为“dir/a.foo”。“茎”对于构造相关文件名非常有用。
    自动化变量“$*”需要两点说明：
    Ø        对于一个明确指定的规则来说不存在“茎”，这种情况下“$*”所代表的值发生变化。此时，如果目标文件名带有一个可识别的后缀，那么“$*”表示文件中除后缀以外的部分。例如：“foo.c”则“$*”的值为：“foo”，因为.c是一个可识别的文件后缀名。GUN make对明确规则的这种奇怪的处理行为是为了和其它版本的make兼容。通常，在除静态规则和模式规则以外，明确指定目标文件的规则中避免使用这个变量。
    Ø        当明确指定文件名的规则中目标文件名包含不可识别的后缀时，此变量为空。
    自动化变量“$?”在显式规则中也是非常有用的，规则中可以使用它来指定只对更新的依赖文件进行操作。例如，函数库文件“libN.a”，它由一些.o文件组成。如下的规则实现了根据变化的.o文件更新库文件：

    lib: foo.o bar.o lose.o win.o
        ar r lib $?

    上述列出的自动量变量中。其中有四个在规则中代表一个文件名（$@、$&lt;、$%、$*）。而其它三个的在规则中代表一个文件名的列表。GUN make中，还可以通过这七个自动化变量来获取一个完整文件名中的目录部分或者具体文件名，需要在这些变量中加入“D”或者“F”字符。这样就形成了一系列变种的自动环变量。这些变量在以前版本的make中使用，在当前版本的make中，可以使用“dir”或者“notdir”函数来实现同样的功能。
    $(@D)
    代表目标文件的目录部分（去掉目录部分的最后一个斜杠）。如果“$@”是“dir/foo.o”，那么“$(@D)”的值为“dir”。如果“$@”不存在斜杠，其值就是“.”（当前目录）。注意它和函数“dir”的区别！
    $(@F)
    目标文件的完整文件名中除目录以外的部分（实际文件名）。如果“$@”为“dir/foo.o”，那么“$(@F)”只就是“foo.o”。“$(@F)”等价于函数“$(notdir $@)”。
    $(*D)
    $(*F)
    分别代表目标“茎”中的目录部分和文件名部分。
    $(%D)
    $(%F)
    当以如“archive(member)”形式静态库为目标时，分别表示库文件成员“member”名中的目录部分和文件名部分。它仅对这种形式的规则目标有效。
    $(&lt;D)
    $(&lt;F)
    分别表示规则中第一个依赖文件的目录部分和文件名部分。
    $(^D)
    $(^F)
    分别表示所有依赖文件的目录部分和文件部分（不存在同一文件）。
    $(+D)
    $(+F)
    分别表示所有依赖文件的目录部分和文件部分（可存在重复文件）。
    $(?D)
    $(?F)
    分别表示被更新的依赖文件的目录部分和文件部分。`</pre>

    # 5.逻辑判断语句

    ## 5.1使用语法

    <pre class="prettyprint">`1.ifeq/else/endif
        &lt;1&gt;
        ifeq (&lt;arg1&gt;,&lt;arg2&gt;)
        &lt;text-ifeq&gt;
        else
        &lt;text-else&gt;
        endif

        &lt;2&gt;
        ifeq (&lt;arg1&gt;,&lt;arg2&gt;)
        &lt;text-ifeq&gt;
        endif

    2.ifneq/else/endif
        &lt;1&gt;
        ifneq (&lt;arg1&gt;,&lt;arg2&gt;)
        &lt;text-ifneq&gt;
        else
        &lt;text-else&gt;
        endif

        &lt;2&gt;
        ifneq (&lt;arg1&gt;,&lt;arg2&gt;)
            &lt;text-ifneq&gt;
        endif

    3.ifdef/else/endif
        &lt;1&gt;
        ifdef &lt;variable-name&gt;
        &lt;text-ifdef&gt;
        else
        &lt;text-else&gt;
        endif

        &lt;2&gt;
        ifdef &lt;variable-name&gt;
        &lt;text-ifdef&gt;
        endif

    4.if &lt;condition&gt;, &lt;then-part&gt;, &lt;else-part&gt;
        相当于C语言中:
        if(condition)
            then-part;
        else
            else-part;`</pre>

    # 6.常用函数

    ## 6.1函数调用语法

    <pre class="prettyprint">`语法：
    $(&lt;function&gt; &lt;arguments&gt;)
    ${&lt;function&gt; &lt;arguments&gt;}`</pre>

    ## 6.2字符串处理函数

    <pre class="prettyprint">`1.subst
    字符串替换函数
    $(subst &lt;from&gt;,&lt;to&gt;,&lt;text&gt;)
    例：
    $(subst ee,EE,feet on the street)
    ==&gt;fEEt on the strEEt

    2.patsubst
    模式字符串替换函数
    $(patsubst &lt;pattern&gt;,&lt;replacement&gt;,&lt;text&gt;)

    3.strip
    去空格函数，去掉字符串开头和结尾的空格
    $(strip &lt;string&gt;)

    4.findstring
    查找字符串函数
    $(findstring &lt;findstr&gt;,&lt;string&gt;)

    5.filter
    过滤函数
    $(filter &lt;pattern...&gt;,&lt;text&gt;)

    6.filter-out
    反过滤函数
    $(filter-out &lt;pattern...&gt;,&lt;text&gt;)

    7.sort
    排序函数
    $(sort &lt;list&gt;)

    8.word
    取单词函数
    $(word &lt;n&gt;,&lt;text&gt;)

    9.wordlist
    取单词串函数，得到从&lt;s_num&gt;到&lt;e_num&gt;的单词串
    $(wordlist &lt;s_num&gt;,&lt;e_num&gt;,&lt;text&gt;)

    10.words
    单词数统计函数
    $(words &lt;string&gt;)

    11.firstword
    取首单词函数
    $(firstword &lt;string&gt;)
    `</pre>

    ## 6.3文件名操作函数

    <pre class="prettyprint">`1.dir
    取目录函数
    $(dir &lt;path_files_name&gt;)

    2.notdir
    取文件名函数
    $(nodir &lt;path_files_name&gt;)

    3.suffix
    取后缀函数
    $(suffix &lt;names&gt;)

    4.basename
    取前缀函数
    $(basename &lt;names&gt;)

    5.addsuffix
    添加后缀函数
    $(addsuffix &lt;suffix&gt;,&lt;names&gt;)

    6.addprefix
    添加前缀函数
    $(addprefix  &lt;prefix&gt;,&lt;names&gt;)

    7.join
    连接函数
    $(join &lt;list1&gt;,&lt;list2&gt;)
    `</pre>

    ## 6.4makefile调试函数

    <pre class="prettyprint">`1.error
    产生一个致命错误
    $(error &lt;text&gt;)

    2.warning
    生成一个警告信息
    $(warning &lt;text&gt;)

    3.info
    生成一个普通信息
    $(info &lt;text&gt;)`</pre>

    ## 6.5其他函数

    <pre class="prettyprint">`1.foreach
    循环函数
    $(foreach &lt;var&gt;,&lt;list&gt;,&lt;text&gt;)

    2.if
    条件判断函数
    $(if &lt;condition&gt;,&lt;then-part&gt;)
    $(if &lt;condition&gt;,&lt;then-part&gt;,&lt;else-part&gt;)

    3.call
    调用函数的函数
    $(call &lt;func&gt;,&lt;parm1&gt;,&lt;parm2&gt;,&lt;parm3&gt;,...)
    注：
    func：自定义函数
    parm1：func中的参数1（$(1)）
    parm2: func中的参数2（$(2)）
    func中的所有参数变量均用,$(1),$(2)...表示
    例：
    reverse = $(2) $(1)
    foo=$(call reverse ,a,b)
    ==&gt;
    foo=b a

    4.origin
    变量源函数，输出变量来源
    源列表：
    &lt;1&gt;undefined:未定义变量
    &lt;2&gt;default:默认定义
    &lt;3&gt;environment:环境变量
    &lt;4&gt;file:makefile中定义的变量
    &lt;5&gt;command line:命令行定义的变量
    &lt;6&gt;override:override重新定义的变量
    &lt;7&gt;automatic:自动化变量
    $(origin &lt;variable&gt;)

    5.shell
    调用外部shell命令函数
    $(shell commands)
    例：
    fileslist：=$(shell echo *.c)
    `</pre>

    # 7.Makefile调试功能

    ## 7.1makefile调试

    <pre class="prettyprint"></pre>

    ## 7.2makefile 源码调试

    <pre class="prettyprint">`一般方式是通过makefile向源码增加宏定义
    main.c:
    #ifdef DEBUG
    .....
    Makefile:
    .PHONY: all clean

    LINUX_ROOT=$(LINUX_SRC)

    obj-m := sil9024.o

    all:
        @make -C $(LINUX_ROOT) M=$(PWD) modules "CFLAGS += -DDEBUG"
        @cp sil9024.ko ../ko/  -vfr

    clean:
        @make -C $(LINUX_ROOT) M=$(PWD) clean`</pre>

    # 8.隐含规则

    <pre class="prettyprint">`这里写代码片

# 9.写在后面

非常感谢皓哥的《跟我一起写Makefile》，之前看过一点，不过没有机会全部了解，这次基本全部了解过，这是我的一点读书笔记，主要参考的就是皓哥的这本书（姑且算书吧^-^），目的为分析安卓源码中的makefile打基础，所以研究不是很深入，以后有时间再深入了解。

            <div>
                作者：WEINILUO 发表于2016/4/23 18:56:25 [原文链接](http://blog.csdn.net/weiniluo/article/details/51136429)
            </div>
            <div>
            阅读：55 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51136429#comments)
            </div>
