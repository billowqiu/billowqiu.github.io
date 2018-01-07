---
title: Flicker free drawing of any control
id: 164
categories:
  - VC
date: 2009-02-22 21:56:00
tags:
---

    

<span class="bodycopy">&nbsp;
<p>There are two articles dealing with this subject on this site, but i guess sometimes it's needed to free one single control from flickering, and it's not needed to use a complete new window-class. I will show you a really small solution of the flickering-problem, which uses the CMemDC-class from the article by Keith Rule. To make a control flicker free you have to go three steps, as described now. 

#### 1\. Override the OnEraseBkgnd()-routine of the control 

Flickering apears, because the complete control is erased and it needs some time to fill the control-area with some text/graphics or whatever. There is a really short moment, when nothing can bee seen, because the background was erased but nothing is written yet. That's why we don't erase the background of the control anymore. to do this, we have to override the OnEraseBkgnd()-routine of the control like this: 

<pre>   BOOL CRecordList::OnEraseBkgnd(CDC* pDC)
   {
      <span style="color: #0000ff;"><span class="codeKeyword">return</span></span> FALSE;
   }
</pre>

#### 2\. Override the OnPaint()-routine of the control 

&nbsp;

The second thing to do, is to override the OnPaint()-routine. The trick is, to paint the complete control into a memory device-context and copy it in the original DC via BitBlt(). The CMemDC-class does this work for you. Because we don't erase the background anymore (see above), we need to erase the memory-device-context with the background-color of the control. A typical OnPaint() should look like this:

&nbsp;

<pre>   <span style="color: #0000ff;"><span class="codeKeyword">void</span></span> CRecordList::OnPaint()
   {
      CPaintDC dc(<span style="color: #0000ff;"><span class="codeKeyword">this</span></span>);
      CMemDC memDC(&amp;dc);

      CRect clip;
      memDC.GetClipBox(&amp;clip);
      memDC.FillSolidRect(clip, GetSysColor(COLOR_WINDOW));

      DefWindowProc(WM_PAINT, (WPARAM)memDC-&gt;m_hDC, (LPARAM)0);
   }
</pre>

#### 3\. Override the EraseBkgnd()-routine of the control's parent

</span></p>

<a name="more"> </a>

Imagine the following situation. You have a dialog with the flicker free control in it. And now the user resizes the window. What happens? First of all, the background of the dialog is erased and after that all controls of the window are redrawn. But when the background is erased and THAN the controls are redrawn, we still have flickering! That why we have to exclude the controls area out of the clipping-box of the control's parent. And here's how we do it: 

<pre>   BOOL CListBar::OnEraseBkgnd(CDC *pDC)
   {
      CRect clip;
      m_Control-&gt;GetWindowRect(&amp;clip);<span style="color: #009900;">// get rect of the control</span>

      ScreenToClient(&amp;clip);
      pDC-&gt;ExcludeClipRect(&amp;clip);

      pDC-&gt;GetClipBox(&amp;clip);
      pDC-&gt;FillSolidRect(clip, GetSysColor(COLOR_BTNFACE));

      <span style="color: #0000ff;"><span class="codeKeyword">return</span></span> FALSE;
   }

That's it. You can make near every control flicker free in this way, but for some of the controls, you have to change the routines above slightly. Like for the ListCtrl in Report-mode, because the columne-header-area have to be excluded of the clip-box, like this: 
<pre>   <span style="color: #0000ff;"><span class="codeKeyword">void</span></span> CRecordList::OnPaint()
   {
      CPaintDC dc(<span style="color: #0000ff;"><span class="codeKeyword">this</span></span>);

      CRect headerRect;
      GetDlgItem(0)-&gt;GetWindowRect(&amp;headerRect);
      ScreenToClient(&amp;headerRect);
      dc.ExcludeClipRect(&amp;headerRect);

      CMemDC memDC(&amp;dc);
      .
      .
      .
   }
</pre>

&nbsp;

&nbsp;

</pre>
</div>