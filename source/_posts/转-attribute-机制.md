---
title: '[转]__attribute__机制'
tags: []
date: 2016-09-12 10:41:13
---

内核中常会用到**attribute**，今天仔细了解一下这个**attribute**机制。

### 1.概要

**attribute**可以设置函数属性（Function Attribute）、变量属性（Variable Attribute）和类型属性（Type Attribute）。它的书写特征是：**attribute**前后都有两个下划线，后面attribute-list是相应的**attribute**参数，语法格式如下： 

**attribute** ((attribute-list)) 

另外，它必须放于声明的尾部“；”之前。

### 2.函数属性

函数属性可以帮助开发者把一些特性添加到函数声明中，从而可以使编译器在错误检查方面的功能更强大。**attribute**机制也很容易同非GNU应用程序做到兼容之功效。 

    GNU CC需要使用 –Wall编译器来击活该功能，这是控制警告信息的一个很好的方式。下面介绍几个常见的属性参数。 

    **attribute** format。该**attribute**属性可以给被声明的函数加上类似printf或者scanf的特征，它可以使编译器检查函数声明和函数实际调用参数之间的格式化字符串是否匹配。该功能十分有用，尤其是处理一些很难发现的bug。format的语法格式为： 

format (archetype, string-index, first-to-check) 

format属性告诉编译器，按照printf, scanf, strftime或strfmon的参数表格式规则对该函数的参数进行检查。“archetype”指定是哪种风格；“string-index”指定传入函数的第几个参数是格式化字符串；“first-to-check”指定从函数的第几个参数开始按上述规则进行检查。 

具体使用格式如下： 

**attribute**((format(printf,m,n))) 

**attribute**((format(scanf,m,n))) 

其中参数m与n的含义为： 

m：第几个参数为格式化字符串（format string）； 

n：参数集合中的第一个，即参数“…”里的第一个参数在函数参数总数排在第几，注意，有时函数参数里还有“隐身”的呢，后面会提到； 

    在使用上，**attribute**((format(printf,m,n)))是常用的，而另一种却很少见到。下面举例说明，其中myprint为自己定义的一个带有可变参数的函数，其功能类似于printf： 

//m=1；n=2 

extern void myprint(const char *format,…) **attribute**((format(printf,1,2))); 

//m=2；n=3 

extern void myprint(int l，const char *format,…) **attribute**((format(printf,2,3))); 

需要特别注意的是，如果myprint是一个函数的成员函数，那么m和n的值可有点“悬乎”了，例如： 

//m=3；n=4 

extern void myprint(int l，const char *format,…) **attribute**((format(printf,3,4))); 

    其原因是，类成员函数的第一个参数实际上一个“隐身”的“this”指针。（有点C++基础的都知道点this指针，不知道你在这里还知道吗？） 

    这里给出测试用例：attribute.c，代码如下： 

[cpp] view plain copy 

extern void myprint(const char *format,…) **attribute**((format(printf,1,2))); 

    void test() 

    { 

        myprint(“i=%d\n”,6); 

        myprint(“i=%s\n”,6); 

        myprint(“i=%s\n”,”abc”); 

        myprint(“%s,%d,%d\n”,1,2); 

    } 

    gcc编译后会提示format argument is not a pointer的警告。若去掉**attribute**((format(printf,1,2)))，则会正常编译。需要注意的是，编译器只能识别类似printf的标准输出库函数。 

    还有一个**attribute** noreturn，该属性通知编译器函数从不返回值，当遇到类似函数需要返回值而却不可能运行到返回值处就已经退出来的情况，该属性可以避免出现错误信息。C库函数中的abort()和exit()的声明格式就采用了这种格式，如下所示：  

extern void exit(int)   **attribute**((noreturn)); 

extern void abort(void) **attribute**((noreturn)); 

为了方便理解，大家可以参考如下的例子： 

[cpp] view plain copy 

//name: noreturn.c  ；测试**attribute**((noreturn)) 

    extern void myexit(); 

    int test(int n) 

    { 

        if ( n &gt; 0 ) 

        { 

            myexit(); 

            /* 程序不可能到达这里*/ 

        } 

        else 

            return 0; 

    } 

编译后的输出结果如下： 

$gcc –Wall –c noreturn.c 

