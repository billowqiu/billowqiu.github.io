---
title: WM_NCHITTEST的几点心得
id: 148
categories:
  - VC
date: 2009-06-29 01:43:00
tags:
---

    

**1：WM_NCHITTEST是作为Non-Client测试使用的，**

**2：当此消息的返回值为MSDN上面所述的<span style="color: #0000ff;"><span style="text-decoration: underline;">DefWindowProc</span></span>**<span style="color: #000000;">**所规定的相应值时，系统才会进一步产生相应的客户区或者非客户区鼠标消息，而且在你使用OnNcCalcSize时自定义的非客户区内，系统是不会产生任何鼠标消息的，除非你返回一个值以表示当前鼠标在哪个区域，毕竟对于你的区域系统也不知道该返回什么值**</span>

**3：除了返回<span style="color: #0000ff;"><span style="text-decoration: underline;">HTCLIENT</span><span style="color: #000000;"><span style="text-decoration: underline;">，</span>其他任何值系统都会产生相应的NC鼠标消息</span></span>**

**<span style="color: #0000ff;"></span>**

**<span style="color: #0000ff;"><span style="color: #000000;">这个消息困惑好久了，今天测试了下才发现是这样的，真是恍然大悟，明天做个例子测试下。</span></span>**

</div>