---
title: Function object And Binders
id: 154
categories:
  - C++
date: 2009-05-18 22:01:00
tags:
---

    

<span style="font-size: small;">
<p class="p0" style="margin-top: 0pt; margin-bottom: 0pt; padding: 0pt;"><span style="font-size: 10.5pt; color: #000000; font-family: '宋体'; mso-spacerun: 'yes';">这两个概念一般都是一起出现的，今天再次翻起《Ruminations&nbsp;On&nbsp;C++》，这两个概念真的如Koenig所说比较难以理解，加上这次我以前至少也看过了3次了，但是每次都是当时看懂了，过后就忘了，究其原因1:没有真正理解；2没有应用到实际中。</span>

<span style="font-size: 10.5pt; color: #000000; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;现在暂时也不会应用到实际，但是这次的理解必须写下来。其实函数对象倒还好，一个重载了()了类其对象基本上就可以叫做函数对象，Koenig的说法是：函数对象提供了一种方法，将要调用的函数与准备传递给这个函数的隐式参数捆绑起来。</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">template&lt;typename&nbsp;_Arg1,&nbsp;typename&nbsp;_Arg2,&nbsp;typename&nbsp;_Result&gt;</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;struct&nbsp;binary_function</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typedef&nbsp;_Arg1&nbsp;first_argument_type;&nbsp;&nbsp;&nbsp;///&lt;&nbsp;the&nbsp;type&nbsp;of&nbsp;the&nbsp;first&nbsp;argument</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;///&nbsp;&nbsp;(no&nbsp;surprises&nbsp;here)</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typedef&nbsp;_Arg2&nbsp;second_argument_type;&nbsp;&nbsp;///&lt;&nbsp;the&nbsp;type&nbsp;of&nbsp;the&nbsp;second&nbsp;argument</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typedef&nbsp;_Result&nbsp;result_type;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;///&lt;&nbsp;type&nbsp;of&nbsp;the&nbsp;return&nbsp;type</span>

<span style="font-size: 10.5pt; color: #000000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;};</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">template&lt;typename&nbsp;_Tp&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;struct&nbsp;greater&nbsp;:&nbsp;public&nbsp;binary_function&lt;_Tp,&nbsp;_Tp,&nbsp;bool&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bool</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;operator()(const&nbsp;_Tp&amp;&nbsp;__x,&nbsp;const&nbsp;_Tp&amp;&nbsp;__y)&nbsp;const</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;return&nbsp;__x&nbsp;&gt;&nbsp;__y;&nbsp;}</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">};</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">有了<span style="font-family: Times New Roman;">greater</span><span style="font-family: 宋体;">我们就可以定义一个对象</span></span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">greater&lt;int&gt;&nbsp;gt;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">bool&nbsp;greater1000(int&nbsp;n)</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">{</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">return&nbsp;gt(n,1000);</span><span style="font-size: 10.5pt; color: #ff0000; font-family: '宋体'; mso-spacerun: 'yes';">//<span style="font-family: 宋体;">这里就是调用了</span><span style="font-family: Times New Roman;">greater</span><span style="font-family: 宋体;">的</span><span style="font-family: Times New Roman;">operator()</span></span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">}</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">bind2nd<span style="font-family: 宋体;">可以将一个二元函数对象</span><span style="font-family: Times New Roman;">f(x,y)</span><span style="font-family: 宋体;">中的第二个参数</span><span style="font-family: Times New Roman;">y</span><span style="font-family: 宋体;">绑定到一个新的一元函数对象上</span><span style="font-family: Times New Roman;">g(x)</span><span style="font-family: 宋体;">，这时通过</span><span style="font-family: Times New Roman;">g(x)(y)</span><span style="font-family: 宋体;">即相当于</span><span style="font-family: Times New Roman;">f(x,y)</span><span style="font-family: 宋体;">了，真是妙，这不就是数学里面的复合函数，这次是第一次领悟到这一点，也算是一点进步吧。通过</span><span style="font-family: Times New Roman;">bind2nd</span><span style="font-family: 宋体;">的源码可以很清晰得看出其内部机理：</span></span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">template&lt;typename&nbsp;_Operation&gt;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;class&nbsp;binder2nd</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;:&nbsp;public&nbsp;unary_function&lt;typename&nbsp;_Operation::first_argument_type,</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;typename&nbsp;_Operation::result_type&gt;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;protected:</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Operation&nbsp;op;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typename&nbsp;_Operation::second_argument_type&nbsp;value;</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;public:</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;binder2nd(const&nbsp;_Operation&amp;&nbsp;__x,</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">const&nbsp;typename&nbsp;_Operation::second_argument_type&amp;&nbsp;__y)</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;:&nbsp;op(__x),&nbsp;value(__y)&nbsp;{&nbsp;}</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span style="font-size: 10.5pt; color: #ff0000; font-family: '宋体'; mso-spacerun: 'yes';">//<span style="font-family: 宋体;">下面的</span><span style="font-family: Times New Roman;">()</span><span style="font-family: 宋体;">重载即相当于</span><span style="font-family: Times New Roman;">f(x,y)</span><span style="font-family: 宋体;">了，只不过这时的</span><span style="font-family: Times New Roman;">y</span><span style="font-family: 宋体;">已经是</span><span style="font-family: Times New Roman;">binder2nd</span><span style="font-family: 宋体;">对象的一个成员了，当然</span><span style="font-family: Times New Roman;">f</span><span style="font-family: 宋体;">也是起一个成员，这样外部只需传递</span><span style="font-family: Times New Roman;">x</span><span style="font-family: 宋体;">了，即将</span><span style="font-family: Times New Roman;">binary</span><span style="font-family: 宋体;">转换为</span><span style="font-family: Times New Roman;">unary</span><span style="font-family: 宋体;">了。</span></span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typename&nbsp;_Operation::result_type</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;operator()(const&nbsp;typename&nbsp;_Operation::first_argument_type&amp;&nbsp;__x)&nbsp;const</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;return&nbsp;op(__x,&nbsp;value);&nbsp;}</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;_GLIBCXX_RESOLVE_LIB_DEFECTS</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;109.&nbsp;&nbsp;Missing&nbsp;binders&nbsp;for&nbsp;non-const&nbsp;sequence&nbsp;elements</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typename&nbsp;_Operation::result_type</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;operator()(typename&nbsp;_Operation::first_argument_type&amp;&nbsp;__x)&nbsp;const</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;return&nbsp;op(__x,&nbsp;value);&nbsp;}</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;}&nbsp;_GLIBCXX_DEPRECATED_ATTR;</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;///&nbsp;One&nbsp;of&nbsp;the&nbsp;@link&nbsp;s20_3_6_binder&nbsp;binder&nbsp;functors@endlink.</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;template&lt;typename&nbsp;_Operation,&nbsp;typename&nbsp;_Tp&gt;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;inline&nbsp;binder2nd&lt;_Operation&gt;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;bind2nd(const&nbsp;_Operation&amp;&nbsp;__fn,&nbsp;const&nbsp;_Tp&amp;&nbsp;__x)</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;typedef&nbsp;typename&nbsp;_Operation::second_argument_type&nbsp;_Arg2_type;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;binder2nd01&lt;_Operation&gt;(__fn,&nbsp;_Arg2_type(__x));</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">}&nbsp;</span>

