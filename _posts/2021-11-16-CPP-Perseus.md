---
published: true
title: CPP—Perseus
layout: post
author: Eureka Bing
category: Programming
---

* 目录
{:toc}

## 摘要

呈现给读者的是一份指南，一份极其精要的C++指南。我试图简明扼要的解释C++的核心，切中它的要害。对于语法或者C++一些旁的细节，那些则不是我关心的。我在写这本书时不断对它删减，再删减，直到它删无可删，也就是现在读者所能看到的样子。

## 前言

> 要从哪些东西开始，我觉得并不很难决定，因为我已经知道，要从最简单、最容易认识的东西开始。   
—— 笛卡尔，《谈谈方法》（第二部分）

请允许我先解释这本书的名字：《CPP—Perseus》，它由CPP和Perseus两部分拼接而成。前一部分CPP，即C++英文名字，无须多余解释；后一部分Perseus，取自古希腊神话宙斯之子——珀尔修斯。关于这个名字的由来，我进一步作出解释。

C++是一门庞大而复杂的语言，而且它还在不断地演变，加入新的特性。我想，任何一门潮流的编程语言都会随着时间日渐庞大。即便是C++之父Bjarne Stroustrup都忍不住说：“C++ is a large and complicated language”。

我们该怎么怎么面对C++这个复杂的庞然大物？我想起来了古希腊神话人物珀尔修斯，让我借助他的形象进行说明。珀尔修斯面对美杜莎这个庞然大物，为了割下她的头颅，避免自己变成石头。他穿着飞行的鞋子，依靠世界上最轻的物质——风和云。不直接视那个戈耳工女妖的面孔，通过铜盾的反射看着她的整个形象，一举砍下她的头颅。

我从这个神话中吸取教训：如果一旦沉沦在C++的各种细节，如同在直视美杜莎的眼睛，很快，我就会因此变成石头。印度的《涅槃经》记载的盲人摸象也有相似的寓意，过分注重细节时很难掌握全局的面貌。我想，我只有朝向一个目标——把握C++的核心特性，避重就轻，不关心它的任何细节。正如珀尔修斯做的那样，把所有的精力都集中一起，只为一举砍下美杜莎的头颅，不在别的事情上分心，从而避免成为石头。

让我化身珀尔修斯，轻快扼要的解决掉C++这个庞然大物。


## 1. 学习方法

>
To really know a city, you have to live in it, often for years.
想要真正了解一个城市，你必须生活在其中，而且往往需要多年。  
-- *A Tour of C++*, 2nd, Bjarne Stroustrup.

### 1.1 建议：忽略细节，关注编程技术

- “You don't have to know every detail of C++ to write good programs.” (你不必知道C++的每个细节才可以写出好程序)。
- “Focus on programming techniques, not on language features.”(关注编程技术，而不是语言特性)。
- However, to write good C++ you only need to understand a few fundamental techniques – the rest is indeed details.

以上是三条来自Bjarne Stroustrup的建议。即便是C++之父本人，他同样也不知道C++的所有细节。C++ 是一门庞大而复杂的语言，不要迷失在它的细节之中。写出好的C++代码只需要掌握一些基本技术，其余的确只是细节。

### 1.2 学好C++最低需要读几本书？一些推荐**

最低三本:

- C++合法性指南 — 在 C++ 中你能做什么和不能做什么。
- C++道德指南 — 在 C++ 中你应该和不应该做什么。
- 示例编程指南 — 展示大量示例，自由使用 C++ 标准库。

详见：[isocpp.org: Should I buy one book, or several? ](https://isocpp.org/wiki/faq/how-to-learn-cpp#buy-several-books)  

### 1.3 辅助学习C++的在线网站**

1. [isocpp](https://isocpp.org/)
2. [cppreference](https://en.cppreference.com/w/)
3. [ACCU](https://accu.org/ )  （专注于C++编程，发行期刊有CVu和Overload）

### 1.4 推荐阅读的论文

- [Foundations of C++](https://www.stroustrup.com/ETAPS-corrected-draft.pdf), B. Stroustrup, Texas A&M University, ETAPS 2012 Keynote
- [Abstraction and the C++ machine model](https://www.stroustrup.com/abstraction-and-machine.pdf), B. Stroustrup, 2005 ICESS'04

> We keep learning little things that help us take giant steps as we go.
我们不断学习很少的东西，以帮助我们取得更大的进步。(不积跬步，无以至千里)
 -- Donald Knuth, June 2011.

---