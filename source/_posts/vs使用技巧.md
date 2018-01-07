---
title: VS使用技巧
id: 119
categories:
  - VC
date: 2010-05-21 19:40:00
tags:
---

    

&nbsp;&nbsp; &nbsp; &nbsp; 各位搞开发的调试代码想必是少不了的，很多情况下都是为了查看当前内存变量，自从用VS2008后，发现其调试功能相对于之前使用的VS98那可是加强不少，个人体会最深的是对于STL调试时很容易看到其内部结构。

&nbsp;&nbsp; &nbsp; &nbsp; 再说说调试中遇到的问题，

1：有时我们定义了一个包含多个嵌套结构的类或者是很长的继承链，当需要查看最底层的某个属性时往往在监视窗口中点'+''-'号半天，这样严重影响了效率和情绪(尤其是催得紧时)，举个例子下面为了查看某个ACE_INET_Addr 对象内部的实际IP地址：

![ACE地址](http://hi.csdn.net/attachment/201005/21/0_1274441034eb7O.gif)

点了半天加号才找到目的，有时点错了还得狂点减号，+_+。

这里贴上一个定制调试器直接展开预设结构的方法：

首先在找到VS的&ldquo;...Common7/Packages/Debugger/autoexp.dat&rdquo;文件

<p>添加以下到[Visualizer]段，

; ACE

ACE_INET_Addr {

&nbsp;&nbsp;preview &nbsp;(

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; #(

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; "ip=",

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [(unsigned short)$e.inet_addr_.in4_.sin_addr.S_un.S_un_b.s_b1],

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ".",

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [(unsigned short)$e.inet_addr_.in4_.sin_addr.S_un.S_un_b.s_b2],

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ".",

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [(unsigned short)$e.inet_addr_.in4_.sin_addr.S_un.S_un_b.s_b3],

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ".",

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [(unsigned short)$e.inet_addr_.in4_.sin_addr.S_un.S_un_b.s_b4],

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; " port=",

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; [(unsigned short)( ($e.inet_addr_.in4_.sin_port &gt;&gt; 8) + (($e.inet_addr_.in4_.sin_port &amp; 0x00ff) &lt;&lt; 8 ) ) ]

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;)

&nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;)

}

可以显示ACE_INET_Addr为127.0.0.1:1234格式。

这样以后就方便多了，尤其是经常碰到这样的结构时。

2：我们一般为了进入函数内部经常需要按F11，这对于我们学习别人的代码很有帮助，但是当你只是为了跟到某个函数内部，而这个函数的参数里面又有一些其他构造或者调用代码时，一般都是先进入后者，比如说有下面一段代码：

void test( std::string strTest, boost::share_ptr spTest);

当你调试到调用test时如果直接按F11会首先进到std::string构造函数，接着再按F11会进入boost::share_ptr构造函数，你得再次按F11才会进入test，烦了吧，O(&cap;_&cap;)O哈哈~，这时我们可以这样弄：

<p>在HKEY_LOCAL_MACHINE/SOFTWARE/Microsoft/VisualStudio/9.0/NativeDE/StepOver下添加字符串类型的键值：

1) 键名stl，键值std/:/:.*=NoStepInto

2) 键名boost, 键值boost/:/:.*=NoStepInto

值为正则表达式，/scope表示作用域，/funct表示函数，/oper表示操作符

至此你只需一次F11即可进入test了。

</p>

&nbsp;

以上技巧来源于一位牛X的同事，为了方便以后查询，也为遇到同样问题的人提供方便，特留个记号。

</p>

&nbsp;

写到这里突然想到最近一直在琢磨OO，这里之所以叫技巧，是因为当你知道这个东西后你可以按照同样的方法进行扩展，而不需要太多的学习，而思想却不是这样，你设计并实现某个类，但是并代表不需要学习其他类的设计了，设计真的需要经验，当然也需要灵感，毕竟是设计不是实现。

</div>