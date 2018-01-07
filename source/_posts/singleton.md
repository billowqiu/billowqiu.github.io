---
title: Singleton
id: 133
categories:
  - 设计模式
date: 2009-10-18 00:01:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 这个应该是最简单的一个设计模式，记得当初在学校学习C++时看到这样一个类，当时还觉得这确实是个技巧，但是那会基本上没有实际用到，这次在项目中则到处可见，个人觉得Singleton用到的场合还比较多，基本上叫做**Mgr之类的都是Singleton，当然经验还不足不知道是不是有些地方有滥用，我们项目中用到的是一个#define:

#define SINGLE_INSTANCE(class_name)&nbsp; /

private: /

&nbsp;&nbsp;&nbsp; class_name(); /

&nbsp;&nbsp;&nbsp; class_name(const class_name&amp;); /

&nbsp;&nbsp;&nbsp; class_name&amp; operator(const class_name&amp;);/ 

public: /

&nbsp;&nbsp;&nbsp;&nbsp; static class_name&amp; instance() /

&nbsp;&nbsp;&nbsp;&nbsp;{static class_name _instance;return _instance;}

这样假如定义了一个类：

class A

{

//这样就是一个Singleton类了

SINGLE_INSTANCE(A)

};

模板形式的更爽：

<div class="CC1"><span style="color: #ff7700;">template</span>&lt;<span style="color: #ff7700;">class</span> T&gt; <span style="color: #ff7700;">class</span> Singleton {</div>
<div class="CC1">&nbsp; Singleton(<span style="color: #ff7700;">const</span> Singleton&amp;);</div>
<div class="CC1">&nbsp; Singleton&amp; <span style="color: #ff7700;">operator</span>=(<span style="color: #ff7700;">const</span> Singleton&amp;);</div>
<div class="CC1"><span style="color: #ff7700;">protected</span>:</div>
<div class="CC1">&nbsp; Singleton() {}</div>
<div class="CC1">&nbsp; <span style="color: #ff7700;">virtual</span> ~Singleton() {}</div>
<div class="CC1"><span style="color: #ff7700;">public</span>:</div>
<div class="CC1">&nbsp; <span style="color: #ff7700;">static</span> T&amp; instance() {</div>
<div class="CC1">&nbsp;&nbsp;&nbsp; <span style="color: #ff7700;">static</span> T theInstance;</div>
<div class="CC1">&nbsp;&nbsp;&nbsp; <span style="color: #ff7700;">return</span> theInstance;</div>
<div class="CC1">&nbsp; }</div>
<div class="CC1">};</div>
<div class="CC1">&nbsp;</div>
<div class="CC1"><span style="color: #dd0000;">// A sample class to be made into a Singleton</span></div>
<div class="CC1"><span style="color: #ff7700;">class</span> MyClass : <span style="color: #ff7700;">public</span> Singleton&lt;MyClass&gt; {</div>
<div class="CC1">&nbsp; <span style="color: #ff7700;">int</span> x;</div>
<div class="CC1"><span style="color: #ff7700;">protected</span>:</div>
<div class="CC1">&nbsp;&nbsp;<span style="color: #ff7700;">friend</span> <span style="color: #ff7700;">class</span> Singleton&lt;MyClass&gt;;</div>
<div class="CC1">&nbsp; MyClass() { x = 0; }</div>
<div class="CC1"><span style="color: #ff7700;">public</span>:</div>
<div class="CC1">&nbsp; <span style="color: #ff7700;">void</span> setValue(<span style="color: #ff7700;">int</span> n) { x = n; }</div>
<div class="CC1">&nbsp; <span style="color: #ff7700;">int</span> getValue() <span style="color: #ff7700;">const</span> { <span style="color: #ff7700;">return</span> x; }</div>
<div class="CC1">};</div>
</div>