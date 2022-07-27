#binary exploit 0x1 stack based - (/)

## 0x1-0x1 what is C/ASM
C/ASM its programming langagues for low level devlopment but asm used for bootloader mod escalating stuff and C for kernel and User apps
print text with those langagues (linux sys)

C
```
	char txt[] = "Hello\n";			//declar string var
	write(fd, msg,sizeof(msg));		//call write with giving argv
```
ASM
```
txt db 'Hello',0xa		;string
len equ $ - msg			;lenght of string

mov edx,len 			;pass len to edx bc sys_write get the link from that reg
mov ecx,txt				;pass string to ecx ************************
mov eax,4				;pass the number of sys_write syscall (x86)
int 0x80				;call kernel
```
## 0x1-0x2 what is a register
a processor register its a quickly accessiblle location that the processor store data in processing time
an example what the processor do when request to add 2 numbers
```
	mov 	<reg1> , <first value>		;mov the val to the first reg
	mov 	<reg2> , <second value>		;*******************second ***
	add 	<reg1> , <reg2>				;add the values and store result into reg1
```
so by that i think you got what the prupose of registers

## 0x1-0x3 think whats is BOF vuln
a BOF vulnerabilty its when you give to the program to much data that he allocated for your input so by that you overflow the other segments
and that can be exploited by controlling eip the prupose from that register is to point on the next instruction so by that you can execute all instructions by putting the address of those instruction and you know what next ;)

##0x2-0x1 heap exploitation - malloc,free[C]
malloc & free its functions that let you to access to the heap segment on the memory write&read data from that segment
usage
````
data = (char *)malloc(sizeof(char)*1024); // by that you allocated 1024 byte on the heap for char type
free(data); 							  // by that you freed the memory and data var its no more pointing on a address on heap
````

##0x2-0x2 chunks
the chunk its the structure of data located on the heap where ther is information about the allocated chunk(size,prev size....) and data

##0x2-0x3 exploitation
the wrong usage of those functions can cauze memory leak,rce so how?
ther is lot of vulns & attacks related to that ther is a table of it i get it from a resource you can find the link at the end of the articel
```
--------------------------------------------------------------------------------------------------------------------------------------------------------
Attack					 target																		technique
--------------------------------------------------------------------------------------------------------------------------------------------------------
Double Free 			| Making malloc return an already allocated fastchunk                      | Disrupt the fastbin by freeing a chunk twice
Forging chunks  		| Making malloc return a nearly arbitrary pointer	                       | Disrupting fastbin link structure
Unlink Exploit  		| Getting (nearly)arbitrary write access              					   | Freeing a corrupted chunk and exploiting unlink
Shrinking Free Chunks 	| Making malloc return a chunk overlapping with an already allocated chunk | Corrupting a free chunk by decreasing its size
House of Spirit			| Making malloc return a nearly arbitrary pointer 						   | Forcing freeing of a crafted fake chunk
House of Lore 			| Making malloc return a nearly arbitrary pointer 						   | Disrupting smallbin link structure
House of Force          | Making malloc return a nearly arbitrary pointer                          | Overflowing into top chunk's header
House of Einherjar      | Making malloc return a nearly arbitrary pointer 						   | Overflowing a single byte into the next chunk
--------------------------------------------------------------------------------------------------------------------------------------------------------
```

##0x3 kernel
the kernel exploitation its a bit hard to understand because you need already some skills on kernel dev and lot of experience on debugging and asm/c
i will talk about that in the after talking about kernel programming

#A QUICK WRITEUP FOR HEAP/STACK BASED (level = easy)
## 0x1 stack ret2win
first think checking in our case nothing is enabled
so lets reverse that binary or without reversing its easy just if you try to fill the input with will cz segmentation fault so after leaking the exact  lenth to eip just check for the win , or write & execute shell code
```
#demo exploit for ret2win using pwn framwork pythno
from pwn import *
len = 40
address_of_win = p32(0x000001) # pack it into littel indian form \x01\x00\x00\x00
payload = 40+address_of_win
p=process("./bin")	#start a process for the vulnerable software
p.send(payload)
```

## 0x2 UAF heap (use after free)
the mean of this attack its when you locate a chunk in memory then locate again then free it x2 then relocate the second pointer will point into the first and with that you can overwrit the vaiables with the value you want
```
#demo exploit for UAF pyhton using pwnlib
def al()
   #instructions to allocate in heap
def free()
   #instruction to free allocated data in heap

p=process("./bin")   
al()
al()
free()
p.send("1") 	#we imagine that the value what where are gonna overwrite its to check if is admin true/false with set it to true
```

you can do that i will give a every easy challenge : https://github.com/andrealan/Software-Security-Lab/blob/master/uaf-exercise/uaf-example.c




```thanks for your attention and good bye\n);exit(NULL);```
