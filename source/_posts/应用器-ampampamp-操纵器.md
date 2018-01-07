---
title: 应用器 & 操纵器
id: 149
categories:
  - C++
date: 2009-06-17 23:44:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 操纵器(Manipalator)，以某种方式作用于它的参数所表示的数据；应用器(Applicator)，是一个重载操作符，它的操作数是一个可操纵的值和一个将作用于这个值得操纵器。

&nbsp;template &lt;class stype,class vtype &gt;
class fcn_obj
{
public:
&nbsp; fcn_obj(stype (*f)(stype&amp;,vtype),vtype v):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; func(f),val(v)
&nbsp; {
&nbsp; }
&nbsp; stype&amp; operator()(stype&amp; s)const
&nbsp; {
&nbsp;&nbsp;&nbsp; return (*func)(s,val);
&nbsp; }
private:
&nbsp; stype&amp; (*func)(stype&amp;,vtype),vtype v);
&nbsp; vtype val;
};

template &lt;class stype,class vtype&gt;
stype&amp; operator&lt;&lt; 
(stype&amp; ofile,const fcn_obj&lt;stype,vtype&gt;&amp; im)
{
&nbsp;&nbsp; return im(ofile);
}

fun_obj&lt;ostream long&gt;
hexconv(long n)
{
&nbsp;&nbsp; ostream (*my_hex)(ostream&amp;,long)=hexconv;
&nbsp;&nbsp; return fcn_obj&lt;ostream,long&gt;(my_hex,n);
}

&nbsp;

上面的代码来自Ruminations On C++，iostream库中使用了这个设计，使得cout&lt;&lt;"Hello,World!"&lt;&lt;endl;用起来很舒服，终于知道了endl为什么叫做操纵器了。

</div>