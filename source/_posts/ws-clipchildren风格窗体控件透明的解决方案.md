---
title: WS_CLIPCHILDREN风格窗体控件透明的解决方案
id: 161
categories:
  - VC
date: 2009-04-18 20:34:00
tags:
---

    

<span style="font-size: medium;">在Windows下窗体都是矩形的，但是有时在做界面时尤其是需要贴图的界面中比如QQ2008(09另当别论)，很多时候控件都是&ldquo;非矩形&rdquo;的，主要是因为图片不是矩形的，可能需要边角地方透明比如</span>[<span style="font-size: medium;">http://blog.csdn.net/ToCpp/archive/2009/01/22/3849541.aspx</span>](http://blog.csdn.net/ToCpp/archive/2009/01/22/3849541.aspx)<span style="font-size: medium;">中按钮的边角就不是矩形的，这时如果窗体的样式没有WS_CLIPCHILDREN倒很好弄，如果有这个样式就麻烦了，因为这个样式会使父窗体不绘制子窗体的区域。说到这里其实也就自然而然有一种解决方案了:就是在绘制控件前先绘制父窗体的背景，刚刚发现一个相当简单的实现方法，DrawThemeParentBackground，这个函数刚好实现需求，不过前提是XP以上系统才能用。当然通过自己获取父窗体背景再绘制也是可以的，具体方案网上就很多了。</span>

</div>