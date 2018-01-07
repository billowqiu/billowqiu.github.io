---
title: N个数组成最大整数
id: 128
categories:
  - 数据结构/算法
date: 2009-11-23 23:33:00
tags:
---

    

<span style="font-size: small;">&nbsp;最近帮一个高中同学做其专升本的数据结构试卷，总体感觉比我们那会考试时难多了，其中最后一道算法设计题如下：</span>

<span style="font-size: small;">&nbsp;</span>

**<span style="font-size: small;">&ldquo;</span><span style="font-size: small;"><span style="font-family: '宋体'; mso-spacerun: 'yes';">请设计一个程序，其功能是：从键盘输入整数n及n个无符号数，将这n个无符号数连成一个最大的多位整数并输出之。例如，将5个无符号整数</span><span style="font-family: '宋体'; mso-spacerun: 'yes';">6750，67，34，345，7</span><span style="font-family: '宋体'; mso-spacerun: 'yes';">连成767675034534。</span></span><span style="font-size: small;">&rdquo;</span>**

&nbsp;

<span style="font-size: small;">第一次将题目看错，以为很简单，当然我基本上都是用C++写的，没有太考虑其中的数据结构(没办法c++用久了，有点不习惯c那种方式)，仔细看发现，这个是要将一组数据进行某种方式的排序然后组合的，当然这里根本就没有考虑最大整数的实际意义，要不<span style="font-size: small;">767675034534肯定会溢出了，想了会本来想用个类来包装每个输入的整数，然后比较每一位大小再组合的，后来发现似乎也不简单，主要是没有找到很有说服力的规律，google下发现这是一个南京师范大学非计算机专业本科生程序竞赛试题，寒啊，可惜那个网页打不开，在换了下关键词google，竟然在百度知道里面有个人问，下面两个回答，第一个和我想的差不多，第二个真是一语道天机啊，按照字符串大小排序，最近真是状态萎靡啊，下面是我写的一个小程序：</span></span>

<span style="font-size: small;"><pre style="border: 1px dotted #785;background: #f5f5f5;">#include &lt;sstream&gt;
#include &lt;string&gt;
#include &lt;iostream&gt;
#include &lt;vector&gt;
#include &lt;algorithm&gt;
#include &lt;functional&gt;
using namespace std;

int main(int argc, char* argv[])
{
cout&lt;&lt;"sizeof(unsigned int)="&lt;&lt;sizeof(unsigned int)&lt;&lt;endl;
cout&lt;&lt;"UINT_MAX = "&lt;&lt;UINT_MAX&lt;&lt;endl;
int n;
cout&lt;&lt;"Please In put n:"&lt;&lt;endl;
cin &gt;&gt; n;

vector&lt;string&gt; vecStrInteger;
for (int i = 0 ; i&lt; n ;++i)
{
cout&lt;&lt;"Please In put The "&lt;&lt;i+1&lt;&lt;"'s unsigned integer:";
unsigned int data;
cin &gt;&gt; data;
stringstream strStream;
strStream &lt;&lt; data;
vecStrInteger.push_back(strStream.str());
}

sort(vecStrInteger.begin(),vecStrInteger.end(),greater&lt;string&gt;());
cout&lt;&lt;"The bigest integer is:"&lt;&lt;endl;
for (vector&lt;string&gt;::iterator iter = vecStrInteger.begin() ; iter != vecStrInteger.end() ; ++iter)
{
cout&lt;&lt;*iter;
}
cout&lt;&lt;endl;
return 0;
}</pre> </span>

<span style="font-size: small;">不知道这和数据结构有啥关联！！</span>

&nbsp;

<span style="font-size: small;"><span style="font-size: small;">
<p>&nbsp;

</span></span></p>
</div>