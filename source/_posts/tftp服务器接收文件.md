---
title: TFTP服务器接收文件
id: 127
categories:
  - 系统编程
date: 2009-12-10 15:12:00
tags:
---

    

最近看到很多UDP可靠文件传输的文章，其中有提到TFTP协议，在CodeProject上面搜到一个客户端的，觉得很不错，[http://www.codeproject.com/KB/IP/tftp_client.aspx](http://www.codeproject.com/KB/IP/tftp_client.aspx)

这里模仿者写了个服务器端，只实现了服务器端的接受文件，配合着基本上就可以作为一般的UDP文件接发例子了，大致代码如下

头文件：

<pre style="border: 1px dotted #785;background: #f5f5f5;">#pragma comment(lib,"ws2_32.lib")

#define TFTP_DEFAULT_PORT    69

#define TFTP_OPCODE_READ     1
#define TFTP_OPCODE_WRITE    2
#define TFTP_OPCODE_DATA     3
#define TFTP_OPCODE_ACK      4
#define TFTP_OPCODE_ERROR    5

#define TFTP_DEFAULT_MODE    "octet"

#define TFTP_DATA_PKT_SIZE   512

#define TFTP_ERROR_MSG_MAX1024
#define TFTP_PACKET_DATA_MAX1024

#define TFTP_RETRY     3

#define TFTP_RESULT_ERROR1
#define TFTP_RESULT_CONTINUE    2
#define TFTP_RESULT_DONE3

#define TFTP_STATUS_NOTHING1
#define TFTP_STATUS_GET2
#define TFTP_STATUS_PUT3
#define TFTP_STATUS_ERROR4

#define TFTP_TIMEOUT5000 //ms,5&Atilde;&euml;

#include &lt;fstream&gt;

class CTFTPServer  
{
protected:
class CPacket
{
public:
CPacket();
~CPacket();
bool AddByte(BYTE c);
bool AddWord(WORD c);
bool AddString(char* str);
bool AddMem(char* mem,int len);
unsigned char* GetData(int pos=0);
WORD GetWord(int pos);
BYTE GetByte(int pos);
bool GetMem(int pos,char* mem,int len);
int  GetSize();
void SetSize(int N);
void Clear();
bool IsValid();
bool IsNull();
bool IsACK(int blocknum);
bool IsDATA(int blocknum);
bool IsError();
bool GetError(char* buf,unsigned int buflen);

void MakeACK(int blocknum);
void MakeDATA(int blocknum,char* data,int size);
bool MakeWRQ(int opcode,char* filename);

protected:
unsigned char n_dataBuf[TFTP_PACKET_DATA_MAX];
int m_nSize;
};
public:
CTFTPServer();
virtual ~CTFTPServer();
bool Start(std::string strIP,USHORT nPort = TFTP_DEFAULT_PORT);
bool Stop();
protected:
static DWORD WINAPI WorkThread(LPVOID lParam);
HANDLE m_hWorkThread;
private:
SOCKETm_sockSvr;
SOCKADDR_INm_sockAddr;
SOCKADDR_INm_clientAddr;
boolm_bRunning;
CPacketm_packetRecv;
CPacketm_packetSend;

FILE*m_fileRecv;
unsigned intm_nPackNum;
unsigned intm_nTotalBytes;
charm_chFileName[64];
std::ofstreamm_logFile;
void ProcessRecvData();
};</pre> 

实现文件：

&nbsp;<pre style="border: 1px dotted #785;background: #f5f5f5;">// TFTPServer.cpp: implementation of the CTFTPServer class.
//
//////////////////////////////////////////////////////////////////////

#include "stdafx.h"
#include "TFTPServer.h"

//////////////////////////////////////////////////////////////////////
// TFTP packet class functions
//////////////////////////////////////////////////////////////////////
CTFTPServer::CPacket::CPacket()
{
Clear();
}
CTFTPServer::CPacket::~CPacket()
{

}

unsigned char* CTFTPServer::CPacket::GetData(int pos)
{
return &amp;(n_dataBuf[pos]);
}

int CTFTPServer::CPacket::GetSize()
{
return m_nSize;
}

void CTFTPServer::CPacket::Clear()
{
m_nSize=0;
memset(n_dataBuf,0,TFTP_PACKET_DATA_MAX);
}

bool CTFTPServer::CPacket::IsValid()
{
return (m_nSize!=-1);
}

bool CTFTPServer::CPacket::IsNull()
{
return (m_nSize == 0);
}

void CTFTPServer::CPacket::SetSize(int N)
{
if ( N &gt; TFTP_PACKET_DATA_MAX) 
{
m_nSize =-1;
} 
else
{
m_nSize =N;
}
}

bool CTFTPServer::CPacket::AddByte(BYTE c)
{
if (!IsValid()) return false;
if (m_nSize &gt;= TFTP_PACKET_DATA_MAX)
{
m_nSize=-1;
return false;
}
n_dataBuf[m_nSize] = (unsigned char)c;
m_nSize++;
return true;
}

bool CTFTPServer::CPacket::AddMem(char* mem,int len)
{
if (!IsValid()) return false;
if ( m_nSize+len&gt;= TFTP_PACKET_DATA_MAX)
{
m_nSize=-1;
return false;
}
memcpy(&amp;(n_dataBuf[m_nSize]),mem,len);
m_nSize+=len;
return true;
}

bool CTFTPServer::CPacket::AddWord(WORD c)
{
//一个Word等于两个Byte,先存高一个Byte,再存低Byte
if (!AddByte(  *(((BYTE*)&amp;c)+1) )) return false; 
return (!AddByte(  *((BYTE*)&amp;c) ));
}

bool CTFTPServer::CPacket::AddString(char* str)
{
if ((str==NULL)||(strlen(str)==0)) return true;
int n=strlen(str);

for (int i=0;i&lt;n;i++) 
if (!AddByte(*(str+i))) 
return false;
return true;//AddByte(0);
}

bool CTFTPServer::CPacket::GetMem(int pos,char* mem,int len)
{
if (pos&gt;m_nSize) return false;
if (len&lt;m_nSize-pos) return false;

memcpy(mem,&amp;(n_dataBuf[pos]),m_nSize-pos);

return true;

}

WORD CTFTPServer::CPacket::GetWord(int pos)
{
WORD hi = GetByte(pos);
WORD lo = GetByte(pos+1);
return ((hi&lt;&lt;8)|lo);
}

BYTE CTFTPServer::CPacket::GetByte(int pos)
{
return  (BYTE)n_dataBuf[pos];
}

bool CTFTPServer::CPacket::IsACK(int blocknum)
{
if (GetSize()!=4) return false;
WORD opcode =  GetWord(0);
WORD block = GetWord(2);
if (opcode != TFTP_OPCODE_ACK) return false;
if (blocknum != block)  return false;

return true;
}

bool CTFTPServer::CPacket::IsDATA(int blocknum)
{
if (GetSize()&lt;4) return false;
WORD opcode =  GetWord(0);
WORD block = GetWord(2);
if (opcode != TFTP_OPCODE_DATA) return false;
if (blocknum != block)  return false;

return true;
}

bool CTFTPServer::CPacket::IsError()
{
if (GetSize()&lt;4) return false;
WORD opcode =  GetWord(0);
return  (opcode == TFTP_OPCODE_ERROR);
}

bool CTFTPServer::CPacket::GetError(char* buf,unsigned int buflen)
{
if (!IsError()) return false;
WORD errcode = GetWord(2);
char* errmsg = (char*)GetData(4);
if (buf==NULL) return false;
if (strlen(errmsg)+10&gt;buflen) return false;
sprintf(buf,"#%i:%s",errcode,errmsg);
return true;
}

void CTFTPServer::CPacket::MakeACK(int blocknum)
{
Clear();
AddWord(TFTP_OPCODE_ACK);
AddWord(blocknum);
}

bool CTFTPServer::CPacket::MakeWRQ(int opcode,char* filename)
{
ATLASSERT(filename!=NULL);
if (strlen(filename)&gt;TFTP_DATA_PKT_SIZE) return false;
Clear();
AddWord(opcode);
AddString(filename);
AddByte(0);
AddString(TFTP_DEFAULT_MODE);
AddByte(0);
return true;
}

void CTFTPServer::CPacket::MakeDATA(int blocknum,char* data,int size)
{
ATLASSERT(data!=NULL);
ATLASSERT(size&lt;=TFTP_DATA_PKT_SIZE);
Clear();
AddWord(TFTP_OPCODE_DATA);
AddWord(blocknum);
AddMem(data,size);
}
//////////////////////////////////////////////////////////////////////
// Construction/Destruction
//////////////////////////////////////////////////////////////////////

CTFTPServer::CTFTPServer():m_bRunning(false),m_sockSvr(INVALID_SOCKET)
{
::ZeroMemory(&amp;m_sockAddr,sizeof(m_sockAddr));
::ZeroMemory(m_chFileName,64);
m_fileRecv = NULL;

m_logFile.open("TFTPSvr.log");
}

CTFTPServer::~CTFTPServer()
{

}

bool CTFTPServer::Start(std::string strIP,USHORT nPort/* = TFTP_DEFAULT_PORT*/)
{
int nError = 0;
int nRet = 0;
m_sockSvr = ::socket(AF_INET,SOCK_DGRAM,IPPROTO_UDP);
if(INVALID_SOCKET == m_sockSvr)
{
nError = ::WSAGetLastError();
return false;
}
//绑定到端口
m_sockAddr.sin_family = AF_INET;
m_sockAddr.sin_addr.s_addr = ::inet_addr(strIP.c_str());
m_sockAddr.sin_port = ::htons(nPort);
nRet = ::bind(m_sockSvr,(sockaddr*)&amp;m_sockAddr,sizeof(m_sockAddr));
if (SOCKET_ERROR == nRet)
{
nError = ::WSAGetLastError();
return false;
}
m_bRunning = true;

m_hWorkThread = ::CreateThread(NULL,0,WorkThread,this,0,0);
if (m_hWorkThread != NULL)
{
::CloseHandle(m_hWorkThread);
}
return true;
}

bool CTFTPServer::Stop()
{
if (m_bRunning)
{
m_bRunning = false;
::closesocket(m_sockSvr);
}

return true;
}

DWORD WINAPI CTFTPServer::WorkThread( LPVOID lParam )
{
CTFTPServer *pSvr = reinterpret_cast&lt;CTFTPServer*&gt;(lParam);

if (pSvr != NULL)
{
while (pSvr-&gt;m_bRunning)
{
int nRecvLen;

int  SenderAddrSize = sizeof(sockaddr_in);
nRecvLen =  recvfrom(
pSvr-&gt;m_sockSvr, 
(char*)pSvr-&gt;m_packetRecv.GetData(), 
TFTP_PACKET_DATA_MAX, 
0, 
(SOCKADDR *)&amp;pSvr-&gt;m_clientAddr, 
&amp;SenderAddrSize
);

pSvr-&gt;m_logFile&lt;&lt;"CTFTPServer::WorkThread 接收到来自"&lt;&lt;::inet_ntoa(pSvr-&gt;m_clientAddr.sin_addr)&lt;&lt;":"&lt;&lt;
::ntohs(pSvr-&gt;m_clientAddr.sin_port)&lt;&lt;std::endl;

if ( ( nRecvLen == 0) || (nRecvLen == WSAECONNRESET ) ) 
{
ATLTRACE("Connection Closed./n");
//break;
}
if ( nRecvLen == SOCKET_ERROR)
{
ATLTRACE("recv error./n");
//break;
}
char bufTrace[1024] = {0};
::sprintf(bufTrace,"%s/n",pSvr-&gt;m_packetRecv.GetData());
ATLTRACE(bufTrace);
pSvr-&gt;m_packetRecv.SetSize(nRecvLen);
pSvr-&gt;ProcessRecvData();
//Sleep(1);
}
}

return 0;
}

void CTFTPServer::ProcessRecvData()
{
//处理接收数据
DWORD dwOpCode = m_packetRecv.GetWord(0);
switch (dwOpCode)
{
case TFTP_OPCODE_READ:
{
ATLTRACE("客户端下载文件/n");
break;
}
case TFTP_OPCODE_WRITE:
{
//获取文件名
::strcpy(m_chFileName,(const char*)m_packetRecv.GetData(2));
m_packetRecv.Clear();
char bufTrace[1024] = {0};
::sprintf(bufTrace,"客户端上传文件:%s/n",m_chFileName);
ATLTRACE(bufTrace);
if( (m_fileRecv  = fopen( m_chFileName, "wb" )) == NULL )
{
ATLTRACE("创建文件失败!/n");
}
//上传文件服务器确认包
m_packetSend.MakeACK(0);

//发送数据
int nLenSended = ::sendto(m_sockSvr,(char*)m_packetSend.GetData(),m_packetSend.GetSize(),
0,(SOCKADDR *)&amp;m_clientAddr,sizeof(m_clientAddr));
m_packetSend.Clear();

//下一个数据序号为1
m_nPackNum = 1;
m_nTotalBytes = 0;
break;
}
case TFTP_OPCODE_DATA:
{
DWORD dwDataNum = m_packetRecv.GetWord(2);

if (m_nPackNum != dwDataNum)
{
m_logFile&lt;&lt;"TFTP_OPCODE_DATA 期望数据序号:"&lt;&lt;m_nPackNum&lt;&lt;" 与发过来的不一致:"&lt;&lt;dwDataNum&lt;&lt;std::endl;
break;
}

char bufTrace[1024] = {0};
::sprintf(bufTrace,"客户端上传文件数据序号:%d/n",dwDataNum);
ATLTRACE(bufTrace);
char dataBuf[TFTP_DATA_PKT_SIZE] = {0};
if (!m_packetRecv.GetMem(4,dataBuf,TFTP_DATA_PKT_SIZE))
{
ATLTRACE("复制数据错误/n");
}
//数据前面4个字节为opcode和packnum
int nDataLen = m_packetRecv.GetSize() - 4;
m_nTotalBytes += nDataLen;
m_nPackNum++;
//m_logFile&lt;&lt;"CTFTPServer::ProcessRecvData() 接收数据大小为:"&lt;&lt;nDataLen&lt;&lt;std::endl;

size_t nWrited = fwrite(dataBuf,1,nDataLen,m_fileRecv);

//m_logFile&lt;&lt;"CTFTPServer::ProcessRecvData() 写入文件大小为:"&lt;&lt;nWrited&lt;&lt;std::endl;
//发完了
if (nDataLen &lt; TFTP_DATA_PKT_SIZE)
{
fclose(m_fileRecv);
m_logFile&lt;&lt;"CTFTPServer::ProcessRecvData() 文件接收完毕,关闭文件&hellip;&hellip;,接收大小为:"&lt;&lt;m_nTotalBytes&lt;&lt;std::endl;
m_logFile&lt;&lt;"CTFTPServer::ProcessRecvData() 文件接收完毕,关闭文件&hellip;&hellip;,接收数据块大小为:"&lt;&lt;m_nPackNum&lt;&lt;std::endl;
}
m_packetRecv.Clear();
m_packetSend.MakeACK(dwDataNum);
//发送数据
int nLenSended = ::sendto(m_sockSvr,(char*)m_packetSend.GetData(),m_packetSend.GetSize(),
0,(SOCKADDR *)&amp;m_clientAddr,sizeof(m_clientAddr));
m_packetSend.Clear();
break;
}
case TFTP_OPCODE_ACK:
{
ATLTRACE("确认包/n");
break;
}
case TFTP_OPCODE_ERROR:
{
ATLTRACE("/n");
break;
}
default:
{
ATLTRACE("/n");
break;
}
}
}
</pre> 

只在本机测试了下，不知道在internet上面是啥效果。

</div>