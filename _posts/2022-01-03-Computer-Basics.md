---
published: true
title: 神奇口袋之计算机基础
layout: post
author: Ray Cao
category: Magic
hide: true
---  

### 中断

1.这篇论文[ A Problem of Program Execution Time Measurement](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103MeasureAppExeTime)论文讨论了程序执行时间测量的误差，误差的根源是因为无法保证测量的开头和结束时间点恰好就是中断的发生时间。在完美理想的情况下，除非中断是连续的，而非间接性的，它才能可能恰如其分的捕捉到事件的开头和结尾。但这就必然会导致程序永远不会被执行，所以这种连续中断的理想不应当成立的。另一方面，自然界似乎不存在真正完美连续的东西，即便是光也是由粒子构成的，这似乎又从哲学的角度否定了连续中断存在的可能性。

2.这篇论文[Choosing the Right Timer Interrupt Frequency on Linux](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103RightIF)则测试了各种不同的中断频率，以帮助我们选择最合适的频率。

3.这个[MCU: Interrupts and Timers](https://students.iitk.ac.in/eclub/assets/lectures/techkriti12/timers_and_interrupts.pdf)幻灯片，开头以没有中断的灯泡切换的程序为例，当针脚的电平改变时程序无法确定能捕捉到这一变化，因此要先确保能够捕捉到事件，再进行逻辑比较。中断是CPU实现任务调度和恢复的基础，没有它，CPU失去时间概念。

后面作者讲解了中断的自增**timer寄存器**，它是中断设计的核心。当它从0增长到最大值，然后继续从0重复，过程独立于CPU。Timer产生两种中断：`OVERFLOW interrupt`和`COMPARE MATCH interrupt`，后者依赖是一个OCR(输出比较寄存器)，因为它是可编程的，所以我们的操作系统可以指定中断频率。当OCR和timer数值相等，则触发中断。

![interrupt](https://github.com/crb912/crb912.github.io/blob/master/my_private/image/20220104interrupt.png?raw=true)

Timer三种模式：a.正常模式，保持自增。b.CTC (Clear Timer on Compare) Mode 比较成功时置零 c.PWM(脉宽调制)模式，利用比较匹配把整个Timer寄存器的数据划分成两段，两段分别使用不同电平，就由此产生了不同宽度的脉冲。

