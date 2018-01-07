---
title: 最简单的MFC程序
id: 194
categories:
  - VC
date: 2007-01-21 18:31:00
tags:
---

    <TABLE style="TABLE-LAYOUT: fixed">
<TBODY>
<TR>
<TD>
<DIV class=cnt id=blog_text>
<DIV>

#include &lt;afxwin.h&gt;

//定义一个CWinApp的派生类
class CMinApp:public CWinApp
{
public:
virtual BOOL InitInstance();
};
//重载CWinApp成员函数InitInstance()
BOOL CMinApp::InitInstance()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //应用程序初始化
{
CFrameWnd* pFrame=new CFrameWnd;&nbsp;&nbsp;&nbsp;&nbsp; //动态生成主窗口类对象
pFrame-&gt;Create(0,_T("A Minimal MFC Program")); //创建主窗口
pFrame-&gt;ShowWindow(SW_SHOWMAXIMIZED); //显示主窗口
pFrame-&gt;UpdateWindow();&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //刷新主窗口
AfxGetApp()-&gt;m_pMainWnd=pFrame;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //指定应用程序主窗口
return TRUE;
}

//定义一个全局CMinApp对象
CMinApp HelloApp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //生成应用程序对象并执行应用程序

在程序的一开始由CWinApp派生的一个自己的类CMinApp。这个类利用CWinApp中的虚拟初始化函数，改写成自己的初始化函数。在这个初始化函数中动态地建立了CFrameWnd类实例，用来定义窗口和显示窗口。而CWinApp自身去完成建立消息循环的工作，在这个小例子中没有处理消息。上述工作实际上还停留在定义上，程序的运行只有最后一句话，派生类CMinApp的实例化。

在MFC变成结构中CWinApp相当于WinMain()函数，其中的InitInstance函数是MFC应用程序的入口点，且它是可以重载的，可以应用它编写自己应用程序的初始化。InitInstance函数完成以下工作：

1：定义框架窗口对CFrameWnd作为应用程序的主窗口

2：显示主窗口

3：建立局部窗口与应用程序对象的关系

声明应用程序对象时，应用程序自动运行，因为类的构造器调用了run()函数，在run()中建立消息循环

CFrameWnd类负责窗口的创建和显示

从上面的例子中可以看出MFC程序至少要有两个对象，应用对象和主框架对象，在MFC应用程序中需要定义一个单独的全局应用程序对象。类CFrameWnd的对象代表中应用程序的主框架窗口。主框架窗口的创建和显示工作由CMinApp的成员函数InitInstance完成，在应用程序中必须重载该函数，因为CWinApp基类根本无法知道具体应用程序需要什么样的主框架窗口。应用程序运行开始时，Windows会自动调用应用程序框架内部的WinMain函数（在MFC应用程序中仍然存在WinMain函数作为程序的入口，只不过它被隐藏在应用程序内部了）WinMain函数会去查找应用程序的全局构造对象，然后调用InitInstance函数以进行必要的初始化，接着调用隐藏在CWinApp基类中的函数Run(),应用程序进入运行状态。用户可以通过关闭主窗口来终止应用程序，这一操作会引起一系列事件的发生，首先CFrameWnd对象将被删除，然后退出WinMain,最后删除CMinApp对象。
</DIV></DIV></TD></TR></TBODY></TABLE>
</div>