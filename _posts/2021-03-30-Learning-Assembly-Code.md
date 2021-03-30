---
published: true
title:  Learning Assembly Code
layout: post
author: RuiBing
category: Computer Science
---  

x86-64 ISA

## x86-64 calling conventions
passing arguments and return value: 

Up to **six integer or pointer arguments **are passed by registers **in the order**:  

`rdi rsi rdx rcx r8 r9`
  
`rax` is used to return up to 64-bit values. 
And `rdx` can be used to return up to 128-bit values.

More or larger arguments and return values are passed through the `stack`.

## Code examples

C code file mstore.c:

```c
long mult2(long);

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

The assembly-code:

```assembly
multstore:
	pushq	%rbx	
	movq	%rdx, %rbx
	call	mult2@PLT
	movq	%rax, (%rbx)
	popq	%rbx
```
This function has three register sized arguments, so the caller will put them in the first three registers:

- x in `rdi` 
- y in `rsi`
- dest in `rdx` 

So the first instruction is pushing the `rbx` register to the program stack before it gets overwritten. This is because: `rbx` is a **callee saved register ** which means a function that overwrites it needs to save it before modification and restore it before returning.

The second instruction moves the data from `rdx` to `rbx`. This is copying the pointer into `rbx` before making the procedure call. Because `rdx` is caller saved,  the compiler decides to move dest to a register rather than save it on the stack.

The *x* and *y* variables are still in `rdi `and `rsi` so the arguments to *mult2* are already in the right places when the *mult2* function is called.

When *mult2*  returns the return value is an `rax`, so the next line of C code corresponds to exactly one line of assembly which is to move the 64-bit return value from `rax` to the `memory location` addressed by the value in `rbx`, that is copying it into *dest*.

## Reference

- [x86-64 Assembly Programming Part 1: Registers, Data Movement, and Addressing Modes](https://www.youtube.com/watch?v=lUbPUWtmVUU)
- Bryant and O'Hallaron, CS:APP 3e

