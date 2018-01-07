---
title: 删除CToolBar浮动时的菜单
id: 181
categories:
  - VC
date: 2008-06-12 04:06:00
tags:
---

    &nbsp; 你可以通过重写ON_WM_WINDOWPOSCHANGED消息处理函数来删除浮动状态下的工具栏实际是系统菜单。这个函数在CToolBar的大小，位置或者Z轴顺序改变时被调用。我们并没有处理WM_SIZE消息处理函数，因为这个函数仅仅只是在大小改变时被调用，而每次当工具栏浮动时并没有被调用。 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 我们可以通过CControlBar类的成员m_pDockBar成员而不是通过直接调用GetParent()来得到工具栏的父窗口。从CToolBar派生一个类，我暂且称之为CToolBarEx，当然你可以选择一个你认为适合的名字。我们仅仅是在工具栏浮动是删除实际是系统菜单,所以检查工具栏是否是正在浮动状态，并确保m_pDockBar是一个合法的指针。

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在你的新类中添加如下成员变量： BOOL m_bMenuRemoved;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 当我们删除系统菜单时将其设置为TRUE，确保仅仅在需要是删除系统菜单。
我们需要得到一个m_pDockBar指针来检查工具栏是否确实是在一个CDockFrameWnd类中，以便可以安全地删除CToolBar的系统菜单。下面是示例代码:

#include &lt;afxpriv.h&gt;

void CXToolBar::OnWindowPosChanged(WINDOWPOS FAR* lpwndpos)

{

&nbsp;&nbsp;&nbsp;&nbsp; CToolBar::OnWindowPosChanged(lpwndpos);

&nbsp;&nbsp;&nbsp;&nbsp; // should only be called once, when floated.

&nbsp;&nbsp;&nbsp;&nbsp; if( IsFloating() )

&nbsp;&nbsp;&nbsp;&nbsp; {

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if( m_pDockBar &amp;&amp; !m_bMenuRemoved )

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CWnd* pParent = m_pDockBar-&amp;gt;GetParent();

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if( pParent-&gt;IsKindOf(

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; RUNTIME_CLASS(CMiniFrameWnd)))

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pParent-&gt;ModifyStyle( WS_SYSMENU, 0, 0 );

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; m_bMenuRemoved = TRUE;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }

&nbsp;&nbsp;&nbsp;&nbsp; }

&nbsp;&nbsp;&nbsp;&nbsp; else if( m_bMenuRemoved ) {

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; m_bMenuRemoved = FALSE;

&nbsp;&nbsp;&nbsp;&nbsp; }

}
原文网址:[<U><FONT color=#0000ff>www.codejock.com/support/articles/mfc/general/g_8.asp</FONT></U>](http://www.codejock.com/support/articles/mfc/general/g_8.asp)

</div>