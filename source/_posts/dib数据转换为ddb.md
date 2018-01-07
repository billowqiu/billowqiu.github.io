---
title: DIB数据转换为DDB
id: 136
categories:
  - VC
date: 2009-10-12 15:55:00
tags:
---

    

前段时间做视频拍照时，需要将接受到的视频数据进行拍照，由于传输过来的是原始的DIB数据，因此需要将其转换为DDB，经过查找MSDN得出如下片段，比较实用，留个记号

<pre style="border: 1px dotted #785;background: #f5f5f5;">HDC hdc = ::GetDC(NULL);
HBITMAP hbmpFriend = NULL;

BITMAPINFO bmi; 
memset(&amp;bmi,sizeof(BITMAPINFO),0);
bmi.bmiHeader.biSize = sizeof(BITMAPINFOHEADER); 
bmi.bmiHeader.biWidth = nwidth; 
bmi.bmiHeader.biHeight = nheight; 
bmi.bmiHeader.biPlanes =1; 
bmi.bmiHeader.biBitCount = 24; 

// If the bitmap is not compressed, set the BI_RGB flag. 
bmi.bmiHeader.biCompression = BI_RGB; 

// Compute the number of bytes in the array of color 
// indices and store the result in biSizeImage. 
// For Windows NT, the width must be DWORD aligned unless 
// the bitmap is RLE compressed. This example shows this. 
// For Windows 95/98/Me, the width must be WORD aligned unless the 
// bitmap is RLE compressed.
bmi.bmiHeader.biSizeImage = ((bmi.bmiHeader.biWidth * 24 +31) &amp; ~31) /8
* bmi.bmiHeader.biHeight; 
// Set biClrImportant to 0, indicating that all of the 
// device colors are important. 
bmi.bmiHeader.biClrImportant = 0; 
bmi.bmiHeader.biXPelsPerMeter = 0;
bmi.bmiHeader.biYPelsPerMeter = 0;

hbmpFriend = ::CreateCompatibleBitmap(hdc,nwidth,nheight);

int nRetCopyLines = SetDIBits(hdc, (HBITMAP)hbmpFriend, 
0, nheight, (LPVOID)pRGB, (BITMAPINFO*)&amp;bmi, DIB_RGB_COLORS);

if (hbmpFriend &amp;&amp; nRetCopyLines)
{
//该干啥就干啥
}

::DeleteObject(hbmpFriend);
::ReleaseDC(NULL, hdc);</pre> 

</div>