"In God I trust, all others I pen-test" - Binoj Koshy, cyber security expert


read all the articles in the hackndo blog

https://beta.hackndo.com/about/

this videos are a good start

https://www.youtube.com/watch?v=1S0aBV-Waeo

https://www.youtube.com/watch?v=Uk-xv8uxiJo

Intro to Binary Exploitation (Pwn)
a playlist

https://www.youtube.com/playlist?list=PLHUKi1UlEgOIc07Rfk2Jgb5fZbxDPec94

RazviOverflow 

https://www.youtube.com/playlist?list=PLchBW5mYosh_F38onTyuhMTt2WGfY-yr7

this docs

https://bitvijays.github.io/LFC-BinaryExploitation.html

https://web.archive.org/web/20120622232123/http://www.mentalcases.net/texts/security/TextSectionStudy.txt

format string vuln

https://owasp.org/www-community/attacks/Format_string_attack

stanford article from 2001 : https://cs155.stanford.edu/papers/formatstring-1.2.pdf


esp = start/top of the stack / stack pointer

ebp = bottom/base of the stack

eip = holds index / instruction pointer / return addr pointer

Intel :

OPERATION DESTINATION, SOURCE

Exemple : mov eax, 42

AT&T :

OPERATION SOURCE, DESTINATION

Exemple : mov $42, %eax

The instruction pointer holds the location of the next instruction, and increments itself after every instruction. So basically, the CPU reads the instruction pointer, fetches the next instruction, does it, increments the instruction pointer and then goes back to step one.

The value of the instruction pointer itself can be set to some arbitrary location in memory. You ever wonder what an infinite loop is? It's what happens when an instruction pointer is set to instructions that keep telling the instruction pointer to set itself to that same set of instructions.


there are three pertinent registers to a stack.

■ EIP The extended instruction pointer. This points to the code that you are currently executing. When you call a function, this gets saved on the stack for later use.

■ ESP The extended stack pointer. This points to the current position on the stack and allows things to be added and removed from the stack using push and pop operations or direct stack pointer manipulations.

■ EBP The extended base pointer. This register should stay the same throughout the lifetime of the function. It serves as a static point for referencing stack-based information like vari- ables and data in a function using offsets. This almost always points to the top of the stack for a function.


https://www.youtube.com/watch?v=2jfoLxQXq3Y&list=PLxfrSxK7P38X7XfG4X8Y9cdOURvC7ObMF



Understanding the architecture helps one at various stages during exploit development. 
https://www.sciencedirect.com/science/article/pii/B9781597494861000036


Fuzzing is a process of sending deliberately malformed data to a program in order to generate failures, 
or errors in the application. When performed by those in the software exploitation community,
fuzzing usually focuses on discovery of bugs that can be exploited to allow an attacker to run their own code, 
and along with binary and source code analysis fuzzing is one of the primary ways in which exploitable software bugs are discovered.

https://resources.infosecinstitute.com/topic/intro-to-fuzzing/



...technique called canary values, where an additional value is placed on the stack in the prologue and checked for integrity in the epilogue.
- hack proofing ur network (2002) , chap 8, buffer overflow

pour se protéger des buffer overflows, gcc ajoute une valeur secrète sur la stack, appelée canari, juste avant l’adresse contenue dans EBP. Un dépassement de tampon est en général utilisé pour réécrire EIP, qui se trouve être juste derrière l’adresse contenue dans EBP. Donc si jamais cela se passait, la valeur secrète serait également réécrite. Une vérification de cette valeur est effectuée avant de sortir de la fonction, et si elle a été modifiée, alors le programme s’arrête brutalement et nous jette des tomates à la figure.
https://beta.hackndo.com/technique-du-canari-bypass/



Since we examined the stack of a compiled program, we know that to take con- trol of the EIP register, we must overwrite the 8 bytes of the buffer, then 4 bytes of a saved EBP register, and then 4 bytes of saved EIP.
This means that we have 12 bytes of filler that must be filled with something. In this case, we’ve chosen to use 0x90, which is the hex value for the Intel NOP operation.This is an imple- mentation of a NOP sled, but we won’t need to slide in this case because we know where we need to go and can avoid it.This is just filler that we can use to overwrite the buffer and EBP on the stack.We set this up using the memset() C library call to set the first 12 bytes of the buffer to 0x90.
memset(writeme,0x90,12);

- hack proofing ur network (2002) , chap 8, buffer overflow

 push $0x808e248, which pushes the address of the string “EXAMPLE\n” onto the stack.To see what’s at that address, we can type the following from the (gdb) prompt: x/s 0x808e248.
 
- hack proofing ur network (2002) , chap 8, buffer overflow

 we can’t have the address of the string without it being in memory; and without the address, we can’t get to the string. 
 In this case we do a jmp/call: when you execute a call, the address of the next instruction is pushed onto the stack.
 The call pushes the address of the next instruction onto the stack (the next instruction down is actually a string). But the call actually doesn’t know the dif- ference.
 
