 
## Memory / data storage

*links*
https://omu.rce.so/lessons/asm-x86-64/
https://ctf101.org/reverse-engineering/what-is-assembly-machine-code/
https://www.cs.virginia.edu/~evans/cs216/guides/x86.html

*Source to compilation*
Complex languages are compiled into machine code.
It is a one-way process unless decompilers are used to translate.
Disassemblers help to assemble the machine code

*64 bit system*
Registers = small storage location (in 64 bit)
Multi-sized access --> Register RAX can have lower 32 bits accessed by EAX, then 16 bits accessed by AX
RIP --> a register storing a pointer (it just goes through the machine code and runs it)
hardware-stack --> is like a stack for function calls etc.
CPU will fetch the instruction at RIP, decode it and run it

*memory*
memory is an array of **bytes** -> 8bit numbers
`[]` square bracket is used to dereference memory adresses

*diagram of memory*
```
higher address
0xffffffff --> .----------------.
               |    reserved    |  <-- command line args
               +----------------+      environment variables
               |                |
               |     stack      |  <-- user stack, function frames
               |       |        |
               :       |        :
               '       v        '
                                   <-- mapped data
               .       ^        .
               :       |        :
               |       |        |
               |     heap       |  <-- user heap, dynamic memory
               +----------------+
               |      bss       |  <-- global memory 
               +----------------+
               |     text       |  <-- code segments
0x00000000 --> '----------------'
lower address
```

*THE STACK*
- stack grows from higher to lower memory addresses
	- 0x7FFFFFFF000 to 0x7FFFFFDE000
- It's the place to store local variables
- between RBP (base pointer) and RSP (stack pointer)
- `push`
	- also means move down the RSP by bytes of value, then store value in stack
```
sub rsp, 8
mov [rbp-16], (value)
```
- `pop <destination>`
	- also means move the RSP up
	- stores value popped into destination

*Function calls*
- Storing return points `call`
	- create a new stack frame
	- `[RBP +8]` is the return point

Function prologue
```
push rbp
mov rbp, rsp
```
previous `rbp` is pushed into bottom of stack, then `rsp` address is moved into `rbp`. Since `rbp` is now pointing to bottom of stack, a new stack frame is created.

Function epilogue
```
(leave)
mov esp, ebp
pop ebp
ret
```
location of `ebp` is stored in `esp`, bottom of stack is now previous beginning, a stack frame is deleted . Last value is returned and placed in `ebp` which now points to previous start.
`ret` will pop last value into `eip` changing control back.

*Representation!*
https://users.ece.utexas.edu/~valvano/embed/chap3/chap3.htm#:~:text=Numbers%20are%20stored%20on%20the,will%20have%20a%20separate%20address.
**numbers** 
-> changed to bits (binary) -> organised into bytes (8 bits which is 2^8 = 256 numbers, or 0-255 or 0x00 to 0xff)

UNSIGNED -> normal binary base 2 system 
SIGNED -> N = -128•b7 + 64•b6 + 32•b5 + 16•b4 + 8•b3 + 4•b2 + 2•b1 + b0 -> can rep negative numbers
(this is for 8bit numbers - 1 byte, up to 255)

There is also 16bit, 32bit etc. (used as HALF-WORD or WORD, then DWORD...), but if data has multiple bytes needs to know which to read first..

*endianness!*
basically its like the order in which you read your bytes -> little endian is back to front (least significant byte stored first), big endian is front to back (most significant byte)
0x86 architecture uses little endian

## Assembly Syntax

*Execution types*
	1. Data movement (`mov rax, [rsp - 0x40]`)
	2. Arithmetic (`add rbx, rcx`)
	3. Control-flow (`jne 0x8000400`)

*Data movement*
`mov <destination>, <target>` --> move target into register
`[esi+eax]` --> address calculation, like putting * for pointers in c++ to get the data at the mem
`push <target>` --> push target onto the stack
`pop <target>` --> removes first data element from stack into target
`lea <destination>, <target>` --> load effective address into destination register. Target is a **memory address**, used for moving an adress into a register instead of getting  the value at the adress
`movabs` --> movabs is used for absolute data moves, to either load an arbitrary 64-bit constant into a register or to load data in a register from a 64-bit address.
`mov(l,d...)` --> just a normal move but with length of bits specified, e.g. l is long


*Arithmetic*
`add <operand>, <operand>` --> add two operands, stored in first
`sub` --> same as add
`inc <operand>` --> increase contents of operand by one
`dec` --> decrease
`imul <register> <operand> <const>` --> multiply and store in first operand (third is optional, if included, only back 2 is multiplied)
...

*Control-flow*
`jmp <label>` --> jump to instruction at label
`cmp <op>, <op>` --> compare
`j(condition) <label>` --> jumps if condition is met, use after a cmp e.g. `je <label` = jump when equal
https://faydoc.tripod.com/cpu/jns.htm (summary of jumps)
`call <label>` --> jump to label and push current code location into hardware supported stack
`ret` --> pop the last location of the stack and jump there

