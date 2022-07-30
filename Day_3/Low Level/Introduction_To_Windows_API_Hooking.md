# Introduction To Windows API Hooking
- You Will Learn In This Article About Windows API Hooking And How You Can Do It.

![image](https://user-images.githubusercontent.com/107004485/181091888-68718060-df29-4cdd-ab53-c9d17f4c85d6.png)
## What's API Hooking?
- **API Hooking** Is A Technique We Can Modify With It The Flow Of The API Requests.
## Beginners Friendly
### What's API Requests
- **API Requests** Is A Message Sent To A Specific Server To Provide A Service From An Api.
### What's The Flow Of An API Request?
- **The Flow Of An API Requests** Contains A Request And A Response For A Specific API Operation.
### What's API Hooking Types?
- We Have Just Two Types:
- **Local Hook** : Change Only Specific Processes.
- **Global Hook** : Change All The System Processes.
### Simple Information
- **Windows API Hooking** Is One The Best Techniques Used To Detect Malwares.

# Lets Go With Serghini 
![image](https://user-images.githubusercontent.com/107004485/181099174-cf52ed2e-9df4-4bd8-aeae-3fc2b233adb8.png)

- We Will Hook [MessageBoxA()](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messageboxa) So We Should OverWrite The First 5 Bytes Of The Function.
- My Program Source Code Before Win API Hooking:
```C++
#include <iostream>
#include <Windows.h>

using namespace std;

int main(void) {
	cout << "Without API Hooking." << endl;
	MessageBoxA(0, "Not Hooked Yet LOL.", "From Z0rch3r", MB_OK);
	return 0;
}
```
- Output Of My Program:

![image](https://user-images.githubusercontent.com/107004485/181123176-cdc41606-2abb-4166-81c3-35aab009bf85.png)
- Explaining My Program:
	- [MessageBoxA()](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messageboxa) Display A Window Contains A Title And A Message To Be 	   Displayed In The Window.
- My Program Source Code After Win API Hooking:
```C++
#include <iostream>
#include <Windows.h>
#pragma comment(lib,"user32.lib")
using namespace std;
char hook[5] = { 0 };
char MessageBoxBytes[5];
FARPROC HookedFuncAddr;
int __stdcall SetMyHookFunc(HWND hWnd, LPCSTR lpText, LPCSTR lpCaption, UINT uType) {
	WriteProcessMemory(GetCurrentProcess(), (LPVOID)HookedFuncAddr, MessageBoxBytes, 5, 0);
	return MessageBoxA(0, "Hooked By Z0rch3r kek!", "After WIN API Hooking", MB_OK);
}
int activehook(void) {
	HINSTANCE library = LoadLibraryA("user32.dll");
	HookedFuncAddr = GetProcAddress(library, "MessageBoxA");
	SIZE_T bytesRead = 0;
	ReadProcessMemory(GetCurrentProcess(), (LPCVOID)HookedFuncAddr, MessageBoxBytes, 5, &bytesRead);
	void* SetMyHookFuncAddr = &SetMyHookFunc;
	memcpy_s(hook, 1, "\x68", 1);
	memcpy_s(hook + 1, 4, &SetMyHookFuncAddr, 4);
	memcpy_s(hook + 5, 1, "\xC3", 1);
	SIZE_T bytesWrite = 0;
	WriteProcessMemory(GetCurrentProcess(), (LPVOID)(HookedFuncAddr), hook, sizeof(hook), &bytesWrite);
	return 0;
}
int main(void) {
	MessageBoxA(0, "Not Hooked Yet", "Before WIN API Hooking", MB_OK);
	cout << "something" << endl;
	activehook();
	MessageBoxA(0, "Not Hooked Yet", "Before WIN API Hooking", MB_OK);
	return 0;
}
```
- Output Of My Program:

![image](https://user-images.githubusercontent.com/107004485/181375904-de987058-7c20-4d3f-a981-ae9811fe0834.png)
- Explaining My Program:
	****
	**char hook[5] = {0};**
	- To Save The 5 Original Bytes Of [MessageBoxA()](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-messageboxa)
	
	**HINSTANCE library = LoadLibraryA("user32.dll");
	HookedFuncAddr = GetProcAddress(library, "MessageBoxA");**
	
	- To Get The Memory Address Of MessageBoxA() From [user32.dll](https://en.wikipedia.org/wiki/Microsoft_Windows_library_files#USER32.DLL)
	
	**ReadProcessMemory(GetCurrentProcess(), (LPCVOID)HookedFuncAddr, MessageBoxBytes, 5, &bytesRead);**
	
	- To Save The 5 Original Bytes In MessageBoxBytes Buffer.
	
	**void SetMyHookFuncAddr = &SetMyHookFunc;**
	
	- To OverWrite The 5 Original Bytes With SetMyHookFunc() 

	**memcpy_s(hook, 1, "\x68", 1);**
	**memcpy_s(hook + 1, 4, &SetMyHookFuncAddr, 4);**
	**memcpy_s(hook + 5, 1, "\xC3", 1);**
	
	- On The First Line We Choosed \x68 Because 68 Is The Opcode Of Push, On The Third Line We Choosed \xC3 Because It Means Retn.
# End Of The Article
- Im Happy For You, I Hope You Learned A Lot Of Things About Windows API Hooking.

![image](https://user-images.githubusercontent.com/107004485/181646478-ff208b9c-14d1-4f88-a4b2-f193d7fed4a6.png)

# Some Memes For You
![image](https://user-images.githubusercontent.com/107004485/181613973-489bc5a5-b278-42fa-aa7d-32ee12ece3d7.png)
![image](https://user-images.githubusercontent.com/107004485/181614115-aed8df0a-aa2f-4efa-9dfb-d076e2c5683c.png)
