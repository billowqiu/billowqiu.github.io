---
title: CWnd及窗口产生和销毁过程
id: 192
categories:
  - VC
date: 2007-08-25 18:33:00
tags:
---

    <TABLE style="TABLE-LAYOUT: fixed">
<TBODY>
<TR>
<TD>
<DIV class=cnt id=blog_text>
<DIV>

昨天去新华书店看的今天趁还记得就写下了.

窗口创建：首先定义一个CWnd对象，然后调用CWnd::Create，其实Create又会调用CreateEx，与之对应的API函数也是这样，在CreateEx调用AfxCtxCreateWindowEx之前会调用PreCreateWindow，这个时候偶CWnd对应的窗口的句柄还未被Windows分配，之后调用AfxCtxCreateWindowEx，在调用这个函数的过程中会发送WM_CREATE消息从而引发OnCreate函数调用注意调用OnCreate时CWnd中的m_hWnd已经有值了，在OnCreate中可以进行一些初始化以及创建字窗口之类的事情。至此窗口创建完毕。

窗口销毁：可以发送一个WM_CLOSE消息给指定窗口使其调用OnClose函数，此函数中会发送WM_DESTROY从而引发OnDestroy的调用，该函数又会引发最后一个消息WM_NCDESTROY的发送，OnNcDestroy即被调用，在OnNcDestroy的最后会调用PostNcDestroy，CWnd的PostNcDestroy为

void CWnd::PostNcDestroy()
{
// default to nothing
}，什么也不做，但是CFrameWnd重载了此函数

void CFrameWnd::PostNcDestroy()
{
// default for frame windows is to allocate them on the heap
// the default post-cleanup is to 'delete this'.
// never explicitly call 'delete' on a CFrameWnd, use DestroyWindow instead
delete this;
}即我们看到的在App中new的Frame没有删除就是这个原因，但是我们自己从CWnd派生的窗口就不同了，如果你new了就要delete，而且最好就是在这个函数里delete
</DIV></DIV></TD></TR></TBODY></TABLE>
</div>