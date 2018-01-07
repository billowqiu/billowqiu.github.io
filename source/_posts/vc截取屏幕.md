---
title: VC截取屏幕
id: 191
categories:
  - VC
date: 2007-08-18 18:33:00
tags:
---

    一直对GID 不是很懂，今天狠下心来看了一天的GDI，主要是参照着MSDN看，感觉还是有点收获的。根据MSDN写了一个小小的程序，仅仅就是截取屏幕，另外可以将其保存到剪贴板里。 

以下为源代码：

// Capturing_Image.cpp : 定义应用程序的入口点。
//

#include "stdafx.h"
#include "Capturing_Image.h"

#define MAX_LOADSTRING 100

// 全局变量:
HINSTANCE hInst;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // 当前实例
TCHAR szTitle[MAX_LOADSTRING];&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // 标题栏文本
TCHAR szWindowClass[MAX_LOADSTRING];&nbsp;&nbsp;&nbsp; // 主窗口类名

// 此代码模块中包含的函数的前向声明:
ATOM&nbsp;&nbsp;&nbsp;&nbsp; MyRegisterClass(HINSTANCE hInstance);
BOOL&nbsp;&nbsp;&nbsp;&nbsp; InitInstance(HINSTANCE, int);
LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);
INT_PTR CALLBACK About(HWND, UINT, WPARAM, LPARAM);

void CaptureScreen(HWND,HDC);

int APIENTRY _tWinMain(HINSTANCE hInstance,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; HINSTANCE hPrevInstance,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; LPTSTR&nbsp;&nbsp;&nbsp; lpCmdLine,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; int&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; nCmdShow)
{
UNREFERENCED_PARAMETER(hPrevInstance);
UNREFERENCED_PARAMETER(lpCmdLine);

&nbsp;&nbsp; // TODO: 在此放置代码。
MSG msg;
HACCEL hAccelTable;

// 初始化全局字符串
LoadString(hInstance, IDS_APP_TITLE, szTitle, MAX_LOADSTRING);
LoadString(hInstance, IDC_CAPTURING_IMAGE, szWindowClass, MAX_LOADSTRING);
MyRegisterClass(hInstance);

// 执行应用程序初始化:
if (!InitInstance (hInstance, nCmdShow))
{
&nbsp;&nbsp; return FALSE;
}

hAccelTable = LoadAccelerators(hInstance, MAKEINTRESOURCE(IDC_CAPTURING_IMAGE));

// 主消息循环:
while (GetMessage(&amp;msg, NULL, 0, 0))
{
&nbsp;&nbsp; if (!TranslateAccelerator(msg.hwnd, hAccelTable, &amp;msg))
&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp; TranslateMessage(&amp;msg);
&nbsp;&nbsp;&nbsp; DispatchMessage(&amp;msg);
&nbsp;&nbsp; }
}

return (int) msg.wParam;
}

//
// 函数: MyRegisterClass()
//
// 目的: 注册窗口类。
//
// 注释:
//
//&nbsp;&nbsp;&nbsp; 仅当希望
//&nbsp;&nbsp;&nbsp; 此代码与添加到 Windows 95 中的“RegisterClassEx”
//&nbsp;&nbsp;&nbsp; 函数之前的 Win32 系统兼容时，才需要此函数及其用法。调用此函数十分重要，
//&nbsp;&nbsp;&nbsp; 这样应用程序就可以获得关联的
//&nbsp;&nbsp;&nbsp; “格式正确的”小图标。
//
ATOM MyRegisterClass(HINSTANCE hInstance)
{
WNDCLASSEX wcex;

wcex.cbSize = sizeof(WNDCLASSEX);

wcex.style&nbsp;&nbsp;&nbsp; = CS_HREDRAW | CS_VREDRAW|CS_OWNDC;
wcex.lpfnWndProc = WndProc;
wcex.cbClsExtra&nbsp;&nbsp; = 0;
wcex.cbWndExtra&nbsp;&nbsp; = 0;
wcex.hInstance&nbsp;&nbsp; = hInstance;
wcex.hIcon&nbsp;&nbsp;&nbsp; = LoadIcon(hInstance, MAKEINTRESOURCE(IDI_CAPTURING_IMAGE));
wcex.hCursor&nbsp;&nbsp; = LoadCursor(NULL, IDC_ARROW);
wcex.hbrBackground = (HBRUSH)(COLOR_WINDOW+1);
wcex.lpszMenuName = MAKEINTRESOURCE(IDC_CAPTURING_IMAGE);
wcex.lpszClassName = szWindowClass;
wcex.hIconSm&nbsp;&nbsp; = LoadIcon(wcex.hInstance, MAKEINTRESOURCE(IDI_SMALL));

return RegisterClassEx(&amp;wcex);
}

