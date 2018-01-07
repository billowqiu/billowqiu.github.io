---
title: Float ToolBar and dock ToolBar
id: 187
categories:
  - VC
date: 2007-09-01 18:37:00
tags:
---

    <TABLE style="TABLE-LAYOUT: fixed">
<TBODY>
<TR>
<TD>
<DIV class=cnt id=blog_text>
<DIV><FONT face=黑体 size=4>A docked toolbar is a child of the frame window it's docked to, but a floating toolbar is a child of the mini frame window that surrounds it. The mini frame window is a popup window owned by the frame window, but it's not a child of the frame window. (A popup window is a window with the style WS_POPUP; a child window has the WS_CHILD style instead.) The distinction is important because popup windows owned by a frame window are destroyed before the frame window is destroyed. Child windows, on the other hand, are destroyed _after_ their parents are destroyed. A floating toolbar no longer exists when the frame window's _OnDestroy_ function is called.</FONT></DIV></DIV></TD></TR></TBODY></TABLE>
</div>