---
layout: page
title: "Windows-process"
date: 2019-10-13 01:01
---

## TARTUPINFO结构体 指定进程窗口特性
```c++
typedef struct _STARTUPINFO 
{ 
	DWORD cb;			 	 //包含STARTUPINFO结构中的字节数.如果Microsoft将来扩展该结构,它可用作版本控制手段.应用程序必须将cb初始化为sizeof(STARTUPINFO) 
    PSTR lpReserved;		 //保留。必须初始化为NULL
    PSTR lpDesktop;			 //用于标识启动应用程序所在的桌面的名字。如果该桌面存在，新进程便与指定的桌面相关联。如果桌面不存在，便创建一个带有默认属性的桌面，并使用为新进程指定的名字。如果lpDesktop是NULL（这是最常见的情况 ),那么该进程将与当前桌面相关联 
    PSTR lpTitle;			 //用于设定控制台窗口的名称。如果lpTitle是NULL，则可执行文件的名字将用作窗口名.This parameter must be NULL for GUI or console processes that do not create a new console window.
    DWORD dwX;				 //用于设定应用程序窗口相对屏幕左上角位置的x 坐标（以像素为单位）。 
    DWORD dwY;				 //对于GUI processes用CW_USEDEFAULT作为CreateWindow的x、y参数，创建它的第一个重叠窗口。若是创建控制台窗口的应用程序，这些成员用于指明相对控制台窗口的左上角的位置
    DWORD dwXSize;			 //用于设定应用程序窗口的宽度（以像素为单位）
    DWORD dwYSize;			 //子进程将CW_USEDEFAULT 用作CreateWindow 的nWidth、nHeight参数来创建它的第一个重叠窗口。若是创建控制台窗口的应用程序，这些成员将用于指明控制台窗口的宽度 
    DWORD dwXCountChars;	 //用于设定子应用程序的控制台窗口的宽度（屏幕显示的字节列）和高度（字节行）（以字符为单位） 
    DWORD dwYCountChars; 
    DWORD dwFillAttribute;   //用于设定子应用程序的控制台窗口使用的文本和背景颜色 
    DWORD dwFlags;           //请参见下一段和表4-7 的说明 
    WORD wShowWindow;        //用于设定如果子应用程序初次调用的ShowWindow 将SW_*作为nCmdShow 参数传递时，该应用程序的第一个重叠窗口应该如何出现。本成员可以是通常用于ShowWindow 函数的任何一个SW_*标识符，除了SW_SHOWDEFAULT. 
    WORD cbReserved2;        //保留。必须被初始化为0 
    PBYTE lpReserved2;       //保留。必须被初始化为NULL
    HANDLE hStdInput;        //用于设定供控制台输入和输出用的缓存的句柄。按照默认设置，hStdInput 用于标识键盘缓存，hStdOutput 和hStdError用于标识控制台窗口的缓存 
    HANDLE hStdOutput; 
    HANDLE hStdError; 
} STARTUPINFO, *LPSTARTUPINFO;
```

## PROCESS_INFORMATION存储线程的相关信息
```c++
    typedef struct _PROCESS_INFORMATION { 
        HANDLE hProcess; //存放每个对象的与进程相关的句柄 
        HANDLE hThread;        //返回的线程句柄。 
        DWORD dwProcessId;    //用来存放进程ID号 
        DWORD dwThreadId;      //用来存放线程ID号 
    } PROCESS_INFORMATION, *PPROCESS_INFORMATION, *LPPROCESS_INFORMATION;
```

## PROCESSENTRY32 存放进程信息快照
```c++
 PROCESSENTRY32结构如下：    
  typedef   struct   tagPROCESSENTRY32   {    
  DWORD   dwSize;   //   结构大小；    
  DWORD   cntUsage;   //   此进程的引用计数；    
  DWORD   th32ProcessID;   //   进程ID;    
  DWORD   th32DefaultHeapID;   //   进程默认堆ID；    
  DWORD   th32ModuleID;   //   进程模块ID；    
  DWORD   cntThreads;   //   此进程开启的线程计数；    
  DWORD   th32ParentProcessID;//   父进程ID；    
  LONG   pcPriClassBase;   //   线程优先权；    
  DWORD   dwFlags;   //   保留；    
  char   szExeFile[MAX_PATH];   //   进程全名；    
  }   PROCESSENTRY32;
```

## CreateToolhelp32Snapshot
```c++
HANDLE_WINAPI CreateToolHelp32Snapshot(
                                       DWORD dwFlags,	//指定了获取系统进程快照的类型
                                       DWORD th32ParentProcessID//指向要获取进程快照的ID，获取系统内所有进程快照时是0
                                      );
```

## Process32First与Process32Next
如果`CreateToolhelp32Snapshot`函数调用成功返回快照句柄 需要调用Process32First函数查找系统进程的第一个进程
```c++
BOOL Process32First(
                    HANDLE hSnapshot,
                    LPROCESSENTRY32 lppe
                   );
 
```
再调用Process32Next函数列出系统中其它进程
```c++
BOOL Process32Next(
                    HANDLE hSnapshot,
                    LPROCESSENTRY32 lppe
                   );
```
一个输出系统进程的demo
```c++
    #include <stdio.h>
    #include <windows.h>
    #include <string.h>
    #include <tlhelp32.h>
    int GetProcess()
    {
    	//PROCESSENTRY32结构体，保存进程具体信息
    	PROCESSENTRY32 pe32;
    	pe32.dwSize = sizeof(pe32);
    	//获得系统进程快照的句柄
    	HANDLE hProcessSnap = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    	if (hProcessSnap == INVALID_HANDLE_VALUE)
    	{
    		printf("CreateToolhelp32Snapshot error.\n");
    		return 0;
    	}
    	//首先获得第一个进程
    	BOOL bProcess = Process32First(hProcessSnap, &pe32);
    	//循环获得所有进程
    	while (bProcess)
    	{
    		//打印进程名和进程ID
    		printf("%ls----%d\n", pe32.szExeFile, pe32.th32ProcessID);
    		bProcess = Process32Next(hProcessSnap, &pe32);
    	}
    	CloseHandle(hProcessSnap);
    	return 0;
    }
    int main()
    {
    	GetProcess();
    	return 0;
    }
```