---
published: true
title: 作为艺术的计算机编程
layout: post
author: RuiBing
category: programming
top: true
---


## 核心原则 - 作为艺术的计算机编程

### 1. “事不过三”原则

if you need more than 3 levels of indentation, you're screwed anyway, and should fix your program -- Linus Torvalds, [Linux kernel coding style](https://github.com/torvalds/linux/blob/master/Documentation/process/coding-style.rst).

如果你需要超过3级的缩进，那么你已经把事情搞砸了。应该修复你的程序。

多级指针：Linux Kernel源代码只有20多处 **两级指针，没有任何的***三级指针。

## C++的编程习惯


### 1. 首要原则
- Rules with no enforcement are unmanageable for large code bases. 没有强制执行的规则会导致大型代码库无法管理。
- 牢记SRP(single responsibility principle)设计原则。比如函数要尽可能简单。

### 2. 函数
- 尽量不要把bool值作为函数参数，而应该是使用枚举类。参见：[千万不要把 BOOL设计成函数参数](https://coolshell.cn/articles/5444.html)
- 避免使用带有太多内置类型的函数，如: draw_line(int, int, int, int);  // obscure。这种代码不容易理解参数类型的含义。

### 3. 类

- friend class有利于代码的模块化，它也可用于特定的成员函数。如Node和LinkedList::search()
- 类的设计要合理，make sense, 起码要能从类的名字可推测它可能提供的操作。
- 能用const的地方一定要用const，这会把程序员的意图告诉别人。
- 使用标准库，而不是模仿标准库。( mimics the standard library)
- explicit关键字可以避免代码隐式转换。

### 4.其他

- 避免裸的Union。因为Union可以规避C++的类型系统，导致不易察觉的bug，因此要避免这样使用。我们可以单独维护一个type field，使它成为一个tagged union；或者，使用C++标准库的variant。

## TODO-list

Bjarne Stroustrup谈到，“C++是一门庞大的语言，许多人迷失在它的细节中。然而，写好C++你只需要掌握一些基本技巧，其余的确只是细节”[BS12]。他的这番话也为我的C++学习指明了方向： 专注于C++核心的编程技术，忽略各种繁杂的细节。不是说细节完全不重要，而是细节可以在编程时通过Google学会，不需要浪费大量的时间去掌握细枝末节。把时间用在重要的事情上，提高学习的效率。

因此我从BS的论文、采访、书籍里面搜集到了他提到的一些C++技术，记录在下面，并且会对每条给出解释和说明。希望这些对其他C++学习也能有些帮助。注意的是，C++标准库vector,map,set,list等一系列容器里面已经应用了这些技术，如果你不能理解，可以尝试通过阅读标准库的源码来得到更多的领悟。

- classes for separating interfaces from implementations.
- constructors for establishing invariants, including acquiring resources,
- destructors for releasing resources,
- templates for parameterizing types and algorithms with types
- mapping of source language features to user-defined code specifying their meaning, e.g. [] for
- subscripting, the for-loop, new/delete for construction/destruction on the free store, and the {}
lists.
- use of half-open sequences, e.g. [begin():end()), to define for-loops and general algorithms.


#$# 解释说明

"classes for separating interfaces from implementations"。「类」用于从实现中分离接口。BS在和Bill Venners的访谈提到：“许多C++程序员陷入困境是因为对「面向对象」的滥用，他们认为 C++ 应该是一种非常高级的语言，并且一切都应该是面向对象的。他们认为你应该通过创建一个类作为具有大量虚函数的类层次结构，用这一部分来完成所有工作。例如，这种思想反映在像Java这样的语言中，但很多东西不适合「类」的层次结构。整数(int)不应该是类层次结构的一部分，它不需要，把它放进类里面要开销的；而且很难优雅地做到这一点”[BS03,Bill]。就是说：

1. 当你只是需要一个数据结构时，一定不要去把它设计成「类」。如果它真的是一个数据结构，那就让它成为一个数据结构。
2. 你不应该计划一个层次结构，因为你不需要。仅当你确实需要一个层次结构时，再去使用继承。
3. 你可以用大量独立的「具体类」来编程，比如你想要一个复数，那就写一个复数。不需要用带虚函数的「抽象类」。

---

“constructors for establishing invariants, including acquiring resources； destructors for releasing resources,

这句话包含了两种技术：不变式，RAII。

- 对于不变式，BS提到：“我的经验法则是，当且仅当您可以考虑建立类的不变量时，您才应该拥有一个具有接口和隐藏表示的真实类”[BS03,Bill]。不变式用来保证对象有效。比如，你有个Temperature(温度)类，它接受输入的浮点数用来初始化，但是你始终应该在构造函数里面检测温度是否是大于绝对零度(-273.15℃)，因为初始化的数字小于绝对零度时，那它必定是错误值，对象是无效的。虽然你完全也可以用成员函数实现这一检测，但不应该那样做，你应该总是在构造函数建议不变式。这样就可以完全避免实例了一个无效的对象。
- RAII，资源申请即初始化。就是说，当你开始申请资源时，表明你正在初始化；反过来，当你在销毁对象时，你应该释放资源。RAII，它把对象的生命周期与资源申请/释放的时间点联系到一起。它看起来不能称为一个技术，无非是在构造函数中申请资源，在析构函数中释放资源。但如果不它单独用一个术语去“显式的”提醒程序员们有这么一个技术存在，那么很多程序员就可能会在各种成员函数中申请资源，因此会造成资源管理的灾难。RAII提醒每个C++程序员，你最好不要在非构造函数中new一个资源，应该总是在构造函数申请，在析构函数释放。如果一个总是能做到RAII，那他基本不可能会写出资源泄漏的代码。（注意：「资源」一词不仅仅是指free store自由存储这一内存资源，也包括了文件描述符、信号量、数据库锁等资源，一定要避免这些资源的泄漏）。


[BS12] B. Stroustrup: Foundations of C++, Texas A&M University.
[BS03,Bill] A Conversation with Bjarne Stroustrup, https://www.artima.com/intv/bjarne.html


## 从示例中学习编程



