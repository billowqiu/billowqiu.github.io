---
title: HTTP协议之处理Cookie
id: 135
categories:
  - 系统编程
date: 2009-10-16 15:13:00
tags:
---

    

_&ldquo;由于工作需要，最近在学习HTTP协议相关的一些知识，在登陆一个jsp网站时，POST过去的请求被拒绝了，通过抓包分析可以看到此网站需要设置Cookie，这里找打一篇文章写得很不错，只截抄了关于Cookie协议的一部分&rdquo;_

&nbsp;

> 大多数的 Web 应用程序都要求维护某种会话状态，如用户购物车的内容。这种会话状态的保持很多情况下需要借助于Cookie或者Session的帮助。本文结合在线页面翻译 （Machine Translation System）项目中对于Cookie的处理方法，探讨一下如何在HTTP应用代理中正确处理Cookie的传递和管理问题。
<!--START RESERVED FOR FUTURE USE INCLUDE FILES--><!-- include java script once we verify teams wants to use this and it will work on dbcs and cyrillic characters --><!--END RESERVED FOR FUTURE USE INCLUDE FILES-->

读者定位为具有Java和Web开发经验的开发和设计人员。

读者可以学习到关于Cookie的工作原理和Cookie协议的细节，以及在一个HTTP应用代理的场景下Cookie的管理和处理思想，并可以直接使用文中的代码和思路，提高工作效率。

随着越来越多的系统移植到了Web上，HTTP协议具有了比以前更广泛的应用。不同的系统对WEB实现提出了不同的要求，基于HTTP协议的网络应用正趋于复杂化和多元化。很多应用需要把用户请求的页面进行处理后再返回给用户，比如页面关键字过滤，页面内容缓存、内容搜索、页面翻译等等。这些应用在实际效果上类似于一个HTTP应用代理：它们首先接受用户的请求，根据用户请求的URL去真正的目标服务器取回目标页面，再根据不同应用的要求做出相应处理后返回给用户。这样用户直接面对的就是这个HTTP应用代理，而通过它与其他页面进行交互。Cookie或Session技术的应用，解决了HTTP协议的一个问题 -- 无法保持客户状态，因此它现在被广泛应用于各种Web站点中。上面提到的那些应用如果不能处理好Cookie和Session的传递、更新和废除等问题，就会极大的限制它们所能处理站点的范围，因此如何在HTTP应用代理中正确处理Cookie，成为一个必须解决的问题。本文结合在页面翻译（Machine Translation System）项目中对于Cookie的处理方法，探讨一下这方面的解决方案。

&nbsp;

Machine Translation System（以下简称MTS）是一个在线实时页面翻译系统，为用户在线提供把英文页面翻译成其他9种语言的服务。用户通过向MTS系统提交一个类似下面的URL使用此服务，其中参数url指明了用户所需要翻译的目标地址，参数language指明了所需翻译成的目标语言，www.mts.com是假想中提供MTS服务的站点。

