---
published: true
title: Computer terminoloy(计算机术语)
layout: post
author: RuiBing
category: programming
---  

Some of  computer terms.


## Array Decay
language: C\+\+ 
see: [C++ Core Guidelines: P.4 statically type safe](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#p4-ideally-a-program-should-be-statically-type-safe)

### concept:
The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the memory.

Arrays are not basically the same as pointers; they are completely different animals.


### concept(Chinese):
通常说的数组丢失了它的类型和维度，“腐败”(Decay)成了指针或值。这种情况发生在函数通过值或指针传递数组时，实际上，数组并没有发生改变，而是我们得到的指针是指向了数组的第一个元素。References 1给的代码示例，sizeof实际计算的是该指针的字节大小(在64位系统中，指针是8字节)。

错误的本质原因是：数组和指针完全是两种不同的事物。即便当指针指向数组时，程序员也不能理所应当地认为它们等同，至少编译器不认为它们等同。

### References:
1. [What is Array Decay in C++? How can it be prevented?](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/)
2. [SO: What is array to pointer decay?](https://stackoverflow.com/a/1461449)
