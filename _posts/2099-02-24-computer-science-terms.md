---
published: true
title: 一些术语
layout: post
author: RuiBing
category: programming
---  

# Table of Contents(目录)

2. [Locality](#Locality)




---

## Locality <a name="Locality"></a>
In computer science, [locality of reference(wikipedia)](https://en.wikipedia.org/wiki/Locality_of_reference) , also known as the principle of locality, is the tendency of a processor to access the same set of memory locations repetitively over a short period of time. There are two basic types of reference locality--temporal and spatial locality. 

- *Temporal locality* refers to the reuse of specific data, and/or resources, within a relatively small time duration. 
- *Spatial locality* (also termed *data locality*) refers to the use of data elements within relatively close storage locations. *Sequential locality*, a special case of spatial locality, occurs when data elements are arranged and accessed linearly, such as, traversing the elements in a one-dimensional array.

我第一次看到Locality的概念，是在2016年读的CSAPP一书上，隐隐只记得它在强调读写相近的存储空间会有更好的程序性能，也就是*Spatial locality*。

今天，我在看[Resource acquisition is initialization](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) 时，注意它有提到了locality，原话是: locality (it allows acquisition and release logic to be written next to each other).  复习一下，就知道了这个是属于*Temporal locality*的范畴，它常常容易被忽略。比如我们在代码的开头申请了一个变量，在代码快结束的时候才使用这个变量，这违反了temporal locality的原则。

常“温故而知新”，虽不能“为师矣”，但也可以帮助自己融会贯通不断进步。 

---