//
//&nbsp;&nbsp; 函数: InitInstance(HINSTANCE, int)
//
//&nbsp;&nbsp; 目的: 保存实例句柄并创建主窗口
//
//&nbsp;&nbsp; 注释:
//
//&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 在此函数中，我们在全局变量中保存实例句柄并
//&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 创建和显示主程序窗口。
//
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
&nbsp;&nbsp; HWND hWnd;

&nbsp;&nbsp; hInst = hInstance; // 将实例句柄存储在全局变量中

&nbsp;&nbsp; hWnd = CreateWindow(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, NULL, NULL, hInstance, NULL);

&nbsp;&nbsp; if (!hWnd)
&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return FALSE;
&nbsp;&nbsp; }

&nbsp;&nbsp; ShowWindow(hWnd, nCmdShow);
&nbsp;&nbsp; UpdateWindow(hWnd);

&nbsp;&nbsp; return TRUE;
}

//
// 函数: WndProc(HWND, UINT, WPARAM, LPARAM)
//
// 目的: 处理主窗口的消息。
//
// WM_COMMAND - 处理应用程序菜单
// WM_PAINT - 绘制主窗口
// WM_DESTROY - 发送退出消息并返回
//
//
LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
int wmId, wmEvent;
PAINTSTRUCT ps;
HDC hdc=NULL;
switch (message)
{
case WM_COMMAND:
&nbsp;&nbsp; wmId&nbsp;&nbsp;&nbsp; = LOWORD(wParam);
&nbsp;&nbsp; wmEvent = HIWORD(wParam);
&nbsp;&nbsp; // 分析菜单选择:
&nbsp;&nbsp; switch (wmId)
&nbsp;&nbsp; {
&nbsp;&nbsp; case IDM_CAPTUR:
&nbsp;&nbsp;&nbsp;&nbsp; CaptureScreen(hWnd,hdc);
&nbsp;&nbsp;&nbsp;&nbsp; break;
&nbsp;&nbsp; case IDM_ABOUT:
&nbsp;&nbsp;&nbsp; DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
&nbsp;&nbsp;&nbsp; break;
&nbsp;&nbsp; case IDM_EXIT:
&nbsp;&nbsp;&nbsp; DestroyWindow(hWnd);
&nbsp;&nbsp;&nbsp; break;
&nbsp;&nbsp; default:
&nbsp;&nbsp;&nbsp; return DefWindowProc(hWnd, message, wParam, lParam);
&nbsp;&nbsp; }
&nbsp;&nbsp; break;
case WM_PAINT:
&nbsp;&nbsp; hdc = BeginPaint(hWnd, &amp;ps);
&nbsp;&nbsp; // TODO: 在此添加任意绘图代码...
&nbsp;&nbsp; CaptureScreen(hWnd,hdc);
&nbsp;&nbsp; EndPaint(hWnd, &amp;ps);
&nbsp;&nbsp; break;
case WM_DESTROY:
&nbsp;&nbsp; PostQuitMessage(0);
&nbsp;&nbsp; break;
default:
&nbsp;&nbsp; return DefWindowProc(hWnd, message, wParam, lParam);
}
return 0;
}