noreturn.c: In function `test’: 

noreturn.c:12: warning: control reaches end of non-void function 

    很显然，这是因为一个被定义为有返回值的函数却没有返回值。加上**attribute**((noreturn))则可以解决此问题的出现。 

    后面还有__attribute__const、-finstrument-functions、no_instrument_function等的属性描述，就不多转了，感兴趣的可以看原文。

### 3.变量属性(Variable Attribute)

    关键字__attribute__也可以对变量或结构体成员进行属性设置。这里给出几个常用的参数的解释，更多的参数可参考原文给出的连接。
    在使用__attribute__参数时，你也可以在参数的前后都加上“__”（两个下划线），例如，使用__aligned__而不是aligned，这样，你就可以在相应的头文件里使用它而不用关心头文件里是否有重名的宏定义。
    `</pre>

    aligned (alignment) 

    该属性规定变量或结构体成员的最小的对齐格式，以字节为单位。例如： 

    int x **attribute** ((aligned (16))) = 0; 

        编译器将以16字节（注意是字节byte不是位bit）对齐的方式分配一个变量。也可以对结构体成员变量设置该属性，例如，创建一个双字对齐的int对，可以这么写： 

    struct foo { int x[2] **attribute** ((aligned (8))); }; 

        如上所述，你可以手动指定对齐的格式，同样，你也可以使用默认的对齐方式。如果aligned后面不紧跟一个指定的数字值，那么编译器将依据你的目标机器情况使用最大最有益的对齐方式。例如： 

    short array[3] **attribute** ((aligned)); 

        选择针对目标机器最大的对齐方式，可以提高拷贝操作的效率。aligned属性使被设置的对象占用更多的空间，相反的，使用packed可以减小对象占用的空间。 

        需要注意的是，attribute属性的效力与你的连接器也有关，如果你的连接器最大只支持16字节对齐，那么你此时定义32字节对齐也是无济于事的。 

        使用该属性可以使得变量或者结构体成员使用最小的对齐方式，即对变量是一字节对齐，对域（field）是位对齐。 

        下面的例子中，x成员变量使用了该属性，则其值将紧放置在a的后面：  

           struct test 

              { 

                char a; 

                int x[2] **attribute** ((packed)); 

              }; 

        其它可选的属性值还可以是：cleanup，common，nocommon，deprecated，mode，section，shared，tls_model，transparent_union，unused，vector_size，weak，dllimport，dlexport等。

    ### 4.类型属性（Type Attribute）

    <pre>`关键字__attribute__也可以对结构体（struct）或共用体（union）进行属性设置。大致有六个参数值可以被设定，即：aligned, packed, transparent_union, unused, deprecated 和 may_alias。
    在使用__attribute__参数时，你也可以在参数的前后都加上“__”（两个下划线），例如，使用__aligned__而不是aligned，这样，你就可以在相应的头文件里使用它而不用关心头文件里是否有重名的宏定义。

aligned (alignment) 

该属性设定一个指定大小的对齐格式（以字节为单位），例如：  

struct S { short f[3]; } **attribute** ((aligned (8))); 

typedef int more_aligned_int **attribute** ((aligned (8))); 

    该声明将强制编译器确保（尽它所能）变量类型为struct S或者more-aligned-int的变量在分配空间时采用8字节对齐方式。 

    如上所述，你可以手动指定对齐的格式，同样，你也可以使用默认的对齐方式。如果aligned后面不紧跟一个指定的数字值，那么编译器将依据你的目标机器情况使用最大最有益的对齐方式。例如： 

struct S { short f[3]; } **attribute** ((aligned)); 

这里，如果sizeof（short）的大小为2（byte），那么，S的大小就为6。取一个2的次方值，使得该值大于等于6，则该值为8，所以编译器将设置S类型的对齐方式为8字节。 

aligned属性使被设置的对象占用更多的空间，相反的，使用packed可以减小对象占用的空间。 

需要注意的是，attribute属性的效力与你的连接器也有关，如果你的连接器最大只支持16字节对齐，那么你此时定义32字节对齐也是无济于事的。 

    使用该属性对struct或者union类型进行定义，设定其类型的每一个变量的内存约束。当用在enum类型定义时，暗示了应该使用最小完整的类型（it indicates that the smallest integral type should be used）。 

    下面的例子中，my-packed-struct类型的变量数组中的值将会紧紧的靠在一起，但内部的成员变量s不会被“pack”，如果希望内部的成员变量也被packed的话，my-unpacked-struct也需要使用packed进行相应的约束。  

struct my_unpacked_struct 

{ 

      char c; 

      int i; 

};

struct my_packed_struct 

{ 

     char c; 

     int  i; 

     struct my_unpacked_struct s; 

}**attribute** ((**packed**));

            <div>
                作者：WEINILUO 发表于2016/9/12 10:41:13 [原文链接](http://blog.csdn.net/weiniluo/article/details/52510949)
            </div>
            <div>
            阅读：108 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52510949#comments)
            </div>
