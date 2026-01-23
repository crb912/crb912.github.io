---
published: true
title: 一些科学与技术问题的简评
layout: post
author: Ray Cao
category: Essay
hide: true
---  

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

### 关于未来人类的猜想

1. 食物浓缩成营养液, 不需要咀嚼，人肠胃消化的负担减少，肠胃功能退化。
2. 基因改造技术，出现了各种形态的人，比如有的人长着蜻蜓一样的翅膀。在未来，基因改造技术成了个人的自由，就像现代人整容一样。但是底线是：所有一切的改造都只能针对体细胞，不允许这些基因扩展到人类的基因组里。
3. 各种信息流，导致脑容量更大。
4. 人类的肌肉越来越少，因为不再需要从事繁重的肌肉。
5. 先天性疾病种类和发生率降低，因为产前诊断能发现的问题越来越多了。
6. 许多人都不愿意生育。政府为了维持人口，将健康的精子和卵子在体外培养成受精卵，体外从胚胎发育成新生儿。政府有同一的育婴机构，新生儿将从那里被政府养大，也可以申请领养。
7. 没有学科老师。因为机器人完全胜任了这一工作，它们在讲授知识的时候远比人还要好，也更科学。
8. 毕业后的学生，将可以根据自己的兴趣进入相关的机构、公司、研究所工作。