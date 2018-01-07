---
title: 在VC2005 Express中使用WTL
id: 178
categories:
  - VC
date: 2008-08-14 12:12:00
tags:
---

    

废话不多说，搞了一上午了，

安装VC2005Express，先下PSDK，注意安装的时候要记得安那个Web WorkShop SDK，否则编译的时候会先提示Shlwapi.h这个文件，其他需要注意的就是CodeProject上那个日本哥们写的[<U><FONT color=#0000ff>http://www.codeproject.com/KB/wtl/WTLExpress.aspx</FONT></U>](http://www.codeproject.com/KB/wtl/WTLExpress.aspx)，写的不错，只要安这个步骤，应该不会错的，主要是那个在VC2005Express下设置PSDK，它妈的真服了MS，VC2005Express默认都不让生成Win32工程，还要按照那个挂掉的链接说明设置，找了半天没找到那个链接的相关内容，搞得累了一上午，最后终于google到了，竟然还要改工程默认配置文件，终于搞定了，为了以后设置方便这里把那篇E文Copy了：

<FONT size=3>http://msdn.microsoft.com/vstudio/express/visualc/usingpsdk/
Using Visual C++ 2005 Express Edition with the Microsoft Platform SDK
By Brian Johnson, 
Microsoft Corporation
You can use Visual C++ Express to build powerful .NET Framework applications immediately after installation. In order to use Visual C++ Express to build Win32 applications, you'll need to take just a few more steps. I'll list the steps necessary for building Win32 applications using Visual C++ Express.
Step 1: Install Visual C++ Express.
If you haven't done so already, install Visual C++ Express.

Step 2: Install the Microsoft Platform SDK. 
Install the Platform SDK over the Web from the Download Center. Follow the instructions and install the SDK for the x86 platform.

Step 3: Update the Visual C++ directories in the Projects and Solutions section in the Options dialog box. 
Add the paths to the appropriate subsection:
Executable files: C:/Program Files/Microsoft Platform SDK for Windows Server 2003 R2/Bin
Include files: C:/Program Files/Microsoft Platform SDK for Windows Server 2003 R2/Include
Library files: C:/Program Files/Microsoft Platform SDK for Windows Server 2003 R2/Lib
Note: Alternatively, you can update the Visual C++ Directories by modifying the VCProjectEngine.dll.express.config file located in the /vc/vcpackages subdirectory of the Visual C++ Express install location. Please make sure that you also delete the file "vccomponents.dat" located in the "%USERPROFILE%/Local Settings/Application Data/Microsoft/VCExpress/8.0" if it exists before restarting Visual C++ Express Edition. 

Step 4: Update the corewin_express.vsprops file.
One more step is needed to make the Win32 template work in Visual C++ Express. You need to edit the corewin_express.vsprops file (found in C:/Program Files/Microsoft Visual Studio 8/VC/VCProjectDefaults) and
Change the string that reads:
AdditionalDependencies="kernel32.lib" to
AdditionalDependencies="kernel32.lib user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib"

Step 5: Generate and build a Win32 application to test your paths. 
In Visual C++ Express, the Win32 Windows Application type is disabled in the Win32 Application Wizard. To enable that type, you need to edit the file AppSettings.htm file located in the folder “%ProgramFiles%/Microsoft Visual Studio 8/VC/VCWizards/AppWiz/Generic/Application/html/1033/".
In a text editor comment out lines 441 - 444 by putting a // in front of them as shown here:
// WIN_APP.disabled = true;
// WIN_APP_LABEL.disabled = true; 
// DLL_APP.disabled = true; 
// DLL_APP_LABEL.disabled = true; 
Save and close the file and open Visual C++ Express. 
From the File menu, click New Project. In the New Project dialog box, expand the Visual C++ node in the Product Types tree and then click Win32\. Click on the Win32 Console Application template and then give your project a name and click OK. In the Win32 Application Wizard dialog box, make sure that Windows application is selected as the Application type and the ATL is not selected. Click the Finish button to generate the project. 
As a final step, test your project by clicking the Start button in the IDE or by pressing F5\. Your Win32 application should build and run. </FONT>

</div>