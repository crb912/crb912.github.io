---
published: true
title: Some Computer Science Terminoloy(一些计算机术语)
layout: post
author: RuiBing
category: programming
---  

# Table of Contents(目录)
1. [Array Decayt](#ArrayDecay)
2. [Locality](#Locality)

## Array Decay <a name="ArrayDecay"></a>
language: C\+\+ 
see: [C++ Core Guidelines: P.4 statically type safe](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#p4-ideally-a-program-should-be-statically-type-safe)


The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the memory.

Arrays are not basically the same as pointers; they are completely different animals.

通常说的数组丢失了它的类型和维度，“腐败”(Decay)成了指针或值。这种情况发生在函数通过值或指针传递数组时，实际上，数组并没有发生改变，而是我们得到的指针是指向了数组的第一个元素。References 1给的代码示例，sizeof实际计算的是该指针的字节大小(在64位系统中，指针是8字节)。

错误的本质原因是：数组和指针完全是两种不同的事物。即便当指针指向数组时，程序员也不能理所应当地认为它们等同，至少编译器不认为它们等同。

**References**:
1. [What is Array Decay in C++? How can it be prevented?](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/)
2. [SO: What is array to pointer decay?](https://stackoverflow.com/a/1461449)

---

## Locality <a name="Locality"></a>
In computer science, [locality of reference(wikipedia)](https://en.wikipedia.org/wiki/Locality_of_reference) , also known as the principle of locality, is the tendency of a processor to access the same set of memory locations repetitively over a short period of time. There are two basic types of reference locality--temporal and spatial locality. 

- *Temporal locality* refers to the reuse of specific data, and/or resources, within a relatively small time duration. 
- *Spatial locality* (also termed *data locality*) refers to the use of data elements within relatively close storage locations. *Sequential locality*, a special case of spatial locality, occurs when data elements are arranged and accessed linearly, such as, traversing the elements in a one-dimensional array.

我第一次看到Locality的概念，是在2016年读的CSAPP一书上，隐隐只记得它在强调读写相近的存储空间会有更好的程序性能，也就是*Spatial locality*。

今天，我在看[Resource acquisition is initialization](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) 时，注意它有提到了locality，原话是: locality (it allows acquisition and release logic to be written next to each other).  复习一下，就知道了这个是属于*Temporal locality*的范畴，它常常容易被忽略。比如我们在代码的开头申请了一个变量，在代码快结束的时候才使用这个变量，这违反了temporal locality的原则。

常“温故而知新”，虽不能“为师矣”，但也可以帮助自己融会贯通不断进步。