[<span style="color: #5c81a7;">HTTP://www.mts.com/translate?url=http://www.ibm.com/&amp;language=French</span>](http://www.mts.com/translate?url=http://www.ibm.com/&amp;language=French)

一个完整的MTS系统处理过程可以分解成以下几个步骤：

*   用户向MTS提交合适的URL。*   MTS在接到用户的请求后，解析出用户需要翻译的目标地址和目标语言，根据用户请求的目标地址，把请求转发到目标服务器。*   MTS接受来自目标服务器的应答，包括页面信息和HTTP头信息。*   MTS在确定得到正确的目标页面后，把页面内容送入WebSphere Translation Server进行翻译。*   把翻译后的页面连同修改后的HTTP头信息提交给用户。

<a name="N1006C">**MTS逻辑图**</a>
![MTS逻辑图](http://www.ibm.com/developerworks/cn/java/j-cookie/images/image002.jpg)

当然，这其中涉及到很多的应用处理。比如与各种HTTP/HTTPS站点建立联结、根据HTTP头信息进行页面跳转和错误处理、为始终保持用户在翻译模式下而对目标的HTML页面进行分析和修改，根据系统设置对某些DNT（Do Not Translate）的页面进行过滤和跳转，当然还有对Cookie的处理等等。其他问题跟这篇文章关联不大，我们重点讨论在这种情况下的Cookie处理。Cookie跟随目标服务器的HTTP头信息被MTS接收到，经过MTS整理之后发给客户端浏览器。MTS在接到下一次用户对同一个站点的翻译请求时，再把从客户端得到的Cookie发送给目标服务器。

在以上的场景中，MTS充当的作用类似于一种HTTP应用代理服务器，它代替用户取得目标页面，并在作出相应处理后再提交给用户。当然，这种代理服务器不需要用户修改浏览器的代理服务器参数或者网络配置，而只是简单的在浏览器的地址栏中输入一个MTS能够识别的URL即可。此篇文章也是在这样一个应用场景的基础上，展开对HTTP应用代理服务器如何处理Cookie的讨论。

<a name="N10082"><span class="atitle">问题的产生</span></a>

在MTS系统中，目标服务器的Cookie在两个地方会产生问题。当MTS接收目标服务器应答的时候，Cookie随着HTTP头信息被MTS接收到的。这时候目标服务器认为MTS就是最终客户，因此它赋予了Cookie与目标服务器相符的属性。而如果MTS把这些Cookie原封不动的保存在HTTP头信息中，传给真正的最终用户的话，用户的浏览器会因为这些Cookie不合法而忽略它们。同理，当Cookie从浏览器端传回目标服务器的时候，也会遇到相同的问题。因此有必要对Cookie进行一些处理，以保证用户的浏览器能真正识别和利用这些Cookie。

但是为何用户浏览器无法识别从目标服务器传过来的原始Cookie呢？这是因为出于安全性的考虑，Cookie规范制定的时候对Cookie的产生和接受设置了一些严格的规范，不符合这些规范的Cookie，浏览器和服务器都将予以忽略。下面我们从Cookie规范入手进行介绍。

&nbsp;

目前有以下几种Cookie规范：

*   Netscape cookie草案：是最早的cookie规范，基于rfc2109。尽管这个规范与rc2109有较大的差别，但是很多服务器都与之兼容。*   rfc2109， 是w3c发布的第一个官方cookie规范。理论上讲，所有的服务器在处理cookie(版本1)时，都要遵循此规范。遗憾的是，这个规范太严格了，以致很多服务器不正确的实施了该规范或仍在使用Netscape规范。*   rfc2965规范定义了cookie版本2，并说明了cookie版本1的不足。

rfc2965规范的使用，目前并不多。rfc2109规范相应要严格得多，在实际应用上，并不是所有的浏览器和Web服务器都严格遵守。因此相比较而言，Netscape cookie草案倒是一个比较简洁和被广泛支持的Cookie规范，因此我们在这里以Netscape cookie草案为基础进行讨论，对于其他两种规范，我们的讨论和代码具有相同的意义。关于Netscape cookie草案的细节，大家可以参照Netscape官方站点，这里我们列举一些和我们讨论有关的内容。

根据Netscape cookie草案的描述，Cookie 是Web 服务器向用户的浏览器发送的一段ASCII码文本。一旦收到Cookie，浏览器会把Cookie的信息片断以"名/值"对(name-value pairs)的形式储存保存在本地。这以后，每当向同一个Web 服务器请求一个新的文档时，Web 浏览器都会发送之站点以前存储在本地的Cookie。创建Cookie的最初目的是想让Web服务器能够通过多个HTTP请求追踪客户。有些复杂的网络应用需要在不同的网页之间保持一致，它们需要这种会话状态的保持能力。

浏览器与Web服务器通过HTTP协议进行通讯，而Cookie就是保存在HTTP协议的请求或者应答头部（在HTTP协议中，数据包括两部分，一部分是头部，由一些名值对构成，用来描述要被传输数据的一些信息。一部分是主体(body)，是真正的数据（如HTML页面等））进行传送的。

在HTML文档被发送之前，Web服务器通过传送HTTP 包头中的Set-Cookie 消息把一个cookie 发送到用户的浏览器中。下面是一个遵循Netscape cookie草案的完整的Set-Cookie 头：

<table border="0" cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<td class="code-outline">
<pre class="displaycode">Set-Cookie：customer=huangxp; path=/foo; domain=.ibm.com; 
expires= Wednesday, 19-OCT-05 23:12:40 GMT; [secure]
</pre>
</td>
</tr>
</tbody>
</table>

Set-Cookie的每个属性解释如下：

*   Customer=huangxp 一个"名称＝值"对，把名称customer设置为值"huangxp"，这个属性在Cookie中必须有。*   path=/foo 控制哪些访问能够触发cookie 的发送。如果没有指定path，cookie 会在所有对此站点的HTTP 传送时发送。如果path=/directory，只有访问/directory 下面的网页时，cookie才被发送。在这个例子中，用户在访问目录/foo下的内容时，浏览器将发送此cookie。如果指定了path，但是path与当前访问的url不符，则此cookie将被忽略。*   domain=.ibm.com 指定cookie被发送到哪台计算机上。正常情况下，cookie只被送回最初向用户发送cookie 的计算机。在这个例子中，cookie 会被发送到任何在.ibm.com域中的主机。如果domain 被设为空，domain 就被设置为和提供cookie 的Web 服务器相同。如果domain不为空，并且它的值又和提供cookie的Web服务器域名不符，这个Cookie将被忽略。*   expires= Wednesday, 19-OCT-05 23:12:40 GMT 指定cookie 失效的时间。如果没有指定失效时间，这个cookie 就不会被写入计算机的硬盘上，并且只持续到这次会话结束。*   secure 如果secure 这个词被作为Set-Cookie 头的一部分，那么cookie 只能通过安全通道传输（目前即SSL通道）。否则，浏览器将忽略此Cookie。

一旦浏览器接收了cookie，这个cookie和对远端Web服务器的连续请求将一起被浏览器发送。例如 前一个cookie 被存入浏览器并且浏览器试图请求 URL http://www.ibm.com/foo/index.html 时，下面的HTTP 包头就被发送到远端的Web服务器。

GET /foo/index.html HTTP/1.0
Cookie：customer=huangxp 

&nbsp;

在了解了Cookie协议的一些基本内容之后，让我们看看一次典型的网络浏览过程中浏览器如何识别和处理Cookie：

*   浏览器对于Web服务器应答包头中Cookie的操作步骤：
1\. 从Web服务器的应答包头中提取所有的cookie。
2\. 解析这些cookie的组成部分（名称，值，路径等等）。
3\. 判定主机是否允许设置这些cookie。允许的话，则把这些Cookie存储在本地。*   浏览器对Web服务器请求包头中所有的Cookie进行筛选的步骤：
1\. 根据请求的URL和本地存储cookie的属性，判断那些Cookie能被发送给Web服务器。
2\. 对于多个cookie，判定发送的顺序。
3\. 把需要发送的Cookie加入到请求HTTP包头中一起发送。

以上我们了解了在一个典型的浏览器与Web服务器交互的时候，Cookie的传递过程。下面我们将看到，如果在MTS代理网络浏览的过程中，不对Cookie进行修改，上面的Cookie传递过程将无法实现。

1\. 假设用户希望把 http://www.ibm.com/foo/index.html 页面翻译成法文，应该使用如下的url对MTS发出请求

： 

http://www.mts.com/translate?url=http://www.ibm.com/foo/index.html&amp;language=French

2\. MTS接收用户的请求，连接远程目标服务器 http://www.ibm.com/foo/index.html。目标服务器做出应答，返回HTTP头和HTML页面内容。其中，典型的HTTP头内容如下：

<table border="0" cellspacing="0" cellpadding="0" width="100%">
<tbody>
<tr>
<td class="code-outline">
<pre class="displaycode">HTTP/1.1 200 OK
Date: Mon, 24 Oct 2005 06:54:41 GMT
Server: IBM_HTTP_Server
Cache-Control: no-cache
Content-Length: 19885
Connection: close
Set-Cookie：customer=huangxp; path=/foo; domain=.ibm.com; 
expires= Wednesday, 19-OCT-05 23:12:40 GMT
Content-Type: text/html
</pre>
</td>
</tr>
</tbody>
</table>

3\. MTS不对Set-Cookie后的内容作任何处理，直接把它加到用户浏览器的应答头上发送给浏览器。

4\. 浏览器将从Set-Cookie中解析出domain和path的值，分别是.ibm.com和/foo，并与请求的url：http://www.mts.com/translate?url=http://www.ibm.com/foo/index.html&amp;language=French进行比较。请求url的domain是www.mts.com，path是/，与Set-Cookie中的属性不符，所以浏览器将忽略此Cookie。

另外，在浏览器发送Cookie的时候也会遇到同样的问题，同样如上例，如果浏览器里本来已经存储了http://www.ibm.com/foo/的Cookie，但由于用户要通过MTS访问此站点，浏览器经不会把已经存储的Cookie上转到MTS中，MTS也就无法把之传递到http://ibm.com/foo/上。

基于上面Cookie规范的介绍和例证，我们能看出，浏览器在接受某一个站点的Cookie的时候，需要检查Cookie的参数domain、path、secure，看是否与当前的站点和URL相符，如果不符的话，就会忽略。另一方面。浏览器在上传Cookie的时候，也会根据当前所访问站点的属性，上传相关的Cookie，而其他的Cookie则不予上传。

至此，我们讨论了需要修改Cookie的根本原因在于Cookie规范的限制。

&nbsp;

&nbsp;

&nbsp;

</div>