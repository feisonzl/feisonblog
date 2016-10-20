---
title: '[原]c 差缺补漏'
tags: []
date: 2016-08-25 10:08:29
---

1.类型转换 

1.1自动转换

    高        double    ←←    float
    ↑          ↑             
    ↑         long     
    ↑          ↑
    ↑        unsigned
    ↑          ↑
    低         int      ←←    char,short`</pre>

    规则：在进行自动类型转换的时候，如果原来的数是无符号数，那么在扩展的时候，高位填充的是0；如果是有符号数，那么高位填充的时符号位！这一点有点类似于“&gt;&gt;”操作符，当无符号数右移的时候，高位填充的是0；有符号数右移的时候，高位填充的是符号位。

    <pre class="prettyprint">`  1 #include &lt;stdio.h&gt;
      2
      3 int main()
      4 {
      5     char v16s;
      6     unsigned char v16u;
      7     int v32s;
      8     unsigned int v32u;
      9     v16s=0xfb;
     10     v16u=(unsigned char)v16s;
     11     v32s=(int)v16s;
     12     v32u=(unsigned int)v16s;
     13     printf("v16u:%x,v32s:%x,v32u:%x\n",v16u,v32s,v32u);
     14     v16s=0x0b;
     15     v32s=(int)v16s;
     16     v32u=(unsigned int)v16s;
     17     printf("v32s:%x,v32u:%x\n",v32s,v32u);
     18     v32s=0xfffffffb;
     19     v16s=(char)v32s;
     20     v16u=(unsigned char)v32s;
     21     printf("v16s:%x,v16u:%x\n",v16s,v16u);
     22     v16s=0xfb;
     23     v16u=0xfb;
     24     v32s=(int)v16s;
     25     v32u=(unsigned char)v16u;
     26     printf("v32s:%x,v32u:%x\n",v32s,v32u);
     27
     28     return 0;
     29 }

            <div>
                作者：WEINILUO 发表于2016/8/25 10:08:29 [原文链接](http://blog.csdn.net/weiniluo/article/details/52303134)
            </div>
            <div>
            阅读：104 评论：0 [查看评论](http://blog.csdn.net/weiniluo/article/details/52303134#comments)
            </div>
