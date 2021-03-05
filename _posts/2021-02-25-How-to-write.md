---
published: true
title: How to Write...  (如何写...)
layout: post
author: RuiBing
category: Programming
---  

>When you wish to instruct, be brief. – Cicero


能把问题和概念给别人阐述清楚，也是能力的一种。无论你懂得再多，如果不能把它们清晰的表达出来，那么想法只能”闷死“在你的脑海里了。

由于受到那些优秀博客和项目的启发，我从中吸取经验，并且在不断学习该如何组织语言写好技术博客或者项目介绍之类的。所以就有了这篇关于“如何写..."的系列，它是我对于这件事的学习心得。

暂时先用中文写，有机会补上英文篇。

**注意： **这篇文章还没有写完，暂时没有充裕的时间...

# 目录
1. [如何写好技术博客?](#write_tech_blog)
2. [如何写项目的ＲeadMe](#read_me)
3. [如何写Wiki](#wiki)


## 如何写好技术博客？ <a name="write_tech_blog"></a>
先欣赏一些优秀的文章

### Example 1
阅读这篇：微软文档库的[Lvalues and Rvalues (C++)](https://docs.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp?view=msvc-160)   
文章类别： 该文章偏向于**概念阐述**。

**点评:**  

这篇文章先从C++存在值类别(value category)的现象说起，然后直接抛出了C++17的几种值类型。为了说明类别之间的关系，采用了结构图。接下来有具体的对上面的一些特定value categories做出解释，最后给出代码示例。

每个段落的开头首句，都总结了最核心的一句话，比如这两句。

- An lvalue has an address that your program can access. 
- A prvalue expression has no address that is accessible by your program.


尽管文章非常之简洁，但是思路清晰，很快地就能让读者了解到Lvalues和Rvalues的基本概念(Basic concepts)知识。非常之好，不可多得的技术文章，毕竟微软人才济济，能写出这种思路清晰的技术文章也就见怪不怪了。

#### 值得学习的地方有：

1. 概念解释类的文章最好要有三部分：概念阐述，具体解释，示例
2. 图表能更好的阐述关系结构，表达思想。有句话叫，A picture is worth a thousand words.(一图胜千言)。
3. 段落的的一句话要简明扼要，切中概念的关键点。

### Example 2
阅读这篇： Python的文章 [Writing An Hadoop MapReduce Program In Python](https://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)    
文章类别： 这篇文章偏向于**实践教程**。

**点评:**  

1. 这篇文章开始就解释了自己写作动机：因为Hadoop的文档Python代码示例都是Jython为主的，想使用Jython不能提供的Python特性时就会遇到问题。那么这篇就是为了教学：为Hadoop写符合Python风格的代码。**看完这个Motavation，读者就会知道这个文章是不是自己需要的”菜谱“了，给阅读者减少了的时间浪费**。

2. 通过了目录(Table of Contents)，方便没有耐心的读者，直接跳转到自己他想要的部分；也能清晰看到文章的脉络。

3. **文章的教程部分，几乎是”手把手“的教了**。告诉读者要做什么--编写一个简单示例；要准备什么东西(Prerequisites)--要有Hadoop集群；分别完成map和reduce的代码; 测试正确性；最后在集群上运行代码；最后一部分对原先代码加以改进。

4. 文章最后一部分有改进代码的内容，为什么不放在前面呢？因为确实有些人Python水平有限，不一定能理解到，所以用简单的代码方便他们理解。**最后把这个改进作为甜点，给一些能看懂的”高级“编程人员**。


#### 值得学习的地方有：

1. 开头解释自己的这篇文档的动机，说明独特之处。
2. 引入目录
3. 教程尽可能的傻瓜式，把读者当傻子，这样读者不会有看不懂的地方
4. 先易后难，要保留”高级的“内容，给”高级的“读者

## 如何写项目的ＲeadMe <a name="read_me"></a>
#### 示例１
https://github.com/Velocidex/velociraptor

该项目是个很好的示例，观察它是如何组织的：

1. 首先一两句话简单地说明该项目(工具)的用途
2. Quick start, 引导用户快速上手，如何下载和安装
3. Running Velociraptor locally.　如何使用的问题，并且提供了截图示例
4. Building from source.　指引用户如何从源码编译．
5. Getting the latest version. 获取最新的版本
6. 获取帮助．

很明显，它遵循了一个原则：把重要的东西呈现在文档的前面．对于一个刚看到此项目的人，他最先必然关注的是该项目的用途，其次关注是如何快速使用这个工具，最后对于一些＂高级＂的开发者，他们会关注如何编译．最后才是其它的信息．

### 如何写项目的Wiki  <a name="wiki"></a>
Wiki相对于ReadMeWiki显然需要更为详尽, 提供详细的使用手册给项目使用者.

...待续，有空再补上

https://www.electricmonk.nl/log/2017/08/06/understanding-pythons-logging-module/