*Misc. assembly*
`DWORD PTR`  / `QWORD PTR`--> double word or quadruple word pointer, 4 / 8bytes, eg. default value of int is 4 bytes (32bit)
type man C command to find out what it does
`eax` is like half of `rax` so if they mov stuff in eax then use rax the stuff is inside
ARRAYs will just be a `[address + count]` where count increases
`[+<num>]` means dereferencing, get the memory of the address stored in register
`<num>()` is equivalent
`cdqe` --> extends a dword to qword (fill with zeroes ig?) when going from eax to rax
`movzx` --> zero-extends the source to the destination
`al` --> lowest 
`and` --> bitwise operation returns bits based on comparing 2 bits (two 1's give a 1 otherwise 0)
`xor %eax %eax` --> %eax = 0 since 1^1 is 0 and 0^0 is 0




## Tools
*ida*
Try using Hexrays for decompilation (view->subview->generate pseudocode OR f5)
options->general->basic block boundaries

*commands*
; --> add label/comment
(space) --> change view/m
n --> to change name of a struct
r --> conversion from hex to ascii
Can right click to change hex to decimal

*godbolt*
c to assembly

*misc*
`ltrace ./a.out` --> sees which functions are being called


*ANGR* 
really good for Re

*radare2*
`r2 -d (file)`
`pdf @main` --> can also analyse any other function
`db (reg)` --> set breakpoint
`dc` --> comtinue running
`px @(reg)` --> print
`s (function)` + `VV` --> goes to flow chart mode


## Assembly files

https://www.codeconvert.ai/assembly-to-c-converter

## WASM

https://github.com/WebAssembly/wabt?tab=readme-ov-file -> convert it and stuff
https://developer.mozilla.org/en-US/docs/WebAssembly/Understanding_the_text_format --> understand the WAT


## C (cries)
*Really the basics*
```
#include <stdio.h> //standard input output
#include <stdlib.h> //standard c library functions

int main(int argc, char * argv[]){ //command line args, null terminated array of strings to store args

  printf("Hello World!\n");

  return 0;
}
```

*pointers*
`int * p` --> declares a integer pointer p, it stores a memory address
`*p` --> dereferences p to get the value at the address stored in p
`&a` --> address of variable a

*arrays*
arrays are just contiguous memory block of a sequence of similarly typed data types. Array is a reference to first data item, so each array value is just a **pointer** to memory
`int a[] = {10, 11, 12}`
`a[i] is *(a+i)` array indexing is actually just special pointer dereferencing
although the `a[1]` doesn't actually shift by 1 bytes. Instead, it changes by the size of the data type stored by array. E.g. integers are 4 bytes wide, so calling `a[1]` on an array of integers shifts the reference by 4 bytes

*strings*
string is simply an array of characters that is null terminated
`char str[] = "Hello World!\n";` and
`char str[] = {'H','e','l','l','o',' ','W','o','r','l','d','!','\n','\0'};` 
are equivalent


`readgsword(offset)` --> Read memory from a location specified by an offset relative to the beginning of the GS segment (see stack guard in pwn)
## Compilation
*compiling c*
`gcc <file>.c` --> will produce a.out file which is executable
`gcc -o <new filename> <file>.c` --> to choose filename

steps
1. pre processing (remove comments, expand macros etc.) --> filename.i
2. compile to assembly --> filename.s
3. assembled to machine level instructions --> filename.o
4. linking (extra stuff like linking functions, extra code for setting up env) --> a.out

*linking*
can create .o files with `gcc -c <file.c>`
you can have a file with a function that exists in another file (but it wouldn't link)
`gcc <file1>.o <file2>.o` --> links 2 object files (can use one like a module)

`ld <file1>.o <file2>.o` --> linking directly BUT this wouldn't work cuz it skips a lot of steps like getting the libc functions, various codes that do various things like starting, ending etc.
`ld --dynamic-linker /lib64/ld-linux-x86-64.so.2  -o hello /usr/lib/x86_64-linux-gnu/crt1.o -lc /usr/lib/x86_64-linux-gnu/crti.o hello.o world.o` 

*32 bit*
`gcc -m32 <file>.c` --> compile to 32 bit, default is 64 bit
`gcc -no-pie -m32 -fno-stack-protector -z execstack helloworld.c` --> 32 bit compilation with security features turned off

*function and system calls*
clibrary functions are generally just easy to use ways to do system calls (which does stuff like input output memory programme execution)
can be traced with `ltrace` and `strace`
`strace ./hello > /dev/null`

e.g. a hello world programme would have functions like `printf` but the actual system call is `write` the function just does some stuff and then calls `write`.