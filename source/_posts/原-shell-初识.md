---
title: '[原]shell  初识'
tags: []
date: 2016-06-29 11:06:30
---

### 1.shell简介

#### 1.1 shell

    Shell 是一个用C语言编写的程序，它是用户使用Linux的桥梁。Shell既是一种命令语言，又是一种程序设计语言。
    Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。`</pre>

    #### 1.2 shell脚本

    <pre class="prettyprint">`Shell 脚本（shell script），是一种为shell编写的脚本程序。`</pre>

    #### 1.3shell类型

    <pre class="prettyprint">`bash
    csh
    dash
    ksh
    ....`</pre>

    ### 2.shell入门

    #### 2.1编写第一个shell脚本

    <pre class="prettyprint">`test.sh:
    #!/bin/bash
    echo "Hello World !"

    注：
        #!为一个约定的标记，指定shell的相应类型，用什么解释器执行`</pre>

    #### 2.2执行脚本

    <pre class="prettyprint">`1.
        #chmod +x test.sh
        #./test.sh
    2.
        /bin/bash test.sh
    `</pre>

    ### 3.shell 变量

    #### 3.1变量定义

    <pre class="prettyprint">`var=val
    var="val"
    var='val'
    循环赋值：
    for file in 'ls /etc'
    规则：
    1.变量定义时，不使用$符号；
    2.变量名与等号之间不能有空格；
    3.首个字符必须为字母（a-z，A-Z）；
    4.中间不能有空格，可以使用下划线（_）；
    5.不能使用标点符号；
    6.不能使用bash里的关键字（可用help命令查看保留关键字）。`</pre>

    #### 3.2使用变量

    <pre class="prettyprint">`var=val
    echo $var
    echo $(var)`</pre>

    #### 3.3只读变量

    <pre class="prettyprint">`通过readonly关键字声明：
    var=val
    readonly var`</pre>

    #### 3.4删除变量

    <pre class="prettyprint">`通过unset关键字声明：
    var=val
    unset var`</pre>

    #### 3.5变量类型

    <pre class="prettyprint">`环境变量： 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。

    局部变量：局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。

    shell变量：shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行`</pre>

    #### 3.6shell字符串

    <pre class="prettyprint">`1.字符串声明：
    单引号：
        str='string'
        单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
        单引号字串中不能出现单引号（对单引号使用转义符后也不行）。
    双引号：
        str="string"
        双引号里可以有变量；
        双引号里可以出现转义字符
    无引：
        str=string
    2.拼接字符串：
        your_name="qinjx"
        greeting="hello, "$your_name" !"
        greeting_1="hello, ${your_name} !"
        echo $greeting $greeting_1
    3.获取字符串长度：
        string="abcd"
        echo ${#string} #输出 4
    4.提取子字符串：
        string="runoob is a great site"
        echo ${string:1:4} # 输出 unoo
    5.查找子字符串：
        string="runoob is a great company"
        echo `expr index "$string" is`  # 输出 8
        注意： 以上脚本中 "`" 是反引号，而不是单引号 "'"。`</pre>

    #### 3.7shell数组

    <pre class="prettyprint">`bash支持一维数组（不支持多维数组），并且没有限定数组的大小。
    1.定义数组
        array=(val0 val1 val2 ...)

        array_name=(
        value0
        value1
        value2
        value3
        )

        array_name[0]=value0
        array_name[1]=value1
        array_name[n]=valuen
    2.读取数组
        ${array[index]}
        通过"@"获得数组中的所有元素：
        echo ${array[@]}
    3.获取数组长度
        获取数组元素个数：
        length=${#array[@]}
        length=${#array[*]}
        获得单个数组元素的长度：
        length=${#array[n]}`</pre>

    #### 3.8shell注释

    <pre class="prettyprint">`1.以"#"开头行表示shell注释行
    2.shell无多行注释操作，只能每行行头添加"#"`</pre>

    ### 4.Shell 传递参数

    #### 4.1传递参数使用

    <pre class="prettyprint">`我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……
    例：
    test.sh:
    #!/bin/bash
    echo "Shell 传递参数实例！";
    echo "执行的文件名：$0";
    echo "第一个参数为：$1";
    echo "第二个参数为：$2";
    echo "第三个参数为：$3";

    效果：
    $ ./test.sh 1 2 3
    Shell 传递参数实例！
    执行的文件名：test.sh
    第一个参数为：1
    第二个参数为：2
    第三个参数为：3`</pre>

    #### 4.2特殊参数

    <pre class="prettyprint">`$# 传递到脚本的参数个数
    $* 以一个单字符串显示所有向脚本传递的参数。
        如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。
    $$    脚本运行的当前进程ID号
    $! 后台运行的最后一个进程的ID号
    $@ 与$*相同，但是使用时加引号，并在引号中返回每个参数。
        如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
    $- 显示Shell使用的当前选项，与set命令功能相同。
    $? 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。

    注：
    $* 与 $@ 区别：
    相同点：都是引用所有参数。
    不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。
    例：
    test.sh:
    #!/bin/bash
    
    echo "-- \$* 演示 ---"
    for i in "$*"; do
        echo $i
    done

    echo "-- \$@ 演示 ---"
    for i in "$@"; do
        echo $i
    done

    效果：
    $ ./test.sh 1 2 3
    -- $* 演示 ---
    1 2 3
    -- $@ 演示 ---
    1
    2
    3`</pre>

    ### 5.shell基本运算符

    <pre class="prettyprint">`原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
    expr 是一款表达式计算工具，使用它能完成表达式的求值操作。
    注意
    1.使用的是反引号 ` 而不是单引号 '；
    2.表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
    例：
    test.sh:
    #!/bin/bash

    val=`expr 2 + 2`
    echo "两数之和为 : $val"`</pre>

    #### 5.1 算数运算符

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th align="left">举例</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>+</td>
      <td align="left">加法</td>
      <td align="left">`expr b` 结果为 30。</td>
    </tr>
    <tr>
      <td>-</td>
      <td align="left">减法</td>
      <td align="left">`expr b` 结果为 10。</td>
    </tr>
    <tr>
      <td>*</td>
      <td align="left">乘法</td>
      <td align="left">`expr b` 结果为  200。</td>
    </tr>
    <tr>
      <td>/</td>
      <td align="left">除法</td>
      <td align="left">`expr a` 结果为 2。</td>
    </tr>
    <tr>
      <td>%</td>
      <td align="left">取余</td>
      <td align="left">`expr a` 结果为 0。</td>
    </tr>
    <tr>
      <td>=</td>
      <td align="left">赋值</td>
      <td align="left">a=$b 将把变量 b 的值赋给 a。</td>
    </tr>
    <tr>
      <td>==</td>
      <td align="left">相等。用于比较两个数字，相同则返回 true。</td>
      <td align="left">[ b ] 返回 false。</td>
    </tr>
    <tr>
      <td>!=</td>
      <td align="left">不相等。用于比较两个数字，不相同则返回 true。</td>
      <td align="left">[ b ] 返回 true。</td>
    </tr>
    </tbody></table>

    #### 5.2 关系运算符

    关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th align="left">返回值</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>-eq</td>
      <td align="left">检测两个数是否相等，相等返回 true。</td>
      <td align="left">[  $b ] 返回 false。</td>
    </tr>
    <tr>
      <td>-ne</td>
      <td align="left">检测两个数是否相等，不相等返回 true。</td>
      <td align="left">[  $b ] 返回 true。</td>
    </tr>
    <tr>
      <td>-gt</td>
      <td align="left">检测左边的数是否大于右边的，如果是，则返回 true。</td>
      <td align="left">[  $b ] 返回 false。</td>
    </tr>
    <tr>
      <td>-lt</td>
      <td align="left">检测左边的数是否小于右边的，如果是，则返回 true。</td>
      <td align="left">[  $b ] 返回 true。</td>
    </tr>
    <tr>
      <td>-ge</td>
      <td align="left">检测左边的数是否大等于右边的，如果是，则返回 true。</td>
      <td align="left">[ $b ] 返回 false。</td>
    </tr>
    <tr>
      <td>-le</td>
      <td align="left">检测左边的数是否小于等于右边的，如果是，则返回 true。</td>
      <td align="left">[  $b ] 返回 true。</td>
    </tr>
    </tbody></table>

    #### 5.3 布尔运算符

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th align="left">举例</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>!</td>
      <td align="left">非运算，表达式为 true 则返回 false，否则返回 true。</td>
      <td align="left">[ ! false ] 返回 true。</td>
    </tr>
    <tr>
      <td>-o</td>
      <td align="left">或运算，有一个表达式为 true 则返回 true。</td>
      <td align="left">[  20 -o $b -gt 100 ] 返回 true。</td>
    </tr>
    <tr>
      <td>-a</td>
      <td align="left">与运算，两个表达式都为 true 才返回 true。</td>
      <td align="left">[  20 -a $b -gt 100 ] 返回 false。</td>
    </tr>
    </tbody></table>

    #### 5.4 逻辑运算符

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th>举例</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>&amp;&amp;</td>
      <td align="left">逻辑的 AND</td>
      <td>[[  100 &amp;&amp; $b -gt 100 ]] 返回 false</td>
    </tr>
    <tr>
      <td>||</td>
      <td align="left">逻辑的 OR</td>
      <td>[[  100 ||  100 ]] 返回 true</td>
    </tr>
    </tbody></table>

    #### 5.5 字符串运算符

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th align="left">举例</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>=</td>
      <td align="left">检测两个字符串是否相等，相等返回 true。</td>
      <td align="left">[ b ] 返回 false。</td>
    </tr>
    <tr>
      <td>!=</td>
      <td align="left">检测两个字符串是否相等，不相等返回 true。</td>
      <td align="left">[ b ] 返回 true。</td>
    </tr>
    <tr>
      <td>-z</td>
      <td align="left">检测字符串长度是否为0，为0返回 true。</td>
      <td align="left">[ -z $a ] 返回 false。</td>
    </tr>
    <tr>
      <td>-n</td>
      <td align="left">检测字符串长度是否为0，不为0返回 true。</td>
      <td align="left">[ -n $a ] 返回 true。</td>
    </tr>
    <tr>
      <td>str</td>
      <td align="left">检测字符串是否为空，不为空返回 true。</td>
      <td align="left">[ $a ] 返回 true。</td>
    </tr>
    </tbody></table>

    #### 5.6 文件测试运算符

    <table>
    <thead>
    <tr>
      <th>运算符</th>
      <th align="left">说明</th>
      <th align="left">举例</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>-b file</td>
      <td align="left">检测文件是否是块设备文件，如果是，则返回 true。</td>
      <td align="left">[ -b $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-c file</td>
      <td align="left">检测文件是否是字符设备文件，如果是，则返回 true。</td>
      <td align="left">[ -c $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-d file</td>
      <td align="left">检测文件是否是目录，如果是，则返回 true。</td>
      <td align="left">[ -d $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-f file</td>
      <td align="left">检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。</td>
      <td align="left">[ -f $file ] 返回 true。</td>
    </tr>
    <tr>
      <td>-g file</td>
      <td align="left">检测文件是否设置了 SGID 位，如果是，则返回 true。</td>
      <td align="left">[ -g $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-k file</td>
      <td align="left">检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。</td>
      <td align="left">[ -k $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-p file</td>
      <td align="left">检测文件是否是具名管道，如果是，则返回 true。</td>
      <td align="left">[ -p $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-u file</td>
      <td align="left">检测文件是否设置了 SUID 位，如果是，则返回 true。</td>
      <td align="left">[ -u $file ] 返回 false。</td>
    </tr>
    <tr>
      <td>-r file</td>
      <td align="left">检测文件是否可读，如果是，则返回 true。</td>
      <td align="left">[ -r $file ] 返回 true。</td>
    </tr>
    <tr>
      <td>-w file</td>
      <td align="left">检测文件是否可写，如果是，则返回 true。</td>
      <td align="left">[ -w $file ] 返回 true。</td>
    </tr>
    <tr>
      <td>-x file</td>
      <td align="left">检测文件是否可执行，如果是，则返回 true。</td>
      <td align="left">[ -x $file ] 返回 true。</td>
    </tr>
    <tr>
      <td>-s file</td>
      <td align="left">检测文件是否为空（文件大小是否大于0），不为空返回 true。</td>
      <td align="left">[ -s $file ] 返回 true。</td>
    </tr>
    <tr>
      <td>-e file</td>
      <td align="left">检测文件（包括目录）是否存在，如果是，则返回 true。</td>
      <td align="left">[ -e $file ] 返回 true。</td>
    </tr>
    </tbody></table>

    ### 6.shell echo命令

    #### 6.1 显示普通字符串

    <pre class="prettyprint">`echo "It is a test"
    echo It is a test`</pre>

    #### 6.2 显示转义字符

    <pre class="prettyprint">`echo "\"It is a test\""`</pre>

    #### 6.3 显示变量

    <pre class="prettyprint">`#!/bin/sh
    read name 
    echo "$name It is a test"`</pre>

    #### 6.4 显示换行

    <pre class="prettyprint">`echo -e "OK! \n" # -e 开启转义
    echo "It it a test"`</pre>

    #### 6.5 显示不换行

    <pre class="prettyprint">`#!/bin/sh
    echo -e "OK! \c" # -e 开启转义 \c 不换行
    echo "It is a test"`</pre>

    #### 6.6 显示结果定向至文件

    <pre class="prettyprint">`echo "It is a test" &gt; myfile`</pre>

    #### 6.7 原样输出字符串，不进行转义或取变量(用单引号)

    <pre class="prettyprint">`echo '$name\"'`</pre>

    #### 6.8 显示命令执行结果

    <pre class="prettyprint">`echo `date`
    注意是反引号，适用于执行相关命令。`</pre>

    ### 7\. shell printf命令

    <pre class="prettyprint">`1.printf语法
        printf format-string [arguments ...]
    例：
    test.sh:
    #!/bin/bash
    
    printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
    printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
    printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
    printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 

    格式符使用同C语言的printf函数。`</pre>

    ### 7\. shell test命令

    Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

    #### 7.1 数值测试

    <table>
    <thead>
    <tr>
      <th>参数</th>
      <th align="left">说明</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>-eq</td>
      <td align="left">等于则为真</td>
    </tr>
    <tr>
      <td>-ne</td>
      <td align="left">不等于则为真</td>
    </tr>
    <tr>
      <td>-gt</td>
      <td align="left">大于则为真</td>
    </tr>
    <tr>
      <td>-ge</td>
      <td align="left">大于等于则为真</td>
    </tr>
    <tr>
      <td>-lt</td>
      <td align="left">小于则为真</td>
    </tr>
    <tr>
      <td>-le</td>
      <td align="left">小于等于则为真</td>
    </tr>
    </tbody></table>

    #### 7.2 字符串测试

    <table>
    <thead>
    <tr>
      <th>参数</th>
      <th align="left">说明</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>=</td>
      <td align="left">等于则为真</td>
    </tr>
    <tr>
      <td>!=</td>
      <td align="left">不相等则为真</td>
    </tr>
    <tr>
      <td>-z 字符串</td>
      <td align="left">字符串的长度为零则为真</td>
    </tr>
    <tr>
      <td>-n 字符串</td>
      <td align="left">字符串的长度不为零则为真</td>
    </tr>
    </tbody></table>

    #### 7.3 文件测试

    <table>
    <thead>
    <tr>
      <th>参数</th>
      <th align="left">说明</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>-e 文件名</td>
      <td align="left">如果文件存在则为真</td>
    </tr>
    <tr>
      <td>-r 文件名</td>
      <td align="left">如果文件存在且可读则为真</td>
    </tr>
    <tr>
      <td>-w 文件名</td>
      <td align="left">如果文件存在且可写则为真</td>
    </tr>
    <tr>
      <td>-x 文件名</td>
      <td align="left">如果文件存在且可执行则为真</td>
    </tr>
    <tr>
      <td>-s 文件名</td>
      <td align="left">如果文件存在且至少有一个字符则为真</td>
    </tr>
    <tr>
      <td>-d 文件名</td>
      <td align="left">如果文件存在且为目录则为真</td>
    </tr>
    <tr>
      <td>-f 文件名</td>
      <td align="left">如果文件存在且为普通文件则为真</td>
    </tr>
    <tr>
      <td>-c 文件名</td>
      <td align="left">如果文件存在且为字符型特殊文件则为真</td>
    </tr>
    <tr>
      <td>-b 文件名</td>
      <td align="left">如果文件存在且为块特殊文件则为真</td>
    </tr>
    </tbody></table>

    <pre class="prettyprint">`另外，Shell还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其优先级为："!"最高，"-a"次之，"-o"最低。例如：
    #!/bin/bash
    cd /bin
    if test -e ./notFile -o -e ./bash
    then
        echo '有一个文件存在!'
    else
        echo '两个文件都不存在'
    fi`</pre>

    ### 8.流程控制

    #### 8.1if else

    <pre class="prettyprint">`1.
        if condition
        then
            command1 
            command2
            ...
            commandN 
        fi
    2.
        if condition
        then
            command1 
            command2
            ...
            commandN
        else
            command
        fi
    3.
        if condition1
        then
            command1
        elif condition2 
        then 
            command2
        else
            commandN
        fi`</pre>

    #### 8.2for

    <pre class="prettyprint">`1.
        for var in item1 item2 ... itemN
        do
            command1
            command2
            ...
            commandN
        done

    无限循环：
        for (( ; ; ))
        do
            commands
        done`</pre>

    #### 8.3while

    <pre class="prettyprint">`1.
        while condition
        do
            command
        done
    无限循环：
    1.
        while :
        do
            command
        done
    2.
        while true
        do
            command
        done
    `</pre>

    #### 8.4case

    <pre class="prettyprint">`1.
        case 值 in
        模式1)
            command1
            command2
            ...
            commandN
            ;;
        模式2）
            command1
            command2
            ...
            commandN
            ;;
        esac`</pre>

    #### 8.5until

    <pre class="prettyprint">`1.
        until condition
        do
            command
        done`</pre>

    ### 8.6 跳出循环

    <pre class="prettyprint">`1.break：适用于所有循环，并且跳出所有循环
    2.continue：适用于所有循环，跳出当前循环
    3.esca：仅用于case循环中`</pre>

    ### 9.shell函数

    #### 9.1 shell函数定义

    <pre class="prettyprint">`[ function ] funname [()]

    {

        action;

        [return int;]

    }
    说明：
        1、可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
        2、参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)`</pre>

    #### 9.2 shell 函数参数

    <pre class="prettyprint">`1.
    在shell函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...
    例：
    test.sh:

    #!/bin/bash
    funWithParam(){
        echo "第一个参数为 $1 !"
        echo "第二个参数为 $2 !"
        echo "第十个参数为 $10 !"
        echo "第十个参数为 ${10} !"
        echo "第十一个参数为 ${11} !"
        echo "参数总数有 $# 个!"
        echo "作为一个字符串输出所有参数 $* !"
    }
    funWithParam 1 2 3 4 5 6 7 8 9 34 73

    效果：
    第一个参数为 1 !
    第二个参数为 2 !
    第十个参数为 10 !
    第十个参数为 34 !
    第十一个参数为 73 !
    参数总数有 11 个!
    作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !

    注意：
        $10 不能获取第十个参数，获取第十个参数需要${10}。当n&gt;=10时，需要使用${n}来获取参数。

    2.特殊参数
        同shell脚本传参相同，请回顾该小节。`</pre>

    ### 10.Shell 输入/输出重定向

    #### 10.1重定向命令表

    <table>
    <thead>
    <tr>
      <th>命令</th>
      <th align="left">说明</th>
    </tr>
    </thead>
    <tbody><tr>
      <td>command &gt; file</td>
      <td align="left">将输出重定向到 file。</td>
    </tr>
    <tr>
      <td>command &lt; file</td>
      <td align="left">将输入重定向到 file。</td>
    </tr>
    <tr>
      <td>command &gt;&gt; file</td>
      <td align="left">将输出以追加的方式重定向到 file。</td>
    </tr>
    <tr>
      <td>n &gt; file</td>
      <td align="left">将文件描述符为 n 的文件重定向到 file。</td>
    </tr>
    <tr>
      <td>n &gt;&gt; file</td>
      <td align="left">将文件描述符为 n 的文件以追加的方式重定向到 file。</td>
    </tr>
    <tr>
      <td>n &gt;&amp; m</td>
      <td align="left">将输出文件 m 和 n 合并。</td>
    </tr>
    <tr>
      <td>n &lt;&amp; m</td>
      <td align="left">将输入文件 m 和 n 合并。</td>
    </tr>
    <tr>
      <td>&lt;&lt; tag</td>
      <td align="left">将开始标记 tag 和结束标记 tag 之间的内容作为输入。</td>
    </tr>
    </tbody></table>

    #### 10.2 Here Document

    <pre class="prettyprint">`Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。
    它的基本的形式如下：
    command &lt;&lt; delimiter
        document
    delimiter
    注意：
        1.结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
        2.开始的delimiter前后的空格会被忽略掉。
    例：
    test.sh:
    #!/bin/bash
    
    cat &lt;&lt; EOF
        welcome
        feison`s blog！
    EOF

    效果：
    welcome
    feison`s blog！`</pre>

    #### 11.shell文件包含

    <pre class="prettyprint">`1.Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。
    2.文件包含语法格式：
    . filename   # 注意点号(.)和文件名中间有一空格
    或
    source filename

    注意：
        被包含的文件 test1.sh 不需要可执行权限。`</pre>

    ### 12.写在后面

    <pre>`本文是学习[菜鸟教程shell](http://www.runoob.com/linux/linux-shell.html)的笔记，记录的是本人在shell方面的薄弱点，主要是打基础，方便以后回顾，编写的不够深入和全面，大家如有不同看法可以留言评论，相互交流学习。

            <div>
                作者：WEINILUO 发表于2016/6/29 11:06:30 [原文链接](http://blog.csdn.net/weiniluo/article/details/51776869)
            </div>
            <div>
            阅读：1954 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/51776869#comments)
            </div>
