---
title: Edit控件改变背景色
id: 168
categories:
  - VC
date: 2009-01-27 15:32:00
tags:
---

    

<span style="background-color: #ff0000;">WM_CTLCOLOREDIT比WM_ERASEBKGND更方便，光响应WM_ERASEBKGND貌似不起作用！</span>

&nbsp;

<span style="background-color: #ff0000;">To change the background color of a single-line edit control, set the brush handle in both the **CTLCOLOR_EDIT** and **CTLCOLOR_MSGBOX** message codes, and call the CDC::SetBkColor function in response to the **CTLCOLOR_EDIT** code.</span>

</div>