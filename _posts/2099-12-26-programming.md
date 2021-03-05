---
published: true
title: 关于软件和编程
layout: post
author: Ruibing
category: Programming
---  
## Good books
只挑选一下被强烈推荐的书籍。

Gamma et al., *Design Patterns*, 395 pgs, Addison-Wesley, 1995, ISBN 0-201-63361-2.

推荐理由: Describes “patterns” that commonly show up in good designs. You must read this book if you intend to do OO and generic design work.

推荐者：[ioscpp.org](https://isocpp.org/wiki/faq/how-to-learn-cpp#design-books)


## Importance of coding standard <a name="Importance_of_coding_style"></a>
编码规范的必要性

**Reading  coding standard books**,
>Design books give you ideas and guidelines for thinking at a higher level with objects, and coding standard books **establish best practices across your organization, plus help make sure everybody can read each others’ code** (e.g., so you can move people around if one of the teams falls behind).

see: [Should I buy one book, or several?](https://isocpp.org/wiki/faq/how-to-learn-cpp#buy-several-books) 

**The necessity of rules**, 
> Upgrading old systems is hard. However, we do believe that a program that uses a rule is less error-prone and more maintainable than one that does not. Often, rules also lead to faster/easier initial development.   -- C++ Core Guidelines, isocpp

As mentioned in the [Introduction](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#in-introduction),

> The purpose of this document is to help developers to adopt modern C++ (currently C++17) and to achieve a more uniform style across code bases. 

My Comments: 

我一直认为，遵守良好的代码规范和风格可以显著地减少出错和易于维护，这也应该是一个程序员对自己的基本要求。不拘一格的行事作风并不适用于编程，除非你写的代码永远不会被别人看到。

如果一个人写的代码极其不规范，我觉得没有阅读的必要了。不规范的代码就如同桌子上像狗屎一样的饭菜，即便它美味又营养，食客只会觉得恶心想吐，而不是想动筷子尝尝。我就是这样的一个人，尽可能去追求规范，这是我对编程的最基本要求。

## Is it necessary to use textbooks to learn programming?
学习编程需要教材吗？

The answer by isocpp.org：
>You’ll need a textbook for learning C++. This is the case even when your implementation comes with ample on-line documentation. The reason is that language and library documentation together with sample code are not good teachers of concepts. Typically such sources are silent about why things are the way they are and what benefits you can expect (and which you shouldn’t expect) from a technique. Focus on concepts and techniques rather than language-technical details.

see : [How do I start learning C++?](How do I start learning C++?) 

即便在线文档足够丰富，语言、库文档、示例代码都不容易阐述概念。我们需要书籍来弄清楚事物的本来的面目，而非只是关注技术细节。读书的目的是为了，“知其然，知其所以然”。

## 如何学习C++? 我的学习方法   <a name="how_to_learn_cpp"></a>
**Related Questions**:
- [How do I start learning C++? ](https://isocpp.org/wiki/faq/how-to-learn-cpp#start-learning) 
- [What is the best book to learn C++ from?](https://isocpp.org/wiki/faq/how-to-learn-cpp#best-book)

关于C++的三个方面：合法性指南，道德性指南，代码示例指南。

一些书是可选的，并不表示用这些书写得不好。比如《C++ Primer Plus》就能作为《A Tour of C++ 》的替代，并且它对没有编程基础的人更友好。

此外，对于C++编程规范，直接看ioscpp官方网页就可以了：[C++ 核心准则](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) . 正如它的引言提到的那样，在不同的代码库之间实现一个更规范的代码风格。代码规范是很重要的一件事，我在这里[Importance of coding style](http://ruibing.org/programming/2099/12/26/programming.html#Importance_of_coding_style)做过解释。

## C++不是一门很难的编程语言
C++绝对不是很难，它只是有一些小小细节的需要程序员留意。比如：

- cost可以修饰成员函数，并且不允许你在该成员函数改变对象
- 构造函数和析构函数的写法
- 成员函数居然可以这么玩：在类内声明，在类外定义
- 允许你使用列表构造`initializer-list constructor`，不过你要使用标准库的类型`std::initializer_list`类型
- 可以直接定义operator的系列函数，实现定义或者重载运算符

... 

这么看来，C++完全不是很难，而是有很多小性子。你需要摸清她的脾气，才能很好地相处融洽。

---

## 计算机英语
我发现自己的英语是哑巴英语。自认为阅读英文的能力还不错，听一些电视剧和对话也还可以，但是发音和英文写作是最大的障碍，尤其英文的发音有很大的问题。

我也不打算再花费太多时间在练习英语上面。但是，我想了下，也该有一个自己最低的要求：就是把保证把**每个计算机英文术语都读的十分准确**，不会读就去查。

---

## 喜欢Python还是C++？

我在学Python的时候，我觉得我进入了一家高档餐厅。美女服务员在笑嘻嘻的问我想吃我什么，我只需点好饭菜，喝瓶免费的果汁，坐在那里。等一会儿就可以开动了。

当我在学C\+\+的时候，我觉得我步入了一家自助餐馆。我看到了桌子，纸巾，刀叉，筷子，以及各种半生不熟的菜和生鲜。我需要自己动手，点好炉火，稍稍费些心思，一会儿也就可以做出美味了。

如果你问我喜欢吃高档餐厅还是喜欢吃自助餐。从生活经验来说，我毫不犹豫地选择高档餐厅，因为简单方便。如果从编程的体验来说，我更喜欢自助餐，喜欢工作在事物的底层。虽然麻烦一点，最终获得的快乐也更多。

## 喜欢简洁的网页
比如[cppreference.com](https://en.cppreference.com/w/cpp/language/modules) 这个网站，简洁的排版，突出主要的功能，广告也很少。

类似的还有[filezilla](https://filezilla-project.org/)。

这些网页的功能共同特点是一点也不花哨，该有的功能全都有。现在的网站大都各种花里胡哨的，看起来炫酷，实际没有什么屁用。用户需要从网页上获取有用的信息，而不是被那些奇怪炫酷的东西吸引注意力。

## 自己动手
我发现Linux系统下就没有像Foxmail那样好用的邮件客户端。Hiri, 我用过，付费我也买过，体验不怎么样。MailSpring的同步有问题，我的hotmail邮箱经常同步不到邮件，不知道原因出在哪里，用了几次还是卸载了。Thunderbird的看起来就像是一个被时代遗弃的产品，而且我的hotmail邮箱也登录不上。

我甚至都想自己写个客户端了，可这个工作量和难度也决不轻松，从零开始写可能需要一两年。我先用着Hiri, 如果它也出问题了，就用MailSpring。

MailSpring是开源软件，如果它也出问题，我就去给它提交补丁。提交代码补丁，总比从零开始写代码的代价要小。

我觉得我过去的想法出了问题，总想吃现成的。**现在，我该把过去的想法摒弃了，不能总是想用现成的软件。既然自己也是一个软件开发者，那么遇到需要修理和优化的软件，自己也该参与其中。**

---

## 过早优化不总是万恶之源 <a name="optimization_not_evil"></a>
Donald Knuth博士曾说过,"premature optimization is the root of all evil"(翻译: 过早优化是万恶之源). 这句话成了很多程序员写出糟糕代码的借口, 并且他们从来不试图优化自己的代码. 但是Knuth博士这句话的下半句很少有人知道: " Yet we should not pass up our opportunities in that critical 3%"(而对于影响性能关键的 3%代码，我们则不能放弃优化的机会). 我在工作中就遇到过这样的事情, 在尝试修改少量的核心的代码, 其程序性能提升1~3倍. 如果放弃这样的优化机会, 将是很大的损失.

---

## 我亦无他，唯手熟尔
在编程行业, 编程人员常常会产生一种错觉: 一旦他们学会了使用某个工具, 就自以为掌握了某种技术. 不论是掌握了一门编程语言, 还是学会了何种新型和"潮流时尚"的工具, 如果只是停留在如何使用的水平, 却不知其如何设计出来的, 那么, 程序员仅仅只是使用者罢了, 这谈不上算什么技术.

就以docker为例, 如果我能熟练使用, 我不过也是这个工具的使用者之一. 用<卖油翁>的话说, "我亦无他，唯手熟尔". 我就必须时时提醒自己, 不论掌握哪些工具, 都应该避免产生自我良好的感觉. 要知道, 对于一个刚入门的编程人员, 他们对着官网的文档看半天, 他们也能学得会. 

那么一个"老"程序员和一个"新"程序员应该怎样才能产生差距, 才不会被淘汰呢? 如果你只是比他们多会几种工具, 我相信不需要几年, 就将会被淘汰.  从我的理解, 就必须学习好算法和稳固计算机的底层原理, 并且不断开阔自己编程的思路, 写出良好的代码(无论是代码风格还是执行的性能). 

2020-03-12
