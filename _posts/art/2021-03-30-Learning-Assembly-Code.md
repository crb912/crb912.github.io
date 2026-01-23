---
published: true
title:  Learning Assembly Code
layout: post
author: RuiBing
category: art
hide: true
---  

x86-64 ISA

## x86-64 calling conventions
passing arguments and return value: 

Up to **six integer or pointer arguments **are passed by registers **in the order**:  

`rdi rsi rdx rcx r8 r9`
  
`rax` is used to return up to 64-bit values. 
And `rdx` can be used to return up to 128-bit values.

More or larger arguments and return values are passed through the `stack`.

## Assembly code examples

C code file mstore.c:

```c
long mult2(long);

// x in %rdi, y in %rsi, dest in %rdx
void multstore(long x, long y, long *dest)
{
    long t = mult2(x);
    *dest = t;
}

```

Command line:

```shell
linux> gcc -Og -S mstore.c
```

### The generated x86-64 assembly-code:

```assembly
multstore:
	pushq	%rbx	
	movq	%rdx, %rbx
	call	mult2@PLT
	movq	%rax, (%rbx)
	popq	%rbx
	ret
```
This function has three register sized arguments, so the caller will put them in the first three registers:

- x in `rdi` 
- y in `rsi`
- dest in `rdx` 

So the first instruction is pushing the contents of `rbx` register onto the program stack before it gets overwritten. This is because: `rbx` is a **callee saved register ** which means a function that overwrites it needs to save it before modification and restore it before returning. All information about local variable names or data types has been stripped away.

The second instruction moves the data from `rdx` to `rbx`. This is copying the pointer into `rbx` before making the procedure call. Because `rdx` is caller saved,  the compiler decides to move dest to a register rather than save it on the stack.

The *x* and *y* variables are still in `rdi `and `rsi` so the arguments to *mult2* are already in the right places when the *mult2* function is called.

When *mult2*  returns the return value is an `rax`, so the next line of C code corresponds to exactly one line of assembly which is to move the 64-bit return value from `rax` to the `memory location` addressed by the value in `rbx`, that is copying it into *dest*.

```assembly
	popq	%rbx
	ret
```
The implicit function return generates two assembly instructions. One to pop the stack and restore the previous value of `rbx`, and finally the return instruction.

>NOTE:
GAS(GNU AS) assembly instructions are generally suffixed with the letters "b", "s", "w", "l", "q" or "t" to determine what size operand is being manipulated. The size designation suffixes:  

>
- b = byte (8 bit).
- s = single (32-bit floating point).
- w = word (16 bit).
- l = long (32 bit integer or 64-bit floating point).
- q = quad (64 bit).
- t = ten bytes (80-bit floating point).


### Functionally equivalent assembly-code:
```assembly
multstore:
	pushq	%rdx	
	call	mult2@PLT
	popq	%rdx
	movq	%rax,  (%rdx)
	ret
```

### Object-code
If we use the *-c* command-line option, GCC will both complier and assemble the code

```shell
linux> gcc -Og -c mstore.c
```
This will generate an object-code file *mstore.o* that is in binary format and hence cannot be viewed directly. Here is a part of hexadecimal representation:

![hex format of object code](https://github.com/crb912/crb912.github.io/blob/master/my_private/image/cs/object-code-example.png?raw=true)


```
53 48 89 d3 e8 00 00 00 00 48 89 03 5b c3
```
This is the object code corresponding to the assembly instructions listed previously.

To inspect the contents of machine-code files, we use the program OBJDUMP as a disassembler. 

```shell
linux> objdump -d mstore.o
```
The result is as follows:

```text
Disassembly of section .text:

0000000000000000 <multstore>:
Offest	Bytes					Equivalent assembly languages
   0:	f3 0f 1e fa          	endbr64 
   4:	53                   	push   %rbx
   5:	48 89 d3             	mov    %rdx,%rbx
   8:	e8 00 00 00 00       	callq  d <multstore+0xd>
   d:	48 89 03             	mov    %rax,(%rbx)
  10:	5b                   	pop    %rbx
  11:	c3                   	retq  
```




### 
a⊕b = (¬a ∧ b) ∨ (a ∧¬b)
## Reference

- [x86-64 Assembly Programming Part 1: Registers, Data Movement, and Addressing Modes](https://www.youtube.com/watch?v=lUbPUWtmVUU)
- Bryant and O'Hallaron, CS:APP 3e
- [x86 Assembly/GAS Syntax](https://en.wikibooks.org/wiki/X86_Assembly/GAS_Syntax) 

