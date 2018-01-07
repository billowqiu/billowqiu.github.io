---
title: WM_NCHITTEST消息
id: 179
categories:
  - VC
date: 2008-07-24 01:10:00
tags:
---

    

以SDK为例：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; case WM_LBUTTONDOWN :
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pt.x = LOWORD(lParam);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pt.y = HIWORD(lParam);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; wsprintf(mess,"pt.x=%d,pt.y=%d",pt.x,pt.y);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; MessageBox(hwnd,mess,"调试",MB_OK);

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; /*ScreenToClient(hwnd,&amp;pt);*/
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (PtInRect(&amp;rcClose, pt))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; MessageBox(hwnd,"点击了关闭按钮","调试",MB_OK);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SendMessage(hwnd,WM_SYSCOMMAND,(WPARAM)SC_CLOSE,(LPARAM)MAKELPARAM(pt.x, pt.y));
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (PtInRect(&amp;rcMin, pt))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SendMessage(hwnd,WM_SYSCOMMAND,(WPARAM)SC_MINIMIZE,(LPARAM)MAKELPARAM(pt.x, pt.y));
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; break;

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; case WM_NCHITTEST:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pt.x = LOWORD(lParam);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; pt.y = HIWORD(lParam);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ScreenToClient(hwnd,&amp;pt);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if(!PtInRect(&amp;rcClose,pt) &amp;&amp; !PtInRect(&amp;rcMin,pt))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return HTCAPTION;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; else
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return HTCLIENT;

当你在消息函数中截获此消息时，你可以选择直接返回相应的值比如HTCAPTION给OS，这时经过我的测试发现OS就不会给你发送WM_LBUTTONDOWN消息了，而如上所示我需要响应WM_LBUTTONDOWN怎么办呢？这时可以通过判断相应点是否在某个区域内返回相应的值，经过测试可以运行。

</div>