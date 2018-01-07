---
title: 两种获取本机IP地址的方法
id: 177
categories:
  - 系统编程
date: 2008-07-15 23:47:00
tags:
---

    

<FONT size=3>两种方法都是通过先得到主机名再获取IP地址：</FONT>

<FONT size=3>The </FONT><A>**<FONT size=3>gethostname</FONT>**</A><FONT size=3> function retrieves the standard host name for the local computer.</FONT>
<PRE class=syntax><FONT size=3>**int** **gethostname(**
  **char*** _[<U><FONT color=#0000ff>name</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT><FONT size=3>**,
**  **int** _[<U><FONT color=#0000ff>namelen</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT>**
**<FONT size=3>**);**
</FONT></PRE>
<DIV class=reftip style="VISIBILITY: hidden; OVERFLOW: visible; POSITION: absolute"></DIV>

#### Parameters

<DL>
<DT><FONT size=3>_name_ </FONT>
<DD><FONT size=3>[out] Pointer to a buffer that receives the local host name. </FONT>
<DT><FONT size=3>_namelen_ </FONT>
<DD><FONT size=3>[in] Length of the buffer, in bytes&nbsp;&nbsp;&nbsp;&nbsp;</FONT></DD></DL>

<FONT size=3>char hostname[50];</FONT>

<FONT size=3>gethostname（hostname,50);即可得到主机名。有了主机名下一步就是获取IP地址，方法一是通过API函数 gethostbyname，需要注意的事这个函数仅仅适用于IPv4，该函数再MSDN中的注释如下:</FONT>

<FONT size=3>The </FONT><A>**<FONT size=3>gethostbyname</FONT>**</A><FONT size=3> function retrieves host information corresponding to a host name from a host database. </FONT>

<FONT size=3>**Note**&nbsp;&nbsp; The </FONT><A>**<FONT size=3>gethostbyname</FONT>**</A><FONT size=3> function has been deprecated by the introduction of the </FONT>[**<FONT color=#0000ff size=3><U>getaddrinfo</U></FONT>**](ms-help://MS.MSDNQTR.v80.chs/MS.MSDN.v80/MS.WIN32COM.v10.en/winsock/winsock/getaddrinfo_2.htm)<FONT size=3> function. Developers creating Windows Sockets 2 applications are urged to use the </FONT><A>**<FONT size=3>getaddrinfo</FONT>**</A><FONT size=3> function instead of </FONT><A>**<FONT size=3>gethostbyname</FONT>**</A><FONT size=3>.</FONT>

<PRE class=syntax><FONT size=3>**struct hostent* FAR** **gethostbyname(**
  **const char*** _[<U><FONT color=#0000ff>name</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT>**
****<FONT size=3>);</FONT>**</PRE>

<FONT size=3>通过传递主机名得到的返回值是一个hosten结构体指针，</FONT>
<PRE class=syntax><FONT size=3>typedef struct hostent {
  char FAR* </FONT>[<FONT color=#0000ff size=3><U>h_name</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>;
  char FAR  FAR** </FONT>[<FONT color=#0000ff size=3><U>h_aliases</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>;
  short </FONT>[<FONT color=#0000ff size=3><U>h_addrtype</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>;
  short </FONT>[<FONT color=#0000ff size=3><U>h_length</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>;
  char FAR  FAR** </FONT>[<FONT color=#0000ff size=3><U>h_addr_list</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>;
} hostent;</FONT></PRE><PRE class=syntax><FONT size=3>我们需要使用其中的</FONT>[<FONT color=#0000ff size=3><U>h_addr_list</U></FONT>](http://hi.baidu.com/fc/editor/)<FONT size=3>，通过</FONT></PRE><PRE class=syntax><FONT size=3>LPCSTR pszIP=inet_ntoa (*(struct in_addr *)pHost-&gt;h_addr_list[0);即可得到IP地址，</FONT></PRE><PRE class=syntax><FONT size=3>注意h_addr_list是主机地址列表，我们这里取的事第一个值，有的电脑装有多个网卡可能有多个IP地址。</FONT></PRE><PRE class=syntax><FONT size=3>方法二是通过API函数getaddrinfo，通过上面的注释也可以看出ＭＳ推荐我们使用这个函数代替</FONT></PRE><PRE class=syntax><FONT size=3>gethostbyname，因为这个函数既适用于IPv4也适用于IPv6，详见MSDN。</FONT></PRE><PRE><FONT size=3>The **getaddrinfo** function provides protocol-independent translation from host name to address.</FONT></PRE><PRE class=syntax><FONT size=3>**int** **getaddrinfo(**
  **const TCHAR*** _[<U><FONT color=#0000ff>nodename</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT><FONT size=3>**,
**  **const TCHAR*** _[<U><FONT color=#0000ff>servname</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT><FONT size=3>**,
**  **const struct addrinfo*** _[<U><FONT color=#0000ff>hints</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT><FONT size=3>**,
**  **struct addrinfo**** _[<U><FONT color=#0000ff>res</FONT></U>](http://hi.baidu.com/fc/editor/)_</FONT>**
****<FONT size=3>);</FONT>**</PRE><PRE class=syntax><DIV class=reftip style="VISIBILITY: hidden; OVERFLOW: visible; POSITION: absolute"><FONT size=3> </FONT></DIV></PRE>

#### Parameters
<PRE class=syntax><DL><DT><FONT size=3>_nodename_ </FONT><DD><FONT size=3>[in] Pointer to a null-terminated string containing a host (node) name or a numeric host address string. The numeric host address string is a dotted-decimal IPv4 address or an IPv6 hex address. </FONT><DT><FONT size=3>_servname_ </FONT><DD><FONT size=3>[in] Pointer to a null-terminated string containing either a service name or port number. </FONT><DT><FONT size=3>_hints_ </FONT><DD><FONT size=3>[in] Pointer to an </FONT>[**<FONT color=#0000ff size=3><U>addrinfo</U></FONT>**](ms-help://MS.MSDNQTR.v80.chs/MS.MSDN.v80/MS.WIN32COM.v10.en/winsock/winsock/addrinfo_2.htm)<FONT size=3> structure that provides hints about the type of socket the caller supports. See Remarks. </FONT><DT><FONT size=3>_res_ </FONT><DD><FONT size=3>[out] Pointer to a linked list of one or more **addrinfo** structures containing response information about the host. </FONT></DD></DL></PRE>

<FONT size=3>MSDN说这个函数是用来提供一种与协议无关的主机名转换为地址的方法，参照MSDN上面的例子经过试验得出下面获取IP的方法：</FONT>

<FONT size=3>gethostname(hostname,100);&nbsp;&nbsp; //获得主机的名称
getaddrinfo(hostname,NULL,&amp;hints,&amp;res);&nbsp;&nbsp; //利用主机名称获取本地地址
char buff[100];
DWORD bufflen=100;
//将本地地址转换成字符串显示
struct sockaddr_in* pSockaddr=(sockaddr_in*)res-&gt;ai_addr;
char *pIP=inet_ntoa(pSockaddr-&gt;sin_addr);</FONT>

<FONT size=3>这里为什么说经过试验呢，MSDN上面的例子将getaddrinfo函数的前两个参数分别设置为主机IP和端口号，显然第一个参数不能设为主机IP了，根据其注释这个参数既可以为主机名也可以是IP，所以设为主机名，第二个参数说可以是一个服务名或者端口号，如果设为端口号，用WSAAddressToString函数获取的值中就包括该端口号，我也不太清楚这是什么原因，总之感觉这个Socket函数很诡异。</FONT>

</div>