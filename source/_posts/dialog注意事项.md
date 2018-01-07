---
title: Dialog注意事项
id: 163
categories:
  - VC
date: 2009-03-17 22:28:00
tags:
---

    

在CodeProject上面有两篇关于对话框的文章写得很不错，

[http://www.codeproject.com/KB/dialog/gettingmodeless.aspx](http://www.codeproject.com/KB/dialog/gettingmodeless.aspx)关于Modeless的，

[http://www.codeproject.com/KB/dialog/dlgboxtricks.aspx](http://www.codeproject.com/KB/dialog/dlgboxtricks.aspx)关于Dilaog的。

&nbsp;

<span style="color: #ff0000;">以下是今天自己总结的一点（仅在MFC上试验的）：</span>

<span style="color: #ff0000;">任何Window当单击标题栏上的X按钮时都会发生WM_CLOSE消息，从MSDN可以查到这个消息的默认实现是调用DestroyWindow，但是对于Modeless对话框貌似不是这样的，你必须自己调用DestroyWindow不知道这是不是就是MSDN说的Modeless必须自己调用DestroyWindow；</span>

&nbsp;

&nbsp;

<span style="color: #ff0000;">Model对话框的流程是这样的：</span>

<span style="color: #ff0000;">WM_CLOSE-&gt;OnCancel-&gt;OnDestroy其中多走了一道OnCancel，貌似Model对话框的WM_CLOSE也不直接调用DestroyWindow，而是在EndDialog间接调用。</span>

&nbsp;

<span style="color: #ff0000;">总结:对于Modeless在WM_CLOSE中自己调用DestroyWindow，否则会出问题的，而且尽量不要在里面有IDOK,IDCANCEL的按钮，即使有也要重载并调用DestroyWindow。</span>

&nbsp;

<span style="color: #339966;">以后有时间再把这个问题在SDK上跑一遍，MFC也真够烦的。</span>

</div>