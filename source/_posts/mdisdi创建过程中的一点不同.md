---
title: 'MDI,SDI创建过程中的一点不同'
id: 189
categories:
  - VC
date: 2007-08-31 18:35:00
tags:
---

    

CMultiDocTemplate* pDocTemplate;
pDocTemplate = new CMultiDocTemplate(
&nbsp;&nbsp; IDR_MDISQUTYPE,
&nbsp;&nbsp; RUNTIME_CLASS(CSquaresDoc),
&nbsp;&nbsp; RUNTIME_CLASS(CChildFrame), // custom MDI child frame
&nbsp;&nbsp; RUNTIME_CLASS(CSquaresView));
AddDocTemplate(pDocTemplate);

// create main MDI Frame window
CMainFrame* pMainFrame = new CMainFrame;
if (!pMainFrame-&gt;LoadFrame(IDR_MAINFRAME))
&nbsp;&nbsp; return FALSE;
m_pMainWnd = pMainFrame;

。。。。。。。。

&nbsp;&nbsp; // Dispatch commands specified on the command line
if (!ProcessShellCommand(cmdInfo))
&nbsp;&nbsp; return FALSE;

// The main window has been initialized, so show and update it.
pMainFrame-&gt;ShowWindow(m_nCmdShow);
pMainFrame-&gt;UpdateWindow();

上面为MDI，下面为SDI

CSingleDocTemplate* pDocTemplate;
pDocTemplate = new CSingleDocTemplate(
&nbsp;&nbsp; IDR_MAINFRAME,
&nbsp;&nbsp; RUNTIME_CLASS(CSquaresDoc),
&nbsp;&nbsp; RUNTIME_CLASS(CMainFrame),&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // main SDI frame window
&nbsp;&nbsp; RUNTIME_CLASS(CSquaresView));
AddDocTemplate(pDocTemplate);

。。。。。。。。。。

// Dispatch commands specified on the command line
if (!ProcessShellCommand(cmdInfo))
&nbsp;&nbsp; return FALSE;

// The main window has been initialized, so show and update it.
pMainFrame-&gt;ShowWindow(m_nCmdShow);
pMainFrame-&gt;UpdateWindow();

主要是在生产文档模板对象时RUNTIME_CLASS(CChildFrame), // custom MDI child frame
VS RUNTIME_CLASS(CMainFrame),&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // main SDI frame window
MDI传递的是childframe,而SDI传递的mainframe，因此MDI的ProcessShellCommand创建的是ChildFrame而SDI创建的MainFrame，所以MDI中就多出了

// create main MDI Frame window
CMainFrame* pMainFrame = new CMainFrame;
if (!pMainFrame-&gt;LoadFrame(IDR_MAINFRAME))
&nbsp;&nbsp; return FALSE;
m_pMainWnd = pMainFrame;

必须自己创建MainFrame.

</div>