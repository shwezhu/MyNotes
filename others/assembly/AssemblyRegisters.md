# Assembly - Registers
Processor operations mostly involve processing data. This data can be stored in memory and accessed from thereon. However, reading data from and storing data into memory slows down the processor, as it involves complicated processes of sending the data request across the control bus and into the memory storage unit and getting the data through the same channel.
To speed up the processor operations, the processor includes some internal memory storage locations, called **registers**.
The registers store data elements for processing without having to access the memory. A limited number of registers are built into the processor chip.

## Processor Registers
There are ten 32-bit and six 16-bit processor registers in IA-32 architecture. The registers are grouped into three categories −
	* 	General registers,
	* 	Control registers, and
	* 	Segment registers.
The general registers are further divided into the following groups −
	* 	Data registers,
	* 	Pointer registers, and
	* 	Index registers.

## Data Registers
Four 32-bit data registers are used for arithmetic, logical, and other operations. These 32-bit registers can be used in three ways −
	* 	As complete 32-bit data registers: EAX, EBX, ECX, EDX.
	* 	Lower halves of the 32-bit registers can be used as four 16-bit data registers: AX, BX, CX and DX.
	* 	Lower and higher halves of the above-mentioned four 16-bit registers can be used as eight 8-bit data registers: AH, AL, BH, BL, CH, CL, DH, and DL.
![](Assembly%20-%20Registers/Photo%20May%201,%202021%20at%2011038%20AM.jpg)

Some of these data registers have specific use in arithmetical operations.
**AX is the primary accumulator**; it is used in input/output and most arithmetic instructions. For example, in multiplication operation, one operand is stored in EAX or AX or AL register according to the size of the operand.
**BX is known as the base register**, as it could be used in indexed addressing.
**CX is known as the count register**, as the ECX, CX registers store the loop count in iterative operations.
**DX is known as the data register**. It is also used in input/output operations. It is also used with AX register along with DX for multiply and divide operations involving large values.

## Pointer Registers
The pointer registers are 32-bit EIP, ESP, and EBP registers and corresponding 16-bit right portions IP, SP, and BP. There are three categories of pointer registers −
**Instruction Pointer (IP)** − The 16-bit IP register stores the offset address of the next instruction to be executed. IP in association with the CS register (as CS:IP) gives the complete address of the current instruction in the code segment.
**Stack Pointer (SP)** − The 16-bit SP register provides the offset value within the program stack. SP in association with the SS register (SS:SP) refers to be current position of data or address within the program stack.
**Base Pointer (BP)** − The 16-bit BP register mainly helps in referencing the parameter variables passed to a subroutine. The address in SS register is combined with the offset in BP to get the location of the parameter. BP can also be combined with DI and SI as base register for special addressing.
![](Assembly%20-%20Registers/Photo%20May%201,%202021%20at%2011426%20AM.jpg)

## Index Registers
The 32-bit index registers, ESI and EDI, and their 16-bit rightmost portions. SI and DI, are used for indexed addressing and sometimes used in addition and subtraction. There are two sets of index pointers −
**Source Index (SI)** − It is used as source index for string operations.
**Destination Index (DI)** − It is used as destination index for string operations.
![](Assembly%20-%20Registers/Photo%20May%201,%202021%20at%2011541%20AM.jpg)

## Control Registers
The 32-bit instruction pointer register and the 32-bit flags register combined are considered as the control registers.
Many instructions involve comparisons and mathematical calculations and change the status of the flags and some other conditional instructions test the value of these status flags to take the control flow to other location.
The common flag bits are:
...

## Segment Registers
Segments are specific areas defined in a program for containing data, code and stack. There are three main segments −
**Code Segment** − It contains all the instructions to be executed. A 16-bit Code Segment register or CS register stores the starting address of the code segment.
**Data Segment** − It contains data, constants and work areas. A 16-bit Data Segment register or DS register stores the starting address of the data segment.
**Stack Segment** − It contains data and return addresses of procedures or subroutines. It is implemented as a 'stack' data structure. The Stack Segment register or SS register stores the starting address of the stack.
Apart from the DS, CS and SS registers, there are other extra segment registers - ES (extra segment), FS and GS, which provide additional segments for storing data.
In assembly programming, a program needs to access the memory locations. All memory locations within a segment are relative to the starting address of the segment. A segment begins in an address evenly divisible by 16 or hexadecimal 10. So, the rightmost hex digit in all such memory addresses is 0, which is not generally stored in the segment registers.
The segment registers stores the starting addresses of a segment. To get the exact location of data or instruction within a segment, an offset value (or displacement) is required. To reference any memory location in a segment, the processor combines the segment address in the segment register with the offset value of the location.

## Example
Look at the following simple program to understand the use of registers in assembly programming. This program displays 9 stars on the screen along with a simple message −
```sass
section	.text
   global _start	 ;must be declared for linker (gcc)
	
_start:	         ;tell linker entry point
   mov	edx,len  ;message length
   mov	ecx,msg  ;message to write
   mov	ebx,1    ;file descriptor (stdout)
   mov	eax,4    ;system call number (sys_write)
   int	0x80     ;call kernel
	
   mov	edx,9    ;message length
   mov	ecx,s2   ;message to write
   mov	ebx,1    ;file descriptor (stdout)
   mov	eax,4    ;system call number (sys_write)
   int	0x80     ;call kernel
	
   mov	eax,1    ;system call number (sys_exit)
   int	0x80     ;call kernel
	
section	.data
msg db 'Displaying 9 stars',0xa ;a message
len equ $ - msg  ;length of message
s2 times 9 db '*' 
```
When the above code is compiled and executed, it produces the following result −
```
Displaying 9 stars
*********
```