- hack proofing ur network (2002) , chap 8, buffer overflow

 Now that we know where the stack starts, how can we exactly pinpoint where our shellcode is going to be on the stack? Simple: we don’t!
We just “pad” our shellcode to increase its size so we can make a reasonable guess.

- hack proofing ur network (2002) , chap 8, buffer overflow




```
 char buffer[25];
 scanf("%s", buffer);

 The preceding code is vulnerable to buffer overflow. If you carefully notice, the buffer size has been set to 25 characters. However, what if the user enters data more than 25 characters? The buffer will simply overflow and the program execution will end abruptly.

 What are fuzzers?
In the preceding example, we had access to the source code, and we knew that the variable buffer can hold a maximum of 25 characters. So, in order to cause a buffer overflow, we can send 30, 40, or 50 characters as input. However, it's not always possible to have access to the source code of any given application. So, for an application whose source code isn't available, how would you determine what length of input should be sent to a particular parameter so that the buffer gets overflowed? This is where fuzzers come to the rescue. Fuzzers are small programs that send random inputs of various lengths to specified parameters within the target application and inform us the exact length of the input that caused the overflow and crash of the application.

- Sagar Rahalkar, Nipun Jaswal - Metasploit Revealed _ Secrets of the Expert Pentester -  Build your Defense against Complex Attacks-Packt (2017) p 162
```


ShellCode: This is the machine language used to execute on the target system. Historically, it was used to execute a shell process, granting the attacker access to the system. So, ShellCode is a set of instructions a processor understands.

Registers are placeholder memory variables that aid execution




  ret2libc
  
  ROP

  "Use code that’s already there"


"
Bypassing NX
NX sets the stack memory region as non-executable
the same way that Linux files have permissions, memory regions within a process also have permissions, read write execute
before, if u had a shellcode in the stack u could execute it
"
https://youtu.be/F4SwLKQI6Vs


Address space layout randomization (ASLR) = ASLR randomly arranges the address space positions of key data areas of a process, including the base of the executable and the positions of the stack, heap and libraries.

attackers trying to execute return-to-libc attacks must locate the code to be executed, while other attackers trying to execute shellcode injected on the stack have to find the stack first.


ROP

ret2libc

ret2plt

stack pivoting

ldd prints the shared objects (shared libraries) required by each
       program or shared object specified on the command line.
       
```
level5@RainFall:~$ ldd level5
	linux-gate.so.1 =>  (0xb7fff000)
	libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb7e4e000)
	/lib/ld-linux.so.2 (0x80000000)
```


PLT vs GOT

```
.plt
This is the PLT, or Procedure Linkage Table. These are stubs that look up the addresses in the .got.plt section, and either jump to the right address, or trigger the code in the linker to look up the address. (If the address has not been filled in to .got.plt yet.)

.got.plt
This is the GOT for the PLT. It contains the target addresses (after they have been looked up)

TL;DR: Those starting with .plt contain stubs to jump to the target, those starting with .got are tables of the target addresses.

	call   0x8048300 <puts@plt>
	
	Let’s step through the process until we get to the actual puts function.
	
	0x8048300 <puts@plt>:	jmp    DWORD PTR ds:0x804a00c
	
	We’re in the PLT, and we see that we’re performing a jmp, but this is not a typical jmp. This is what a jmp to a function pointer would look like. The processor will dereference the pointer, then jump to resulting address.

https://systemoverlord.com/2017/03/19/got-and-plt-for-pwning.html
```

`A stub is a controllable replacement for an Existing Dependency`

PLT - GOT https://www.youtube.com/watch?v=NdlvRRUBG5Y&ab_channel=SourceMeetsSink

```
The purpose of dynamic linking is that binaries do not have to carry all the code necessary to run within them - this reduces their size substantially.
Due to the way ASLR works, these addresses need to be resolved every time the binary is run. Enter the PLT and GOT

When you call puts() in C and compile it as an ELF executable, it is not actually puts() - instead, it gets compiled as puts@plt

Well, as we said, it doesn't know where puts actually is - so it jumps to the PLT entry of puts instead. From here, puts@plt does some very specific things:
If there is a GOT entry for puts, it jumps to the address stored there.
If there isn't a GOT entry, it will resolve it and jump there.

The GOT is a massive table of addresses; these addresses are the actual locations in memory of the libc functions.
puts@got, for example, will contain the address of puts in memory. When the PLT gets called, it reads the GOT address and redirects execution there. 
If the address is empty, it coordinates with the ld.so (also called the dynamic linker/loader) to get the function address and stores it in the GOT.
```
https://ir0nstone.gitbook.io/notes/types/stack/aslr/plt_and_got







