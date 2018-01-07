---
title: 回调函数在MFC中的使用
id: 190
categories:
  - VC
date: 2007-08-26 18:34:00
tags:
---

    

<FONT size=3><SPAN>回调函数的简单定义就是你定义的由</SPAN><SPAN>Windows</SPAN><SPAN>来调用。以下两个函数摘自《</SPAN><SPAN>Programming Windows with MFC</SPAN><SPAN>》，这里暂且不管函数的具体作用，在</SPAN><SPAN>FillListBox</SPAN><SPAN>中有一个</SPAN><SPAN>API</SPAN><SPAN>函数，它调用的回调函数是</SPAN><SPAN>EnumFontFamProc</SPAN><SPAN>，回调函数的声明形式一般都是相对固定的，具体可以参考</SPAN><SPAN>MSDN</SPAN><SPAN>。</SPAN></FONT>

<SPAN><FONT size=3>static int CALLBACK EnumFontFamProc (ENUMLOGFONT* lpelf,</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-tab-count: 2">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </SPAN>NEWTEXTMETRIC* lpntm, int nFontType, LPARAM lParam);</FONT></SPAN>

<SPAN><FONT size=3>void CMainWindow::FillListBox ()</FONT></SPAN>

<SPAN><FONT size=3>{</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>m_wndListBox.ResetContent ();</FONT></SPAN>

<SPAN><FONT size=3></FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>CClientDC dc (this);</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>::EnumFontFamilies ((HDC) dc, NULL, (FONTENUMPROC) EnumFontFamProc,</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </SPAN>(LPARAM) this);</FONT></SPAN>

<SPAN><FONT size=3>}</FONT></SPAN>

<SPAN><FONT size=3>int CALLBACK CMainWindow::EnumFontFamProc (ENUMLOGFONT* lpelf,</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>NEWTEXTMETRIC* lpntm, int nFontType, LPARAM lParam)</FONT></SPAN>

<SPAN><FONT size=3>{</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>CMainWindow* pWnd = (CMainWindow*) lParam;</FONT></SPAN>

<SPAN><FONT size=3></FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>if ((pWnd-&gt;m_wndCheckBox.GetCheck () == BST_UNCHECKED) ||</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </SPAN>(nFontType &amp; TRUETYPE_FONTTYPE))</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </SPAN>pWnd-&gt;m_wndListBox.AddString (lpelf-&gt;elfLogFont.lfFaceName);</FONT></SPAN>

<SPAN><FONT size=3><SPAN style="mso-spacerun: yes">&nbsp;&nbsp;&nbsp; </SPAN>return 1;</FONT></SPAN>

<SPAN><FONT size=3>}</FONT></SPAN>

<FONT size=3><SPAN>请注意这里的函数</SPAN><SPAN>EnumFontFamilies</SPAN><SPAN>中的最后一个参数传递的是</SPAN><SPAN>this</SPAN><SPAN>即该</SPAN><SPAN>CMainWindow</SPAN><SPAN>对象的指针，为什么要这样呢，可以看到</SPAN><SPAN>EnumFontFamProc</SPAN><SPAN>的声明是</SPAN><SPAN>static</SPAN><SPAN>，在</SPAN><SPAN>C++</SPAN><SPAN>中</SPAN><SPAN>static</SPAN><SPAN>函数是不能调用非</SPAN><SPAN>static</SPAN><SPAN>成员的，所以这里传递一个</SPAN><SPAN>this</SPAN><SPAN>就不是很奇怪了。但是为什么要将该函数声明为</SPAN><SPAN>static</SPAN><SPAN>呢，这就要归咎于</SPAN><SPAN>C++</SPAN><SPAN>的特殊性了，众所周知</SPAN><SPAN>C++</SPAN><SPAN>编译器在编译的时候都会在对象中添加一个</SPAN><SPAN>this</SPAN><SPAN>指针，在成员函数调用中又会附加一个参数保存</SPAN><SPAN>this</SPAN><SPAN>指针，但是</SPAN><SPAN>Windows</SPAN><SPAN>的回调函数有严格的定义就是必须按照参数列表传递的参数，加了</SPAN><SPAN>this</SPAN><SPAN>指针后参数列表就会与</SPAN><SPAN>Windows</SPAN><SPAN>期望的参数列表不一致了，因此这里将其声明为</SPAN><SPAN>static</SPAN><SPAN>（</SPAN><SPAN>static</SPAN><SPAN>成员函数不会传递</SPAN><SPAN>this</SPAN><SPAN>指针，这点说起来总是知道，但是真正用时总是忘了，唉）。</SPAN></FONT>

<FONT size=3><SPAN><SPAN style="mso-tab-count: 1">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </SPAN></SPAN><SPAN>另外在</SPAN><SPAN>Windows</SPAN><SPAN>中使用</SPAN><SPAN>callback</SPAN><SPAN>函数很常见，恰好许多支持回调函数的</SPAN><SPAN>API</SPAN><SPAN>函数都像这里的</SPAN><SPAN>EnumFontFamilies</SPAN><SPAN>一样支持自定义的</SPAN><SPAN>LPARAM</SPAN><SPAN>参数，刚好可以传递</SPAN><SPAN>this</SPAN><SPAN>，如果使用的</SPAN><SPAN>API</SPAN><SPAN>函数不支持这样的自定义的</SPAN><SPAN>LPARAM</SPAN><SPAN>参数，就需要其他的方法了，一种比较简单的方法是将</SPAN><SPAN>this</SPAN><SPAN>复制为</SPAN><SPAN>global</SPAN><SPAN>变量使得回调函数可以使用。</SPAN></FONT>

</div>