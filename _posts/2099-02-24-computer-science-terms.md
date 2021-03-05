---
published: true
title: Some Terms(一些术语)
layout: post
author: RuiBing
category: programming
---  

# Table of Contents(目录)

- [Locality](#Locality)
- [Single-responsibility principle](#srp)
- [Low coupling](#low_coupling)


## Single-responsibility principle <a name="srp"></a>
[The single-responsibility principle (SRP) ](https://en.wikipedia.org/wiki/Single-responsibility_principle)  is a computer-programming principle that states that every class in a computer program should have responsibility over a single part of that program's functionality, which it should encapsulate. All of that module, class or function's services should be narrowly aligned with that responsibility. --wikipedia

Source: [C++：ENFORCING THE RULE OF ZERO](https://accu.org/journals/overload/22/120/alday_1896) 

评论（comments）: wiki给了一个不错的示例，比如一个生成报表的程序：报表的内容是可变的，报表的格式也是可变。根据这个srp原则, 这两件不同事情有不同的责任，应当拆分到不同的模块或者类里面去。It would be a bad design to couple two things that change for different reasons at different times. 耦合(couple)两个事物不是一个好设计。

---

## Locality <a name="Locality"></a>
In computer science, [locality of reference(wikipedia)](https://en.wikipedia.org/wiki/Locality_of_reference) , also known as the principle of locality, is the tendency of a processor to access the same set of memory locations repetitively over a short period of time. There are two basic types of reference locality--temporal and spatial locality. 

- *Temporal locality* refers to the reuse of specific data, and/or resources, within a relatively small time duration. 
- *Spatial locality* (also termed *data locality*) refers to the use of data elements within relatively close storage locations. *Sequential locality*, a special case of spatial locality, occurs when data elements are arranged and accessed linearly, such as, traversing the elements in a one-dimensional array.

我第一次看到Locality的概念，是在2016年读的CSAPP一书上，隐隐只记得它在强调读写相近的存储空间会有更好的程序性能，也就是*Spatial locality*。

今天，我在看[Resource acquisition is initialization](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) 时，注意它有提到了locality，原话是: locality (it allows acquisition and release logic to be written next to each other).  复习一下，就知道了这个是属于*Temporal locality*的范畴，它常常容易被忽略。比如我们在代码的开头申请了一个变量，在代码快结束的时候才使用这个变量，这违反了temporal locality的原则。

常“温故而知新”，虽不能“为师矣”，但也可以帮助自己融会贯通不断进步。 

---

## Low coupling <a name="low_coupling"></a>

*Low coupling is often a sign of a well-structured computer system and a good design, and when combined with high cohesion, supports the general goals of high readability and maintainability.* Coupling and cohesion are terms which occur together very frequently. Coupling refers to the interdependencies between modules, while cohesion describes how related the functions within a single module are. Low cohesion implies that a given module performs tasks which are not very related to each other and hence can create problems as the module becomes large.



