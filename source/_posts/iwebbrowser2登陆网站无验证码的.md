---
title: IWebBrowser2登陆网站(无验证码的)
id: 131
categories:
  - VC
date: 2009-10-20 22:51:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 忙活了快两天，第一次正儿八经的使用COM(仅仅是使用)，需求是客户端登陆网站获取相关信息，比如是否有新的事务，开始通过WinInet抓包，由于是第一次接触这个，搞了两天终于登陆成功(主要是卡在SessionID上面了)，进入主界面后发现获取事务的信息不是直接在HTML中，一个做Web的同事给我分析了一遍，竟然是通过JS+AJAX +_+然后再怎么直接更新(不刷新页面)，反正是搞不清，而且这里事务的链接和登录后的页面不是在同一个服务器上面，直接GET那些页面不行。

&nbsp;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 直到没办法了昨天开始用IWebBrowser2，开始本来是想直接用ActiveX控件的，后来发现那个也就是封装了下，反正也是学习干脆直接用IWebBrowser2，算然以前知道这个东西但是没用过，硬着头皮在CodeProject上面找+MSDN，终于了解了大致流程：

&nbsp;

首先通过IWebBrowser2加载URL，为了省事没有分析HTML直接是在浏览器看源文件抠出来的关键词，通过MSHTML的COM接口设置相关的用户名和密码然后提交表单，奇怪的是IHTMLInputButtonElement 竟然没有click之类的方法，网上找到的是通过IHTMLInputButtonElement 找到对应的表单然后提交，具体可以参考源代码，代码中只试了gmail和163的，gmail可以登录成功，163的不知道怎么搞的提交后用户名和密码清空了，另外有一点需要注意的是有的网站登录&ldquo;按钮&rdquo;实际上是个图片，HTML代码大致是这样的

<span class="HTML_TAG"><span style="color: #0000ff;">&lt;</span><span class="HTML_ELM"><span style="color: #800000;">input</span></span><span style="color: #0000ff;"> </span><span class="HTML_ATR"><span style="color: #ff0000;">name</span></span><span style="color: #0000ff;">=<span class="HTML_VAL">"Submit"</span> </span><span class="HTML_ATR"><span style="color: #ff0000;">value</span></span><span style="color: #0000ff;">=<span class="HTML_VAL">"登 录"</span> </span><span class="HTML_ATR"><span style="color: #ff0000;">type</span></span><span style="color: #0000ff;">=<span class="HTML_VAL">"image"</span> </span><span class="HTML_ATR"><span style="color: #ff0000;">src</span></span><span style="color: #0000ff;">=<span class="HTML_VAL">"http://static.xxx.com/v3/www/images/btn_login.gif"</span> /&gt;</span></span>

<span class="HTML_TAG"><span style="color: #000000;">这个在MSHTML对应的是IHTMLImgElement，这个接口在网上讲述的很少，而且由于不是真正的按钮，不能通过IHTMLInputButtonElement 来获取相应的表单，我这里是通过先通过IHTMLDocument2获取IHTMLElementCollection然后通过IHTMLElementCollection::item来枚举子Form，不知道为什么用IHTMLElementCollection::tags获取的Form总是不对，获取到Form后就好办了submit，这个在Demo中没写(还要洗衣服，呵呵)，可以自己试试。</span></span>

&nbsp;

&nbsp;

Demo代码：[http://download.csdn.net/source/1757459](http://download.csdn.net/source/1757459)

</div>