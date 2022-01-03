---
published: true
title: 神奇口袋之计算机基础
layout: post
author: Ray Cao
category: Magic
hide: true
---  

### 中断

这篇论文[ A Problem of Program Execution Time Measurement](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103MeasureAppExeTime)论文讨论了程序执行时间测量的误差，误差的根源是因为无法保证测量的开头和结束时间点恰好就是中断的发生时间。在完美理想的情况下，除非中断是连续的，而非间接性的，它才能可能恰如其分的捕捉到事件的开头和结尾。但这就必然会导致程序永远不会被执行，所以这种连续中断的理想不应当成立的。另一方面，自然界似乎不存在真正完美连续的东西，即便是光也是由粒子构成的，这似乎又从哲学的角度否定了连续中断存在的可能性。

这篇论文[Choosing the Right Timer Interrupt Frequency on Linux](https://ruibing.org/essay/2021/02/01/Papers-Comment.html#20220103RightIF)则测试了各种不同的中断频率，以帮助我们选择最合适的频率。



