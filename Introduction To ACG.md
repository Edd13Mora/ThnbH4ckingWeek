# Introduction To ACG

## Definition of ACG(Arbitrary Code Guard)
- ACG Is a Mitigation Policy Stop Your Code From Allocating RWX Memory.
## Why Its So Dangerous To Disable ACG In Your Process?
- The Attackers Can Add Or Modify RWX Memory Of The Program.

# How I Can Enable ACG In My Source Code?
- We Should Use [SetProcessMitigationPolicy()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy)
- The Code That You Should Add It In Your Source Code:
``` 
  	PROCESS_MITIGATION_DYNAMIC_CODE_POLICY PMDCP = {};
	PMDCP.ProhibitDynamicCode = 1;
	SetProcessMitigationPolicy(ProcessDynamicCodePolicy, &PMDCP, sizeof(PMDCP));
```
- We Will Explain The Code Now:
	- The [PROCESS_MITIGATION_DYNAMIC_CODE_POLICY](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-process_mitigation_dynamic_code_policy) Is A 		Structure Contains Settings Of The Process Mitigation Policy.
	- ProhibitDynamicCode I Set It to 1 To Prevent The Program From Adding Or Modifiying Executable Pages Of Memory.
	- [SetProcessMitigationPolicy()](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) The First 	 Parameter Represent The Mitigation Policy, The Second Parameter Represent The PROCESS_MITIGATION_DYNAMIC_CODE_POLICY Because It Contains The Settings Of The 		Process Mitigation Policy, the third Parameter Represent The Length Of The PROCESS_MITIGATION_DYNAMIC_CODE_POLICY Structure.
# How I Can Know If A Program Has ACG Enabled Using Process Hacker
- **ACG.exe** Has ACG Enabled:
	- First Step:

		![image](https://user-images.githubusercontent.com/107004485/180651915-03859cc2-e243-477b-891f-94ecdb5ea296.png)
	- Second Step:
	
		![image](https://user-images.githubusercontent.com/107004485/180651986-328b0d3a-fa67-40b8-9d33-b35945c791a0.png)
	- Third Step:
	
		![image](https://user-images.githubusercontent.com/107004485/180651997-7b66d899-899a-4228-8c3e-dc945f84123d.png)
	
		- If You See In The Policy Tab Dynamic code prohibited So The Process Has ACG Enabled, If You Didn't See Dynamic code prohibited So ACG Is Disabled In 			The Process.
# I Did Some Experiments:
## I Tried To Allocate RWX Memory When ACG Is Disabled 
- Thats The Source Code Of The Program:
```
#include <iostream>
#include <Windows.h>
using namespace std;
int main() {
	cout << "[#] ACG Is Disabled." << endl << endl;
	size_t dwSize = 1024;
	LPVOID valloc = VirtualAlloc(NULL, dwSize, MEM_COMMIT, PAGE_EXECUTE_READWRITE); 
	if (valloc != NULL) {
		cout << "rwx memory allocated : " << valloc << endl << endl;
	}
}
```
- [VirtualAlloc()](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc) To Allocate RWX Memory You Can Find More Info About The Function In [Msdn](https://docs.microsoft.com/en-us/windows/win32/api/).
- Output Of The Source Code When You Run It:
	
	![image](https://user-images.githubusercontent.com/107004485/180652027-3f528206-bd06-4c69-bbce-c0db76f66a86.png)
		
	- You Can See The RWX Memory Allocated Successfully Without Any Problem.
## I Tried To Allocate RWX Memory When ACG Is Enabled
- Thats The Source Code Of The Program:
```
#include <iostream>
#include <Windows.h>

using namespace std;

int main(void){
	size_t dwSize = 1024;
	cout << "[#] ACG Is Enabled." << endl << endl;
	LPVOID valloc = VirtualAlloc(NULL, dwSize, MEM_COMMIT, PAGE_EXECUTE_READWRITE);
	if (valloc == NULL) {
		cout << "[-] Failed to Allocate RWX Memory." << endl << endl;
	}
	else {
		cout << "[+]RWX Memory Allocated : " << valloc << endl << endl;
}
return 0;
}
```
- Output Of The Source Code When You Run It:

	![image](https://user-images.githubusercontent.com/107004485/180652089-fe40bf72-7ca7-4eea-8459-cb47217f55b7.png)
	
	- You Can See The Program Failed To Allocate RWX Memory.
# Conclusion
- If We Want To Protect Our Malware Or Generally Our Process From Memory Based Attacks We Should Enable ACG, We Learned A Lot Of Things You Can Rest Now.
# Some Memes 
![smalldel](https://user-images.githubusercontent.com/107004485/180652168-7f3acd88-71e8-4ba4-9535-340fc1ad8587.jpeg)

![image](https://user-images.githubusercontent.com/107004485/180652156-852b9813-a458-453e-bf88-309dd42a91e9.png)

![image](https://user-images.githubusercontent.com/107004485/180652131-c10159e1-9765-47cc-ae60-4a2f75ebcbd3.png)