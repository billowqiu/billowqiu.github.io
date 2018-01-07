---
title: Neat Stuff Custom Draw
id: 156
categories:
  - VC
date: 2009-05-17 23:20:00
tags:
---

    

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 专门做界面已经有半年了，期间用得最多也就是Owner Draw，对于Cutom Draw只是知之一二，没有具体用过，其实Ower Draw用得最多也就是Button，ListBox两个，由于项目中有个同事把TreeCtrl进行Custom Draw了，效果很不错，这才意识到其强大及易用，相比较而言Ower Draw就复杂多了，今天再次把CP上那篇经典的关于Custom Draw文章看了一篇，用了半个小时写了Slider的Custom Draw，也算是为了练习使用WTL和GDI+：

<pre style="border: 1px dotted #785;background: #f5f5f5;">class CCDSliderCtrl :  public CWindowImpl&lt;CCDSliderCtrl,CTrackBarCtrl&gt;,
public CCustomDraw&lt;CCDSliderCtrl&gt;
{
public:
BEGIN_MSG_MAP(CCDSliderCtrl)    
CHAIN_MSG_MAP_ALT(CCustomDraw&lt; CCDSliderCtrl &gt;, 1)
DEFAULT_REFLECTION_HANDLER()
END_MSG_MAP()
public:
CCDSliderCtrl():m_pImageThumb(NULL),m_pImageChannel(NULL)
{

}
~CCDSliderCtrl()
{
if (NULL!=m_pImageThumb)
{
delete m_pImageThumb;
m_pImageThumb=NULL;
}

if (NULL!=m_pImageChannel)
{
delete m_pImageChannel;
m_pImageChannel=NULL;
}
}
void SetImage(LPCTSTR lpszThumb,LPCTSTR lpszChannel)
{
if (NULL!=m_pImageThumb)
{
delete m_pImageThumb;
m_pImageThumb=NULL;
}
m_pImageThumb=new Image((WCHAR*)lpszThumb);

if (NULL!=m_pImageChannel)
{
delete m_pImageChannel;
m_pImageChannel=NULL;
}
m_pImageChannel=new Image((WCHAR*)lpszChannel);
}
DWORD OnPrePaint( int idCtrl, LPNMCUSTOMDRAW lpNMCustomDraw )
{
return CDRF_NOTIFYITEMDRAW;
}
DWORD OnItemPrePaint( int idCtrl, LPNMCUSTOMDRAW lpNMCustomDraw )
{
Graphics graphics(lpNMCustomDraw-&gt;hdc);
CRect rcThumb(lpNMCustomDraw-&gt;rc);

switch ( lpNMCustomDraw-&gt;dwItemSpec )
{
case TBCD_CHANNEL:
{
CRect rcClient;
GetClientRect( &amp;rcClient);

ImageAttributes imAtt;
imAtt.SetColorKey(
Color(255, 0,255),
Color(255, 0,255),
ColorAdjustTypeBitmap);

graphics.DrawImage(
m_pImageChannel, 
Rect(0, 0, rcClient.Width(),rcClient.Height()),  // dest rect
0, 0, m_pImageChannel-&gt;GetWidth(),m_pImageChannel-&gt;GetHeight(),          // source rect
UnitPixel,
&amp;imAtt);

break;
}

case TBCD_TICS:
return CDRF_DODEFAULT;
case TBCD_THUMB:
{
graphics.DrawImage(m_pImageThumb,Rect(rcThumb.left,rcThumb.top,rcThumb.Width(),rcThumb.Height()));

break;
}

default:
ATLASSERT( FALSE );
};

return CDRF_SKIPDEFAULT;
}
protected:
Gdiplus::Image* m_pImageThumb;
Gdiplus::Image* m_pImageChannel;
};</pre> 

下面是效果图：

![](http://p.blog.csdn.net/images/p_blog_csdn_net/ToCpp/EntryImages/20090517/SliderCtrl.gif)

</div>