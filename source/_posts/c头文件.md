---
title: C++头文件
id: 204
categories:
  - C++
date: 2006-07-12 18:18:00
tags:
---

    学了两天 C++了，基础部分基本上看了一遍，用到一些编译器时发现里面的头文件不一样，有的有后缀名而有的没有后缀名。后来上网看了下才发现好象是标准C++头文件没有后缀名，以下是在网上找到的C++头文件一览： 

传统 C++

　　#include &lt;assert.h&gt;　　　　//设定插入点&nbsp;&nbsp; 

　　#include &lt;ctype.h&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //字符处理

　　#include &lt;errno.h&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //定义错误码

　　#include &lt;float.h&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //浮点数处理

　　#include &lt;fstream.h&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //文件输入／输出

　　#include &lt;iomanip.h&gt;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //参数化输入／输出

　　#include &lt;iostream.h&gt;　　　//数据流输入／输出

　　#include &lt;limits.h&gt;　　　　//定义各种数据类型最值常量

　　#include &lt;locale.h&gt;　　　　//定义本地化函数

　　#include &lt;math.h&gt;　　　　　//定义数学函数

　　#include &lt;stdio.h&gt;　　　　 //定义输入／输出函数

　　#include &lt;stdlib.h&gt;　　　　//定义杂项函数及内存分配函数

　　#include &lt;string.h&gt;　　　　//字符串处理

　　#include &lt;strstrea.h&gt;　　　//基于数组的输入／输出

　　#include &lt;time.h&gt;　　　　　//定义关于时间的函数

　　#include &lt;wchar.h&gt;　　　　 //宽字符处理及输入／输出

　　#include &lt;wctype.h&gt;　　　　//宽字符分类

标准 C++　（同上的不再注释）

　　#include &lt;algorithm&gt;　　　 //STL 通用算法

　　#include &lt;bitset&gt;　　　　　//STL 位集容器

　　#include &lt;cctype&gt;

　　#include &lt;cerrno&gt;

　　#include &lt;clocale&gt;

　　#include &lt;cmath&gt;

　　#include &lt;complex&gt;　　　　 //复数类

　　#include &lt;cstdio&gt;

　　#include &lt;cstdlib&gt;

　　#include &lt;cstring&gt;

　　#include &lt;ctime&gt;

　　#include &lt;deque&gt;　　　　　 //STL 双端队列容器

　　#include &lt;exception&gt;　　　 //异常处理类

　　#include &lt;fstream&gt;

　　#include &lt;functional&gt;　　　//STL 定义运算函数（代替运算符）

　　#include &lt;limits&gt;

　　#include &lt;list&gt;　　　　　　//STL 线性列表容器

　　#include &lt;map&gt;　　　　　　 //STL 映射容器

　　#include &lt;iomanip&gt;

　　#include &lt;ios&gt;　　　　　　 //基本输入／输出支持

　　#include &lt;iosfwd&gt;　　　　　//输入／输出系统使用的前置声明

　　#include &lt;iostream&gt;

　　#include &lt;istream&gt;　　　　 //基本输入流

　　#include &lt;ostream&gt;　　　　 //基本输出流

　　#include &lt;queue&gt;　　　　　 //STL 队列容器

　　#include &lt;set&gt;　　　　　　 //STL 集合容器

　　#include &lt;sstream&gt;　　　　 //基于字符串的流

　　#include &lt;stack&gt;　　　　　 //STL 堆栈容器　　　　

　　#include &lt;stdexcept&gt;　　　 //标准异常类

　　#include &lt;streambuf&gt;　　　 //底层输入／输出支持

　　#include &lt;string&gt;　　　　　//字符串类

　　#include &lt;utility&gt;　　　　 //STL 通用模板类

　　#include &lt;vector&gt;　　　　　//STL 动态数组容器

　　#include &lt;cwchar&gt;

　　#include &lt;cwctype&gt;

　　using namespace std;

C99 增加

&nbsp;&nbsp; #include &lt;complex.h&gt;　　 //复数处理

&nbsp;&nbsp; #include &lt;fenv.h&gt;　　　　//浮点环境

&nbsp;&nbsp; #include &lt;inttypes.h&gt;　　//整数格式转换

&nbsp;&nbsp; #include &lt;stdbool.h&gt;　　 //布尔环境

&nbsp;&nbsp; #include &lt;stdint.h&gt;　　　//整型环境

&nbsp;&nbsp; #include &lt;tgmath.h&gt;　　　//通用类型数学宏

</div>