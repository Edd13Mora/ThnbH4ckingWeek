# Shadow Stack

Hi im **Uncle j4ck**, today i will talk to you about another interesting topic "the pwn history and the shadow stack", i hope you will like it :)

<p align="center">
  <img src="https://c.tenor.com/G6a837fJZr8AAAAd/smile-creepy-smile.gif"/>
</p>

## Chapter 1 : mouse and cat (overeview of pwn history)

Hacking for a long time ago is about a game of mouse and cat, the programmer finds a solution for a problem and the hacker tries to abuse it then the programmer try again to fix it and it goes on and on until some good programmer (which isnt you ofc XD) makes a complicated effective design that requires a lot of abstraction, in the low level field we can see an example of that which is the buffer overflow problem or:

<p align="center">
  <img src="https://i.imgur.com/AHiwG9s.png"/>
</p>

we can see a that bof is a great example of this game, programmers tried to solve it by making the stack not executable and flaging the places that should be executed but that was easily bypassed using ret2libc using the libc to execute commands by calling shell functions, programmers another time tried to solve this by shuffling the addresses of libc so the attacker can't call libc functions and cant find the address all, it tooks to bypass it, an address to leak and using that we can calculate where libc is and call our function or by making rop attack and using pieces of code called gadgets, another time the programmers solved this problem by a genius method which i will explain using a story, long time ago our ansestors used canary birds in mines to detect toxic gaz, if the bird died by a small quantity of gaz that means that the place is dangerous and it should be evacuated, that was the stunning idea of stack canaries, these are random generated addresses that stay before the RIP/EIP

```
		|	Canary	| |		Padding		| |		RIP		|
```
and its checked after every function call, but the twist was that it wasn't easy to hack but not impossible, there is two plans to hack it (using bruteforce or by binding it to format string vulns), using format string we can leak the canary and include it in our exploit, or we can use forks of functions to bruteforce every byte of the canary because it doesnt matter if the fork crash as long as we can get the canary

*example of a format string vuln*
```c
char* variable = "die by my hand";
printf("%s", variable);
```

# Chapiter 2 : Dark shadow ages (first design)

That's a long path to get to this solution which is really stunning to talk about, so the solution was to include the protection in the system, something that a normal user cant get a hand to it, can't even control it and here popped the idea of the shadow stack.

- Basically i'll explain how function calls works in a basic way:

	1. Function is called 
	2. The address to the instruction after the function is being pushed to the stack
	3. Segment of virtual memory is made for that function  
	4. Bounderies are set
	5. Function ends 
	6. the address to the instruction after the function is popped to get in use 
	7. Execution completes

Here where the attacker using the memory leak to overwrite the saved address and get hold of the execution.
The idea of a shadow stack is to record the saved addresses and compare them when the function completes the execution.

![Shadow_stack](https://d3i71xaburhd42.cloudfront.net/1fa355cabcaa6650603098c41a3a439fbed718a1/2-Figure1-1.png)

# Chapiter 3 : we solved the problem why not impliment it ?

The idea of having a shadow stack is so fucking brilliant why not impliment it in production, well technically android uses it but why not impliment it in all operating systems, well it's slow, comparing the values and storing them everytime will kill the  program's resources, when taking one program and storing his addresses for example 16 addresses (129 byte) you wouldn't even notice it but imagine a server running complexe http server and a database which they are really complexe programs and i can assure you that every function can call a 100 more function that they also call a more 100 function and it keeps growing, that will really takes memory that we need.

```
On SPEC CPU, excluding Fortran, perlbench and gcc,the average overhead of each scheme was as follows: a traditional shadow stack cost 9.69% overhead in CPU time; a parallel shadow stack cost 3.51%; checking the return address cost 0.8% extra, compared to the no-frills parallel shadow stack; zeroing out expired shadow stack entries (for a parallel shadow stack) cost 0.16%; stack canaries cost 2.54%.
For apache, the parallel shadow stack (overwriting, nozeroing out) had 2.73% overhead. Had our test been network bound or I/O bound, we would expect the CPU overhead to be even lower

```
*Taken from The Performance Cost of Shadow Stacks and Stack Canaries paper {https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/43809.pdf}*


# Chapiter 4

> opinion goes brrrrrrrrrrrrrrrrrr

The idea of having a parallel shadow stack is so genius but it have inconvinients like slowliness, but you didnt hear the whole story, all this bullshit is made on an os perspective, which makes it so slow but what if i told you its possible using the cpu cache, but first of all i really dont know if this is possible and how will it work on production, its just an idea i have, what if we can save the function a saved rip in the cpu cache that would make it blazing fast to access but when a function b is invoked all the data we have from function a goes in a temporary memory and then back to the cpu cache , but that requires the cpu companies to accept the implimentation of shadow stacks and write drivers for it. 


- In other words maybe we will see it from now 5 or 4 years, so for now fuck c, rewrite everything in rust.
