---
title: SetViewportOrgEx---GDI学习
id: 171
categories:
  - VC
date: 2009-01-09 15:28:00
tags:
---

    

The **SetViewportOrgEx** function specifies which device point maps to the window origin (0,0). 

总是对这个系列的函数不太清楚，ViewPort(Device Point)，WindowPort(Logic Point)，这个函数将参数中指定的x,y(设备坐标)，映射到Window Origin(0,0)，也就是你实用GDI时其坐标原点现在在(x,y)而不是UpLeft。

<pre class="syntax">**BOOL SetViewportOrgEx(
  HDC**_ <a class="synParam" onclick="showTip(this)"><span style="text-decoration: underline;"><span style="color: #0000ff;">hdc</span></span></a>_**,        **// handle to device context
**  int**_ <a class="synParam" onclick="showTip(this)"><span style="text-decoration: underline;"><span style="color: #0000ff;">X</span></span></a>_**,          **// new x-coordinate of viewport origin
**  int**_ <a class="synParam" onclick="showTip(this)"><span style="text-decoration: underline;"><span style="color: #0000ff;">Y</span></span></a>_**,          **// new y-coordinate of viewport origin
**  LPPOINT**_ <a class="synParam" onclick="showTip(this)"><span style="text-decoration: underline;"><span style="color: #0000ff;">lpPoint</span></span></a>_ // original viewport origin
**);**</pre>

#### Parameters

<dl><dt>_hdc_ </dt><dd>[in] Handle to the device context. </dd><dt>_X_ </dt><dd>[in] Specifies the x-coordinate, in device units, of the new viewport origin. </dd><dt>_Y_ </dt><dd>[in] Specifies the y-coordinate, in device units, of the new viewport origin. </dd><dt>_lpPoint_ </dt><dd>[out] Pointer to a [**<span style="text-decoration: underline;"><span style="color: #0000ff;">POINT</span></span>**](ms-help://MS.MSDNQTR.v90.chs/gdi/rectangl_0tiq.htm) structure that receives the previous viewport origin, in device coordinates. If _lpPoint_ is NULL, this parameter is not used. </dd></dl>

This function (along with **SetViewportExtEx** and **SetWindowExtEx**) helps define the mapping from the logical coordinate space (also known as a _window_) to the device coordinate space (the _viewport_). 

这个函数与**SetViewportExtEx** and **SetWindowExtEx**一起使用使得逻辑坐标映射到相应的设备坐标**SetViewportOrgEx** specifies which device point maps to the logical point (0,0). It has the effect of shifting the axes so that the logical point (0,0) no longer refers to the upper-left corner. 

<pre>//map the logical point (0,0) to the device point (xViewOrg, yViewOrg)
SetViewportOrgEx ( hdc, xViewOrg, yViewOrg, NULL)
</pre>

This is related to the **SetWindowOrgEx** function. Generally, you will use one function or the other, but not both. Regardless of your use of **SetWindowOrgEx** and **SetViewportOrgEx**, the device point (0,0) is always the upper-left corner

**SetWindowOrgEx**可以达到同样的作用，但是通常情况下只应该使用其中的一个(记住)，注意：不管是使用**SetWindowOrgEx**还是**SetViewportOrgEx，**设备点(0,0)始终是在左上角的。

&nbsp;

为了以后不在这里纠缠不清，以后都只用这个函数了，<span style="color: #ff6600;">现在GDI中的(0,0)现在是(x,y)了</span>。

</div>