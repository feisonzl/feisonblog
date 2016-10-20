---
title: '[原]list_for_each_entry深入理解'
tags: []
date: 2016-08-03 14:49:58
---

在内核编程中经常会遇到list_for_each_entry(pos, head, member)，今天深入分析一下list_for_each_entry在内核中是如何定义的。 

list_for_each_entry主要作用是遍历给定类型的链表，被定义在include/linux/list.h中：

    /**
     * list_for_each_entry  -   iterate over list of given type
     * @pos:    the type * to use as a loop cursor.
     * @head:   the head for your list.
     * @member: the name of the list_struct within the struct.
     */
    #define list_for_each_entry(pos, head, member)              \
        for (pos = list_entry((head)-&gt;next, typeof(*pos), member);  \
             &amp;pos-&gt;member != (head);    \
             pos = list_entry(pos-&gt;member.next, typeof(*pos), member))`

    接着需要理解list_entry，list_entry的作用是获取条目的结构类型，同样被定义在include/linux/list.h中：

    ass="prettyprint">`/**
     * list_entry - get the struct for this entry
     * @ptr:    the &amp;struct list_head pointer.
     * @type:   the type of the struct this is embedded in.
     * @member: the name of the list_struct within the struct.
     */
    #define list_entry(ptr, type, member) \
        container_of(ptr, type, member)`

    container_of作用是通过结构体成员变量的地址和结构体成员变量名以及结构体类型获取该结构体变量的地址，被定义在include/linux/kernel.h中：

    ass="prettyprint">`/**
     * container_of - cast a member of a structure out to the containing structure
     * @ptr:    the pointer to the member.
     * @type:   the type of the container struct this is embedded in.
     * @member: the name of the member within the struct.
     *
     */
    #define container_of(ptr, type, member) ({          \
        const typeof( ((type *)0)-&gt;member ) *__mptr = (ptr);    \
        (type *)( (char *)__mptr - offsetof(type,member) );})`

    const typeof( ((type *)0)-&gt;member ) *__mptr = (ptr); 

    typeof是c    语言的一个新拓展，有多重用法，主要用于得到其参数的类型，参数可以是表达式，函数，类型等，这里表示获取成员member的类型。 

    这里定义的局部指针变量__mptr被赋值为member的地址。

    (type _)( (char _)__mptr - offsetof(type,member) ); 

     (char *)__mptr：将__mptr转换为字节类型地址。 

    offsetof(type, member)的作用是返回member在type中的偏移，其在include/linux/stddef.h中的定义如下：

    ass="prettyprint">`#ifdef __compiler_offsetof
    #define offsetof(TYPE,MEMBER) __compiler_offsetof(TYPE,MEMBER)
    #else
    #define offsetof(TYPE, MEMBER) ((size_t) &amp;((TYPE *)0)-&gt;MEMBER)
    #endif

用实际的结构体成员变量的地址减去成员变量在结构体中的偏移地址，自然就得到该结构体变量的起始地址，最终得到(type*)类型的指针。

            
                作者：WEINILUO 发表于2016/8/3 14:49:58 [原文链接](http://blog.csdn.net/weiniluo/article/details/52103145)
            
            
            阅读：114 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52103145#comments)
            