// “关于”框的消息处理程序。
INT_PTR CALLBACK About(HWND hDlg, UINT message, WPARAM wParam, LPARAM lParam)
{
UNREFERENCED_PARAMETER(lParam);
switch (message)
{
case WM_INITDIALOG:
&nbsp;&nbsp; return (INT_PTR)TRUE;

case WM_COMMAND:
&nbsp;&nbsp; if (LOWORD(wParam) == IDOK || LOWORD(wParam) == IDCANCEL)
&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp; EndDialog(hDlg, LOWORD(wParam));
&nbsp;&nbsp;&nbsp; return (INT_PTR)TRUE;
&nbsp;&nbsp; }
&nbsp;&nbsp; break;
}
return (INT_PTR)FALSE;
}

void CaptureScreen(HWND hWnd,HDC hdc)
{
hdc=GetDC(hWnd);
RECT clientRect;
GetClientRect(hWnd,&amp;clientRect);

HDC hdcScreen;
HDC hdcCompatible;
HBITMAP hbmScreen;
// 创建一个普通的包含整个显示器屏幕的设备描述表和一个与之兼容
// 的内存设备描述表，普通的设备描述表提供一个屏幕内容的快照
// 内存设备描述表将该快照的一份拷贝与一个bitmap相连，但是该bitmap
//的表面显示的是一个单色的一个像素的bitmap，因此应该选入一个何时的
//bitmap到该内存设备描述表
//以下摘自MSDN
//A memory DC exists only in memory. When the memory DC is created,its display 
//surface is exactly one monochrome pixel wide and one monochrome pixel high. 
//Before an application can use a memory DC for drawing operations, it must select a 
//bitmap of the correct width and height into the DC.

hdcScreen = CreateDC("DISPLAY", NULL, NULL, NULL); 
hdcCompatible = CreateCompatibleDC(hdcScreen);

// 创建一个与屏幕设备描述表相兼容的bitmap，然后将其选入先前创建的
// 内存设备描述表
&nbsp;&nbsp;&nbsp;&nbsp;
hbmScreen = CreateCompatibleBitmap(hdcScreen, 
&nbsp;&nbsp;&nbsp; GetDeviceCaps(hdcScreen, HORZRES), 
&nbsp;&nbsp;&nbsp; GetDeviceCaps(hdcScreen, VERTRES)); 

if (hbmScreen == 0) 
MessageBox(hWnd,"hbmScreen","错误",MB_OK); 

if (!SelectObject(hdcCompatible, hbmScreen)) 
MessageBox(hWnd,"Compatible Bitmap Selection", "错误",MB_OK); 

//将与屏幕设备描述表相关的颜色信息拷贝到内存设备描述表相关
if (!BitBlt(hdcCompatible, 
0,0, 
GetDeviceCaps(hdcScreen, HORZRES),GetDeviceCaps(hdcScreen, VERTRES), 
hdcScreen, 
0,0, 
SRCCOPY)) 
MessageBox(hWnd,"Screen to Compat Blt Failed","错误",MB_OK);

StretchBlt(hdc, 
&nbsp;&nbsp;&nbsp; 0, 0, 
&nbsp;&nbsp;&nbsp; clientRect.right-clientRect.left, clientRect.bottom-clientRect.top, 
&nbsp;&nbsp;&nbsp; hdcCompatible, 
&nbsp;&nbsp;&nbsp; 0, 0, 
&nbsp;&nbsp;&nbsp; GetDeviceCaps(hdcScreen, HORZRES), GetDeviceCaps(hdcScreen, VERTRES),
&nbsp;&nbsp;&nbsp; SRCCOPY); 

if (OpenClipboard(hWnd)) 
&nbsp;&nbsp;&nbsp;&nbsp; //hWnd为程序窗口句柄
&nbsp;&nbsp;&nbsp; {
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //清空剪贴板
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; EmptyClipboard();
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //把屏幕内容粘贴到剪贴板上,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; SetClipboardData(CF_BITMAP, hbmScreen);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //关闭剪贴板
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; CloseClipboard();
&nbsp;&nbsp;&nbsp;&nbsp; }

DeleteObject(hbmScreen);
DeleteDC(hdcCompatible);
DeleteDC(hdcScreen);
ReleaseDC(hWnd,hdc);
}

附上一个截图：

![](http://blog.programfan.com/upfile/200708/20070818012327.gif)

</div>