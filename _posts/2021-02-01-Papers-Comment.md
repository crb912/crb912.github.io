---
published: true
title: 论文点评
layout: post
author: Eureka Bing
category: Essay
hide: true
---  

## 对软件研究的反思

- 论文：Reflections on Software Research,Communications of the ACM 27,8,(1984)
- 作者: Dennis Ritchie.
- 链接: http://www.jdl.ac.cn/turing/pdf/p758-ritchie.pdf

谦逊的大师！他的这篇论文虽然简短却见解深刻，他基于(过去)软件研究的反思，提出了很多看法：1. Unix的成功不仅只是技术上的，也是社会学的，比如小型机出现、Multics，有许多热情的用户积极投入。2. 计算机研究由什么组成？ 应该去发明新事物！尽管他当时一穷二白，仍然得到了管理层的支持鼓励。尽管许多想法不能说服对别人有用，但还是要有这样的想法和坚持。 3. 如今计算机最大的危险是过度相关，全世界都在关注计算机，但商业主导会损害良好的研究。一些聪明的老师不教学生了，他们去了初创公司。4. 另一个危险是商业压力导致个人会追求高薪的投资（智力或金融上的），导致优秀的思想家从创新转移到迎合当前的时代潮流。另一方面，小企业因为依赖市场就会把注意力放在短期目标。但是像Unix，它是十年磨一剑的作品，良好的科学研究需要时间和耐心,而不是饕餮快餐。总结核心的思想是科学研究要创新，发明新事物，即便不能被人理解。其次，不可以被商业利益裹住脚，一定要有付出时间和足够的耐心。

其中提到有意思的事情： 大型计算机公司很少有有趣的软件系统；他们被锁定在现有的世界中。比如IBM，近年来也几乎再没有产生任何东西。

---

## 在Linux上选择合适的计时器中断频率 <a name="20220103RightIF"></a>

- 论文：Choosing the Right Timer Interrupt Frequency on Linux
- 作者：Mulyadi Santosa
- 链接：http://repository.gunadarma.ac.id/313/1/Choosing The Right Timer_UG.pdf

**中断基本原理**：主板上有一块可编程芯片产生中断，中断信号通过中断控制器发送给CPU。这就如同上下课打铃，没有提醒CPU就失去时间概念。由于该芯片有多种（PIT，CPU Local Timer等等）系统开机时，它会检测这些计时器每一个是否存在。CPU收到中断时，它会：1. 更新系统时间 2.运行过期计时器(唤醒超时的sleep, I/O select()) 3. 比较时间片，重调度 4. 更新系统负载

中断过快会导致CPU性能损失，中断过慢会牺牲用户体验，需平衡两者来获得最大的CPU能力，如何选择合适的中断频率？文章的实验设计值得学习，它分别从CPU密集型程序、键盘输入、多媒体输入，多媒体输出、虚拟机，5个不同类型的软件进行测试时钟中断频率，结论有一定的说服力。其中最有趣的部分在于测定键盘输入的响应时间。事件如下：按键 -> 引起中断 -> 接受到 scan code -> 新数据进入终端驱动，唤醒等待的进程 -> 该进程接受到此次键击。要计算的时间差发生在内核接受到数据时和应用软件被唤醒，因此要从内核空间和用户空间分别捕捉。它的做法是：当键码被解释完全，内核空间下记录一次时间戳计数寄存器的TSC (Time Stamp Counter) number；当控制台程序收到键击时，再记录一次 TSC，不断重复多次。

该论文提到的技巧：1. CPU密集程序用了三层嵌套循环对两随机数做除法。 2. 读取TSC寄存器（可认为是一个高精度的时钟），用到`rdtsc`汇编指令。3.接管内核的键击，修改`drivers/char/keyborad.c.orig`。

论文结论是：“In general case, especially when in doubt, simply use HZ=250 for your kernel setting. It fits fairly nice for multimedia work, officeproductivity, gaming and other end-users workloads. This is also the default setting in latest Linux kernel releases and adopted by major distributions, so you won’t need to change anything.”

---

## 一个程序执行时间测量问题  <a name="20220103MeasureAppExeTime"></a>

- 论文: A Problem of Program Execution Time Measurement
- 作者: Hajdukovi Miroslav, Zorica Suvajdžin, Zark Zivanov, Edin Hodži
- 期刊: Novi Sad J. Math, 33,1, (2003)
- 链接: https://mat.ub.edu/EMIS/journals/NSJOM/Papers/33_1/nsjom_33_1_067_073.pdf

详细讨论了程序执行测量的误差来源的几种情况，并且用数学方法做了更接近的计算。该论文没有实际的应用价值，只是提供了严谨的分析思路。

误差无法消除，这是因为测量执行时间依赖计时器中断。然而每次测量都是等待中断发生之后。世界上并不存在真正意义上的“同时”，永远会有误差，除非这种“同时”是由偶然性驱使。凡是人为预先的设定，那么真正的“同时”就不存在。