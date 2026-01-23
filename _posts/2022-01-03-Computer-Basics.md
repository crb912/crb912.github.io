---
published: true
title: 计算机基础
layout: post
author: Ray Cao
category: Magic
hide: true
---  

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