<span style="font-size: 10.5pt; font-family: '宋体'; mso-spacerun: 'yes';">下面来看看<span style="font-family: Times New Roman;">MSDN</span><span style="font-family: 宋体;">上面的一个例子：</span></span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//&nbsp;functional_bind2nd.cpp</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//&nbsp;compile&nbsp;with:&nbsp;/EHsc</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">#include&nbsp;&lt;vector&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">#include&nbsp;&lt;functional&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">#include&nbsp;&lt;algorithm&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">#include&nbsp;&lt;iostream&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">using&nbsp;namespace&nbsp;std;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//&nbsp;Creation&nbsp;of&nbsp;a&nbsp;user-defined&nbsp;function&nbsp;object</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//&nbsp;that&nbsp;inherits&nbsp;from&nbsp;the&nbsp;unary_function&nbsp;base&nbsp;class</span>

<span style="font-size: 10.5pt; color: #ff0000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//<span style="font-family: 宋体;">定义一个一元函数对象</span></span><span style="font-size: 10.5pt; color: #ff0000; font-family: '宋体'; mso-spacerun: 'yes';">，判定传入的参数是否是大于<span style="font-family: Times New Roman;">15</span></span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">class&nbsp;greaterthan15:&nbsp;unary_function&lt;int,&nbsp;bool&gt;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">{</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">public:</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;result_type&nbsp;operator()(argument_type&nbsp;i)</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return&nbsp;(result_type)(i&nbsp;&gt;&nbsp;15);</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;}</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">};</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">int&nbsp;main()</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">{</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;vector&lt;int&gt;&nbsp;v1;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;vector&lt;int&gt;::iterator&nbsp;Iter;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;int&nbsp;i;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;for&nbsp;(i&nbsp;=&nbsp;0;&nbsp;i&nbsp;&lt;=&nbsp;5;&nbsp;i++)</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;{</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v1.push_back(5&nbsp;*&nbsp;i);</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;}</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;"The&nbsp;vector&nbsp;v1&nbsp;=&nbsp;(&nbsp;";</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;for&nbsp;(Iter&nbsp;=&nbsp;v1.begin();&nbsp;Iter&nbsp;!=&nbsp;v1.end();&nbsp;Iter++)</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;*Iter&nbsp;&lt;&lt;&nbsp;"&nbsp;";</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;")"&nbsp;&lt;&lt;&nbsp;endl;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;Count&nbsp;the&nbsp;number&nbsp;of&nbsp;integers&nbsp;&gt;&nbsp;10&nbsp;in&nbsp;the&nbsp;vector</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">vector&lt;int&gt;::iterator::difference_type&nbsp;result1a;</span>

