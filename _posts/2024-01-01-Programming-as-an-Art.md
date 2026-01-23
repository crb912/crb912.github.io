---
published: true
title: 计算机编程
layout: post
author: RuiBing
category: programming
---

作为艺术的计算机编程

## 编程的原则

### “事不过三”原则

if you need more than 3 levels of indentation, you're screwed anyway, and should fix your program -- Linus Torvalds, [Linux kernel coding style](https://github.com/torvalds/linux/blob/master/Documentation/process/coding-style.rst).

如果你需要超过3级的缩进，那么你已经把事情搞砸了。应该修复你的程序。

多级指针：Linux Kernel源代码只有20多处 **两级指针，没有任何的***三级指针。

### C++的编程习惯

#### 1. 首要原则
- Rules with no enforcement are unmanageable for large code bases. 没有强制执行的规则会导致大型代码库无法管理。
- 牢记SRP(single responsibility principle)设计原则。比如函数要尽可能简单。

#### 2. 函数
- 尽量不要把bool值作为函数参数，而应该是使用枚举类。参见：[千万不要把 BOOL设计成函数参数](https://coolshell.cn/articles/5444.html)
- 避免使用带有太多内置类型的函数，如: draw_line(int, int, int, int);  // obscure。这种代码不容易理解参数类型的含义。

#### 3. 类

- friend class有利于代码的模块化，它也可用于特定的成员函数。如Node和LinkedList::search()
- 类的设计要合理，make sense, 起码要能从类的名字可推测它可能提供的操作。
- 能用const的地方一定要用const，这会把程序员的意图告诉别人。
- 使用标准库，而不是模仿标准库。( mimics the standard library)
- explicit关键字可以避免代码隐式转换。
- Use concrete classes to represent simple concepts;
- Prefer concerete classes over class hierarchies for performance-critical components; 高性能组件尽可能用具体类
- Make a function a member only if it needs direct access to the representation of a class; 仅当需要直接访问类的表示时，才使用成员函数。
- Declare a member function that does not modify the state of its object `const`;
- If a class is a container, give it an initializer-list constructor;
- An abstract class typically doesn’t need a constructor;
- A class with a virtual function should have a virtual destructor;
- 由于抽象类灵活的特性，在派生类的destructor函数要保证资源能被释放，这太重要了。

#### 4.其他

- 避免裸的Union。因为Union可以规避C++的类型系统，导致不易察觉的bug，因此要避免这样使用。我们可以单独维护一个type field，使它成为一个tagged union；或者，使用C++标准库的variant。
- 如果你的代码有太多cast，说明你已经搞砸了。
- Use `dynamic_cast` where class hierarchy navigation is unavoidable; 当类层次漫游行为不可避免时，使用动态强制转换。 如果无法转换向目标类时，想要它报错，就用`dynamic_cast` 作用于引用类型；不希望报错就，就作用于指针类型。
- Use `unique_ptr` or `shared_ptr` to avoid forgetting to delete objects created using new;
- new出来的裸指针，直接从函数返回是很危险的。最好用标准库的`unique_ptr`,例如`return unique_ptr<Shape>{new Circle{p,r}};`

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


## 解释说明

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

## 读计算机科学的经典

过去一直有一个声音，让我们去读经典，读《红楼梦》， 读巴尔扎克，读莎士比亚，读《荷马史诗》，读《论语》... **为什么要读经典**? 

因为真正的美，从古至今都没有改变，改变的只是它表达美的形式和表现方式。就如,即便几千年过去了，我们仍然会觉得万里长城很美，秦兵马俑很美， 比萨斜塔很美，蒙娜丽莎很美，梵高的向日葵很美...美的事物，人们很容易得到共识。

我把这样的想法也放在了编程领域，我们该去读计算机科学的经典。读计算机科学在早先创建的时候，那些先驱者的思想，以及后来不断出现的经典。

为什么要做这件事情？因为一方面计算机的技术越来越纷繁和学科细化；另一方面，越来越多的人痴迷于科学技术产生的工具，甚至有些人痴迷于一种编程语言。正如Bjarne Stroustrup在[访谈](https://www.youtube.com/watch?v=NvWTnIoQZj4)中提到：

> ”Nobody should call themselves a professional if they only knew one language.（没有人应该把自己视为一个专家，如果他们只知道一门编程语言）“

由此，新一代的人们开始忽略了计算机科学的本质，忘掉了那些简单而朴素的真理。**只有去读经典，才能从这越来复杂和庞大的学科中，把握它最美、最精华的思想**。

我打算花两年的时间，把这些经典的论文和书籍看一遍，我要回到那个简单朴素的思想起点。


## 计算机基础

### 3.14 中断

1.这个[MCU: Interrupts and Timers](https://students.iitk.ac.in/eclub/assets/lectures/techkriti12/timers_and_interrupts.pdf)幻灯片，开头以没有中断的灯泡切换的程序为例，当针脚的电平改变时无法确定程序刚好能捕捉到这一变化。因为默认的前提是要从时间上保证事件发生的先后顺序，所以需要一个“钟表”保证同步的顺序，这就是中断存在的本质意义。作者讲到中断的自增**timer寄存器**，它是中断设计的核心。从0增长到最大再回到0，过程独立于CPU。Timer产生两种中断：`OVERFLOW interrupt`和`COMPARE MATCH interrupt`，后者依赖是一个OCR(输出比较寄存器)，因为它是可编程的，所以我们的操作系统可以指定中断频率。当OCR和timer数值相等，则触发中断。

![interrupt](https://github.com/crb912/crb912.github.io/blob/master/my_private/image/20220104interrupt.png?raw=true)

Timer三种模式：a.正常模式，保持自增。b.**CTC** (Clear Timer on Compare) Mode 比较成功时置零 c.**PWM**(脉宽调制)模式，利用比较匹配把整个Timer寄存器的数据区间“切分”成两段，分别使用不同电平，由此产生不同宽度脉冲。这一模式在嵌入式上可能得到应用。

![PWM](https://github.com/crb912/crb912.github.io/blob/master/my_private/image/20220104interrupt_PWM.png?raw=true)

[CodeVisionAVR](http://www.hpinfotech.ro/index.html)可给AVR架构单片机芯片编程。

2.这篇[ A Problem of Program Execution Time Measurement](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103MeasureAppExeTime)论文讨论了程序执行时间测量的误差，误差的根源是因为无法保证测量的开头和结束时间点恰好就是中断的发生时间。在完美理想的情况下，除非中断是连续的，而非间接性的，它才能可能恰如其分的捕捉到事件的开头和结尾。但这就必然会导致程序永远不会被执行，所以这种连续中断的理想不应当成立的。另一方面，自然界不存在真正完美连续的东西，即便是光也是由粒子构成的，这从哲学的角度否定了连续中断存在的可能性。

3.这篇论文[Choosing the Right Timer Interrupt Frequency on Linux](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103RightIF)则测试了各种不同的中断频率，以帮助我们选择最合适的频率。我在这篇[文章](https://ruibing.org/essay/2021/02/01/Concise-Sci-Tech-Commnets.html#20211007WinIF)分别提了Windows和Linux 默认的中断频率数值。

4.CS:APP Beta节选章节 *Measuring Program Execution Time*，盗版Beta上内容，不知作者为什么在正版上删了很棒的这一章节。**CPU在微观上以ns作为事件的时间尺度，而OS在宏观上以ms作为时间尺度。前者基于CPU的时钟自增，后者基于独立的timer**. 因此是两种不同的测量方法。由于上下文切换开销大，因此需消除。

一些事件的时间：视频播放，显卡的刷新每33ms；磁盘启动传输大约10ms；OS的任务切换是5-20ms;**操作系统管理事件在5千-2万的时钟周期，这个范围的时间尺度是微秒(μs)**。

**总结**：

1.偶然与必然的结合  

- 中断信号可以是硬件偶然性事件发出的，比如：keystrokes, disk operations, newwork activity，鼠标移动，这些设备向中断控制器Timer发送[IRQ中断请求](https://en.wikipedia.org/wiki/Interrupt_request_(PC_architecture))，接着OS负责切换和调度相应的进程，让用户期望优先的进程完成中断的处理。
- 中断信号也可以由必然性事件产生，因为单有偶然性是不够的，仍需要一个固定周期的时钟来产生中断。它让OS可以“公平”调度多个进程。

2.硬件和软件的统一

所有教材在讲中断时，几乎都没有将硬件和软件两者很好的结合到一起。嵌入式开发则过分关心其电气特性，软件开发则又集中在其软件的抽象层。中断，它既工作在硬件层，又工作在软件层。如果只是关注其中一个特性，必定无法窥见她面纱之下的真实原貌。因此想要了解中断时，就必须要从这两个方面同时把握。从硬件方面，要知道这些硬件存在，这里[Timer Interrupt Sources](https://wiki.osdev.org/Timer_Interrupt_Sources)提到了各类的时钟，包括中断时钟PIT、HPET等，其中APIC和TSC是每个CPU都有一个，每个时钟又有不同的区别（Fixed Frequency IRQ，IRQ on terminal count）。从软件方面，理解中断带来的好处。就完成了它的学习。



---

### 一些论文
[CSCI E-192](https://canvas.harvard.edu/courses/34992/assignments/syllabus) 课程提供了很好的参考，他们列出的Paper主要在这里：[master list.](https://canvas.harvard.edu/courses/34992/assignments/syllabus)

我觉得其中每一篇的都值得一读，所以就不一一列举了。实在不喜欢的可以略读，但是也该能把握它的思想。

### 一些书单

来自Reddit，更多的讨论在链接这里: [Is there a list of the canonical introductory textbooks covering the major branches of computer science? ](https://np.reddit.com/r/compsci/comments/gprp0/is_there_a_list_of_the_canonical_introductory/c1pcqe5/)  
我只关心我对其中感兴趣的书，人的一生时间有限，我不可以什么书都去读。不管它写的再好，如果我对其不感兴趣也是徒劳。

- **SICP** (Structure and Interpretation of Computer Programs (Abelson & Sussman))
- Formal Language: A Practical Introduction by Webber
- Abstract and Concrete Categories: The Joy of Cats by Adamek et al
- Fundamentals of Computer Graphics by Shirley and Marschner
- Distributed Systems: Concepts and Design by Dollimore et al
- **Introduction to Functional Programming using Haskell **by Bird and Wadler
- **ML for the Working Programmer by Paulson**
- **How to Design Programs**（Felleisen等人）
-  **Concepts, Techniques, and Models of Computer Programming **(Van Roy & Haridi

#### 关于编译器

Reddit上有关于讨论编译器书单的一个话题，[Recommendations for books on compilers](https://www.reddit.com/r/programming/comments/3tgryd/recommendations_for_books_on_compilers/)。其中sanxiyn评论到：
GCC wiki recommends [a list of compiler books](https://gcc.gnu.org/wiki/ListOfCompilerBooks).

打开之后，我只选了一本： **Appel. Modern Compiler implementation in ML**


关于编译器书籍的评论，我想 Vladimir N. Makarov的话语为我指明了学习和阅读的方向：
> If you don't want to be compiler savvy but want to understand the compiler, I'd recommend Appel's, Cooper's, Morgan's book in the same priority.

我现在还不清楚自己想不想精通编译器，所以先读这些基础的书，如果兴趣越多就读更多。

### 关于我的兴趣

**到现在我还不知道我的兴趣聚焦于哪个点，所以我尽可能了解更多，学习更多**。不过我知道我喜欢编程语言中有意思的概念，有意思的想法。**我想我的好奇心会帮助我找到最终的答案，让我的工作有一个核心的关注点。**

我对于计算机中一些个人口味的不喜欢是显而易见的，比如：计算机图形学，密码学，我对这些基本上没有兴趣。我在从前工作了解过关于RSA密钥的算法，只是为了满足老板的商业需要进行学习的。比如我对LSM的数据结构有兴趣，所以我觉得自己**关注的方向还是编程语言方向，关心数据结构和算法**。我对Knuth的CWEB编程语言很感兴趣，它将是我这段时间忙完之后第一件事情。

### 关于编程语言工程实践的书

我不太喜欢看这类书，比如一些人推荐《代码大全》，重构这类的书。因为正如我前面所说的观念： **美是一种共识**。

只要追求美，最终就会够殊途同归。如果年轻时代码写的不够美，只是因为对美的理解不够深刻，只要保持这种追求，最终就会自己独立的找到。如果你觉得能找到一种和一些方法就能够写出美的代码，我觉得这是误解和想当然。只要有了自己对美的理解，自然就能做出美的东西，而不需要任何专注所谓的”技巧“。有个词语叫”洗心革面“，只要”洗了心“，人的面貌自然就新了，而不需要”革面“。不能做表面工作侧重形式，而是专注于内在。


## 从示例中学习编程

pass

---

## 编程碎碎念
### zendesk服务

这个服务很有意思，致力于简化客户和公司之间的客户解决方案。这是一个在细分领域成功创业的想法，特别棒的idea。在未来当互联网的基础设置被巨头垄断时，初创公司想要在互联网分一杯羹，他们就要从细分领域着眼。认真考虑用户的需求，考虑公司的需求，尤其这种看起来毫不起眼的。

### 对软件更新过快的抱怨

我的一些软件总是频繁提示更新，这么多开发者和企业热衷于快速迭代更新。让我感到厌恶。最好的软件应该只发布一次，并且最好永远不再更新。我对那些新功能和特性完全没有感兴趣，我只是需要一个足够稳定的版本，没有bug，辅助完成我的工作。我相信80%的用户只用了软件20%的功能，然而，80%的功能特性不到20%的用户在使用。

为什么要试图囊括所有的需求呢？我能理解这背后的原因：开发者和产品经理拿了薪水，他们总是在不停寻找新的特性加入，以证明自己的工作价值以免被炒鱿鱼，老板也很乐意自己的软件在变得强大。然而，用户真的需要那些新特性吗？这是一个值得他们停下来去思考的问题。起码我希望自己的这些软件可以停止更新。或者提供两个不同更新频率的版本：低更新频率的「稳定版」和高频率更新的「开发版」，来满足两种风格不同的用户。

2022-01-02

---

### 一个人应该学习几门编程语言?

Bjarne Stroustrup(Interview): 
 
 First of all, **nobody should call themselves a professional if they only knew one language.**” **And five is a good number of languages to know reasonably well**. And then you’ll know a bunch, just because you’re interested, because you’ve read about them, because you’ve written a couple of little programs. But five isn’t a bad number. Some of them books between three and seven. Let’s see, well my list is going to be sort of uninteresting because it’s going to be the list of languages that are best known and useful, I’m afraid. Let’s see, C++,of course; Java; maybe Python for mainline work. And if you know those, you can’t help knowing sort of a little bit about Ruby and JavaScript, you can’t help knowing C because that’s what fills out the domain and of course C#. But again, these languages create a cluster so that if you knew either five of the ones that I said, you would actually know the others. I haven’t cheated with the numbers. I rounded out a design space. It would be nice beyond that to know something quite weird outside it just to have an experience, pick one of the functional languages, for instance, that’s good to keep your head spinning a bit when it needs to. I don’t have any favorites in that field. There’s enough of them. And, I don’t know, if you’re interested in high-performance numerical computation, you have to look at one of the languages there, but for most people that’s just esoteric.
 
 Peter Norvig:   
 
 **Learn at least a half dozen programming languages**. Include one language that emphasizes class abstractions (like Java or C++), one that emphasizes functional abstraction (like Lisp or ML or Haskell), one that supports syntactic abstraction (like Lisp), one that supports declarative specifications (like Prolog or C++ templates), and one that emphasizes parallelism (like Clojure or Go).
 
 一门编程语言就只是一门编程语言。它和你是不是一个优秀的程序员没有关系，它只是语法。

---

### 评论英特尔的ControlFlag开源库，以及AI。

[AI 帮代码找 Bug，英特尔开源机器编程工具 ControlFlag](https://m.ithome.com/html/582002.htm)

如果一个程序员对自己写的代码都没了自信，还要指望AI找出Bug，这几乎是一种智商上的羞辱。要是堕落到这种境地，还写什么代码，不如早点回家种地。这个库只是英特尔的员工无聊开发出来的娱乐玩具，如果真有程序员当了真，那就有点可笑了。

二十一世纪的谎言之一就是AI。人脑绝不是任何机器、集成电路和算法能模拟出来的。“人工”永远不会有“智能”，这纯粹是一个哲学问题，而不是一个技术问题。要真是到了哪天，我们的程序需要AI去写，我们的书籍需要AI创作，我们的电影动画需要AI去设计，那么，人类离灭亡就不远了。

2021-10-22

---

### 评网页设计

对于网页设计，最好的设计就是没有其它任何无用的设计。

比如[cppreference.com](https://en.cppreference.com/w/cpp/language/modules) 这个网站，简洁的排版，突出主要的功能，广告也很少。类似的还有[filezilla](https://filezilla-project.org/)。这些网页的功能共同特点是一点也不花哨，该有的功能全都有。

现在的网站大都各种花里胡哨的，看起来炫酷，实际没有什么屁用。用户需要从网页上获取有用的信息，而不是被那些奇怪炫酷的东西吸引注意力。我不知道人们把注意力放在让网站变得更“炫酷”的意义在哪里，就好像青春期的小屁孩剪了个“爆炸”的发型。一切毫无意义的交互动作、过度的渲染，UI设计，无聊的横幅和图标，全是狗屎。

人们就像喜鹊一样，喜欢闪亮的东西。但时髦的东西并不一定是最好的。reddit有个评论写到([链接](https://www.reddit.com/r/webdev/comments/1rdjri/comment/cdmhxtj/?utm_source=share&utm_medium=web2x&context=3)): “我们有一个市场营销天才，他把所有这些愚蠢的东西放在我们的网站上，我们的客户群扩大了一倍。我不明白，但这是商人喜欢的”。

2021-10-20

### 谈科学的门槛

科学的门槛越来越高了，以后只会是少数人的专利。在阿基米德时代，牛顿时代，你有一只笔、一些自制的“破铜烂铁”，有敏锐的洞察力和好奇心，就能做科学实验了。比如像孟德尔，种几亩豌豆也是正经的科学研究。

如今的科学，普通人要做实验需要各种仪器，没有足够的经费几乎是不可能的。普通人，与科学大院两者之间的壁垒只会越来越厚，随着学科发展，现代人掌握的知识要越来越多，而对未来人类的学习能力要求更高。我们还能回到过去那些不需要学历就能从事科学研究的时代吗？恐怕很难。

而计算机，我认为它仍然是一门新兴的学科，它的发展历史不到100年，依然有长足的潜力。我不知道什么是计算机未来真正正确的发展方向，我还在寻找。我希望自己走在正确的方向，我希望将这门科学在将来会更加完善，更加有趣。也许Donald Knuth会指引我，希望可以从他的著作中得到一些他的智慧。

2021-10-12

### 解释<薛定谔的猫>

薛定谔的猫，这个故事其实已经很形象了，但是仍有许多人不能理解它想表达的含义。它想表达的是思想是：“单个的微观事件是无法预测和准确测量的”。

假如你是一个微观粒子，那么你此时在家可能是在睡觉，或是在玩手机，或用电脑，或者在洗手间上厕所。但是不论你在做什么，我都无法知道。如果我想要知道，我就要闯入你的屋子，观察你在做什么。问题是一旦我闯入你家屋子，假如你在蹲马桶，可能就会立马提起裤子了；假如你在睡觉，就因此被我吵醒了。你会因为我的观测改变自己的行为，那么导致我观测的结果“并不准确”。

这就是薛定谔的猫，你永远不知道那只猫是死是活，也不知道它在做什么。这是量子理论最基本的概念。

2021-10-12

### 评论R语言

R语言的特点很明显: 数据类型足够丰富，但数据的操作很笨。如同去一家食物丰富的自助餐厅，但是老板却不提供刀叉，又不给盘子和火锅，到头来全靠手抓才能吃到嘴里。明显不如同样作为脚本语言和可用于科学计算的Python灵活。

数据灵活，但操作不灵活，在执行某些一连串操作时就会费劲——而不得不求助第三方包以快速解决问题。这是一门看起来简单的语言，但是隐藏的坑多，需要一些练习和技巧以后才能得心应手。相比之下，Python看起来简单，用起来也简单。

2021-10-11

### Window 时钟中断   <a name="20211007WinIF"></a>

[microsoft: Interrupt Time](https://docs.microsoft.com/en-us/windows/win32/sysinfo/interrupt-time)中断时间仅100纳秒，这个应该不是操作系统调度的Timer Interrupts。因为windows是闭源的，因此无法知道更多。[这里](https://kb.vmware.com/s/article/1005802)有人提到Window是64HZ或100HZ，那么应该是合理的值了。而Linux系统通常是250HZ(4ms)，[Philip的回答](https://unix.stackexchange.com/a/549912)提到了Jiffies值历史上先是100HZ(10ms),后来又改成了1000HZ(1ms)。

2021-10-07

### 穿孔纸带很酷！

[Programmers Deal With the Legacy of Paper Tape Every Day](https://medium.com/@pragdave/weird-programming-facts-1-3fe47d82a7fd)

1960 年代初期，编写计算机程序的方式很有意思，使用穿孔纸带或者穿孔卡片。以纸带为例，上面3孔下面5孔，因为孔向一侧偏移就不会小不小心让纸带倒置。纸带中间有链轮孔，用于将它拉过阅读器。最下面的孔是奇偶校验位，因此从倒数第二位开始读起。有孔的地方读为1，没孔的地方读为0。

那么，图中的第一个字符是读作(1010100), 对应英文字符T。 第二字符读作（1101000)，对应字符h。如此，就是一整个英文“This is PaperTaper \r\n”。对于\r\n字符的出现，它是为了标识代码行的结束。因为穿孔卡片一张卡片就是一行，而穿孔纸带是连续的，因此它要采用这样特殊方式标记一行结束。70年代的Unix开发者认为\r是多余的，因此如今经常只用\n作为结束。

 在纸带上输入大量空格是一种浪费，因此tab符号就产生了，它表示告诉纸带的阅读机器直接跳到下一列编号（跳过8个字符）。如今物理键盘上的Tab已经不是当初的含义了，通常只是等同输入2或者4个空格。 换页符\f含义是跳到下一页的顶部，而垂直制表符\v会使页面向下跳到预设行列表中的下一页。今天，我们通常将这些字符视为简单的空格。BEL 字符让打字机内敲响个铃铛。NUL 字符则是因为纸带送入阅读器时，很难对齐它，前一排或前两排可能会损坏，因此前面的视为空白字符NUL。DEL字符比较实用，比如在纸带上输入了 500 行代码，就在代码即将完成时打错了一个字符。把纸带剪断不是一个好办法，那么直接在下个孔打满8个孔，表示逻辑上删除前一个字符。这也是为什么DEL字符总是ASCII码表的最后一个！它在纸带很明显。

2021-10-06

### 过早优化不总是万恶之源

Donald Knuth博士曾说过,"premature optimization is the root of all evil"(翻译: 过早优化是万恶之源). 这句话成了很多程序员写出糟糕代码的借口, 并且他们从来不试图优化自己的代码. 但是Knuth博士这句话的下半句很少有人知道: " Yet we should not pass up our opportunities in that critical 3%"(而对于影响性能关键的 3%代码，我们则不能放弃优化的机会). 我在工作中就遇到过这样的事情, 在尝试修改少量的核心的代码, 其程序性能提升1~3倍. 如果放弃这样的优化机会, 将是很大的损失.


