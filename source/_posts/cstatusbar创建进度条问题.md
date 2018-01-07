---
title: CStatusBar创建进度条问题
id: 182
categories:
  - VC
date: 2008-07-06 01:46:00
tags:
---

    <DIV class=cnt id=blog_text>

<FONT color=#800080 size=3>想在状态栏中创建一个进度条，没有在框架窗口中定义进度条，再以状态栏为父窗口创建，而是直接从CStatusBar派生一个类，在其中定义了一个CPropressCtrl成员，按照常规的创子控件思路在OnCreate中创建进度条，</FONT>

<FONT color=#800080 size=3>int CQTStatusBar::OnCreate(LPCREATESTRUCT lpCreateStruct)
{
if (CStatusBar::OnCreate(lpCreateStruct) == -1)
&nbsp;&nbsp; return -1;</FONT>

<FONT color=#800080 size=3>// TODO: 在此添加您专用的创建代码
CRect rect;
GetClientRect(&amp;rect);
rect.left=rect.right-150;
if(!m_wndProress)
m_wndProress.Create(WS_CHILD | WS_VISIBLE,rect,this,111);
m_wndProress.MoveWindow(rect,false);&nbsp;&nbsp; //为了在大小改变时重载位置
m_wndProress.SetRange(0,100);
m_wndProress.SetPos(50);</FONT>

<FONT color=#800080 size=3>return 0;
}</FONT>

<FONT color=#800080 size=3>结果发现进度条并没有产生，通过调试发现这里的rect根本就没有取到值，换在WM_PAINT消息里面处理，结果正常。纳闷了，难道是OnCreate中状态栏还未创建好吗？应该不是的，估计是MFC设计的问题，CStatusBar的Create函数里面就没让设置大小的函数。</FONT>
</DIV>
</div>