<span style="font-size: 10.5pt; color: #ff0000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//</span><span style="font-size: 10.5pt; color: #ff0000; font-family: '宋体'; mso-spacerun: 'yes';">将<span style="font-family: Times New Roman;">10</span><span style="font-family: 宋体;">绑定到新的函数对象</span><span style="font-family: Times New Roman;">binder2nd(greater&lt;int&gt;(),10)</span><span style="font-family: 宋体;">上面，接着通过</span><span style="font-family: Times New Roman;">count_if</span><span style="font-family: 宋体;">来迭代，并传递参数</span><span style="font-family: Times New Roman;">(*v1.begin())</span><span style="font-family: 宋体;">，也就是实现对每一个元素调用</span><span style="font-family: Times New Roman;">greater&lt;int&gt;(*v1.begin(),10)</span></span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;result1a&nbsp;=&nbsp;count_if(v1.begin(),&nbsp;v1.end(),&nbsp;bind2nd(greater&lt;int&gt;(),&nbsp;10));</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;"The&nbsp;number&nbsp;of&nbsp;elements&nbsp;in&nbsp;v1&nbsp;greater&nbsp;than&nbsp;10&nbsp;is:&nbsp;"</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;&lt;&nbsp;result1a&nbsp;&lt;&lt;&nbsp;"."&nbsp;&lt;&lt;&nbsp;endl;</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;Compare&nbsp;counting&nbsp;the&nbsp;number&nbsp;of&nbsp;integers&nbsp;&gt;&nbsp;15&nbsp;in&nbsp;the&nbsp;vector</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;with&nbsp;a&nbsp;user-defined&nbsp;function&nbsp;object</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">vector&lt;int&gt;::iterator::difference_type&nbsp;result1b;</span>

<span style="font-size: 10.5pt; color: #ff0000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//<span style="font-family: 宋体;">这里直接传入一元函数对象</span></span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;result1b&nbsp;=&nbsp;count_if(v1.begin(),&nbsp;v1.end(),&nbsp;greaterthan15());</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;"The&nbsp;number&nbsp;of&nbsp;elements&nbsp;in&nbsp;v1&nbsp;greater&nbsp;than&nbsp;15&nbsp;is:&nbsp;"</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;&lt;&nbsp;result1b&nbsp;&lt;&lt;&nbsp;"."&nbsp;&lt;&lt;&nbsp;endl;</span>

&nbsp;

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;//&nbsp;Count&nbsp;the&nbsp;number&nbsp;of&nbsp;integers&nbsp;&lt;&nbsp;10&nbsp;in&nbsp;the&nbsp;vector</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">vector&lt;int&gt;::iterator::difference_type&nbsp;result2;</span>

<span style="font-size: 10.5pt; color: #ff0000; font-family: 'Times New Roman'; mso-spacerun: 'yes';">//</span><span style="font-size: 10.5pt; color: #ff0000; font-family: '宋体'; mso-spacerun: 'yes';">将<span style="font-family: Times New Roman;">10</span><span style="font-family: 宋体;">绑定到新的函数对象</span><span style="font-family: Times New Roman;">binder2nd(greater&lt;int&gt;(),10)</span><span style="font-family: 宋体;">上面，只不过这时</span><span style="font-family: Times New Roman;">10</span><span style="font-family: 宋体;">是作为第一个参数绑定的，接着通过</span><span style="font-family: Times New Roman;">count_if</span><span style="font-family: 宋体;">来迭代，并传递参数</span><span style="font-family: Times New Roman;">(*v1.begin())</span><span style="font-family: 宋体;">，也就是实现对每一个元素调用</span><span style="font-family: Times New Roman;">greater&lt;int&gt;(10</span><span style="font-family: 宋体;">，</span><span style="font-family: Times New Roman;">*v1.begin())</span><span style="font-family: 宋体;">，也就是比</span><span style="font-family: Times New Roman;">10</span><span style="font-family: 宋体;">小的元素个数</span></span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;result2&nbsp;=&nbsp;count_if(v1.begin(),&nbsp;v1.end(),&nbsp;bind1st(greater&lt;int&gt;(),&nbsp;10));</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;cout&nbsp;&lt;&lt;&nbsp;"The&nbsp;number&nbsp;of&nbsp;elements&nbsp;in&nbsp;v1&nbsp;less&nbsp;than&nbsp;10&nbsp;is:&nbsp;"</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;&lt;&nbsp;result2&nbsp;&lt;&lt;&nbsp;"."&nbsp;&lt;&lt;&nbsp;endl;</span>

<span style="font-size: 10.5pt; font-family: 'Times New Roman'; mso-spacerun: 'yes';">}</span>

<!--EndFragment--></span></p>
</div>