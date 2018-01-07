---
title: Winsock 重复定义的错误
id: 115
categories:
  - VC
date: 2010-08-08 18:49:00
tags:
---

    

Winsock提示重复定义，

这是一个老问题了，之前也碰到过，最近一个项目中再次遇到，摘抄自MSDN：

The _Winsock2.h_ header file internally includes core elements from the _Windows.h_ header file, so there is not usually an #include line for the _Windows.h_ header file in Winsock applications. If an #include line is needed for the _Windows.h_ header file, this should be preceded with the #define WIN32_LEAN_AND_MEAN macro. For historical reasons, the _Windows.h_ header defaults to including the _Winsock.h_ header file for Windows Sockets 1.1\. The declarations in the _Winsock.h_ header file will conflict with the declarations in the _Winsock2.h_ header file required by Windows Sockets 2.0\. The WIN32_LEAN_AND_MEAN macro prevents the _Winsock.h_ from being included by the _Windows.h_ header. An example illustrating this is shown below. 

&nbsp;

&nbsp;

<div class="codeSnippet">
<pre>#ifndef WIN32_LEAN_AND_MEAN
#define WIN32_LEAN_AND_MEAN
#endif

#include &lt;windows.h&gt;
#include &lt;winsock2.h&gt;
#include &lt;ws2tcpip.h&gt;
#include &lt;iphlpapi.h&gt;
#include &lt;stdio.h&gt;
int main() {
  return 0;
}
大意就是因为MS由于历史原因，在Windows.h头文件中包含了Winsock.h，导致与Winsock2.h冲突，所以提示重复定义。</pre>
</div>
</div>