---
title: C++中回调(CallBack)的使用方法
id: 144
categories:
  - C++
date: 2009-07-14 23:55:00
tags:
---

    

回调函数是一个很有用，也很重要的概念。当发生某种事件时，系统或其他函数将会自动调用你定义的一段函数。回调函数在windows编程使用的场合很多，比如Hook回调函数：MouseProc,GetMsgProc以及EnumWindows,DrawState的回调函数等等，还有很多系统级的回调过程。 一般情况下, 我们使用的回调函数基本都是采用C语言风格. 这里介绍一种C++风格的回调对象方法. 采用template实现.

template &lt; class Class, typename ReturnType, typename Parameter &gt;&nbsp;&nbsp; 
class SingularCallBack&nbsp;&nbsp; 
{&nbsp;&nbsp;&nbsp;
public:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; typedef ReturnType (Class::*Method)(Parameter);&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; SingularCallBack(Class* _class_instance, Method _method)&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //取得对象实例地址,及调用方法地址&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class_instance = _class_instance;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; method&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = _method;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; };&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; ReturnType operator()(Parameter parameter)&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // 调用对象方法&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return (class_instance-&gt;*method)(parameter);&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; };&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; ReturnType execute(Parameter parameter)&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // 调用对象方法&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return operator()(parameter);&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; };&nbsp;&nbsp; 
&nbsp; 
&nbsp; 
&nbsp;&nbsp; private:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; Class*&nbsp; class_instance;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; Method&nbsp; method;&nbsp;&nbsp; 
&nbsp; 
};&nbsp; 
示例:

以下是两个类实现.
class A&nbsp;&nbsp; 
{&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; public:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; void output()&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std::cout &lt;&lt; "I am class A :D" &lt;&lt; std::endl;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; };&nbsp;&nbsp; 
&nbsp; 
};&nbsp;&nbsp; 
&nbsp; 
class B&nbsp;&nbsp; 
{&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; public:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; bool methodB(A a)&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; a.output();&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return true;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; }&nbsp;&nbsp; 
&nbsp; 
};&nbsp;&nbsp;
&nbsp;

SingularCallBack的各种调用示例:
A a;
B b;

SingularCallBack&lt; B,bool,A &gt;* cb;
cb = new SingularCallBack&lt; B,bool,A &gt;(&amp;b,&amp;B::methodB);

if(cb-&gt;execute(a))
{
&nbsp;&nbsp; std::cout &lt;&lt; "CallBack Fired Successfully!" &lt;&lt; std::endl;
}
else
{
&nbsp;&nbsp; std::cout &lt;&lt; "CallBack Fired Unsuccessfully!" &lt;&lt; std::endl;
}
&nbsp;&nbsp;
class AClass&nbsp;&nbsp; 
{&nbsp;&nbsp; 
&nbsp;&nbsp; public:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp; AClass(unsigned int _id): id(_id){};&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp; ~AClass(){};&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp; bool AMethod(std::string str)&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; std::cout &lt;&lt; "AClass[" &lt;&lt; id &lt;&lt; "]: " &lt;&lt; str &lt;&lt; std::endl;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return true;&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp; };&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; private:&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp;&nbsp; unsigned int id;&nbsp;&nbsp; 
&nbsp; 
};&nbsp;&nbsp;&nbsp;

typedef SingularCallBack &lt; AClass, bool, std::string &gt; ACallBack;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;

int main()&nbsp;&nbsp; 
{&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; std::vector &lt; ACallBack &gt; callback_list;&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; AClass a1(1);&nbsp;&nbsp; 
&nbsp;&nbsp; AClass a2(2);&nbsp;&nbsp; 
&nbsp;&nbsp; AClass a3(3);&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; callback_list.push_back(ACallBack(&amp;a1, &amp;AClass::AMethod));&nbsp;&nbsp; 
&nbsp;&nbsp; callback_list.push_back(ACallBack(&amp;a2, &amp;AClass::AMethod));&nbsp;&nbsp; 
&nbsp;&nbsp; callback_list.push_back(ACallBack(&amp;a3, &amp;AClass::AMethod));&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; for (unsigned int i = 0; i &lt; callback_list.size(); i++)&nbsp;&nbsp; 
&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; callback_list[i]("abc");&nbsp;&nbsp; 
&nbsp;&nbsp; }&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; for (unsigned int i = 0; i &lt; callback_list.size(); i++)&nbsp;&nbsp; 
&nbsp;&nbsp; {&nbsp;&nbsp; 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; callback_list[i].execute("abc");&nbsp;&nbsp; 
&nbsp;&nbsp; }&nbsp;&nbsp; 
&nbsp; 
&nbsp;&nbsp; return true;&nbsp;&nbsp; 
&nbsp; 
}&nbsp;&nbsp;
&nbsp;

引用:

C++ Callback Solution 

本文来自CSDN博客，转载请标明出处：[http://blog.csdn.net/force_eagle/archive/2009/07/14/4347329.aspx](http://blog.csdn.net/force_eagle/archive/2009/07/14/4347329.aspx)

</div>