---
published: true
title:  理解CPU的Load和Store操作
layout: post
author: RuiBing
category: Science
---  


>我们能够面对问题。  
我们会侦破这起悬案的。   
因为我们能够理出头绪，找到办法。    
--赫尔克里·波洛，《东方快车谋杀案》(1934)

这篇文章可以算是《CS:APP》的一篇读书笔记。不过我觉得我思考问题的角度有所不同，同时也不需要读者对汇编和寄存器知识的掌握。

正文如下：


我们所有计算机程序的本质，是指令的执行过程，从根本上说，也就**是处理器的寄存器(Register)到内存之间的数据交互过程**。

这个交互，无非两个步骤:

- 读内存，也就是加载操作(load), 从内存读到寄存器
- 写内存，就就是存储操作(store)，从寄存器写入到内存

那么，了解处理器的寄存器(Register)到内存的数据交互过程，能帮助我们写出性能更好的代码。


## 两个结论

我们知道，CPU和内存之间存在速度差异。程序的指令，如果需要从CPU Register里面取数据，CPU只需要0 cycles(CPU周期); 如果需要直接访问内存，则需要几百个cycles。(另外，如果能从高速缓存取到目标数据，需要4-75个cycles）

一个非常之快的CPU(处理器)，却要和一个更慢的RAM(内存)交换数据。毫无疑问，如果直接交换数据，那么CPU会因为RAM太慢，拖慢自己的速度。

为了解决两者之间数据交换数据的速度差异，计算机科学家们了就引入了高速缓存(Cache)：让CPU和RAM之间使用多层级的缓存，避免两者直接交换数据，而是从临近的缓存区交换数据。我不打算再详细解释存储器的层次结构，因为它并不妨碍我把主题内容解释清楚。

无论是否有高速缓存的机制，我们都有理由得到**两个结论**:

- 结论1：CPU减少Load操作，会提到程序的性能
- 结论2：CPU减少Store操作，会提高程序的性能

不论是Load(读操作)，还是Store(写操作)，都是CPU和RAM之间交换数据的方式。**程序中CPU和RAM交换数据的次数越少，指令就执行越快。** 

因此，我们让自己的代码，更倾向于使用寄存器，而非频繁读写内存，就会带来程序执行性能的提升。程序员的代码也能控制读写寄存器还是内存？ 是的，你必须了解这背后的细微机制。

## 一段示例代码
直接享用代码，比文字解释要好的多。这段代码简单，即便你没有C语言的功底应该也能看懂。

```C
// 写入到dest，从src读入
void write_read(long *src, long *dst, long  n)
{
	long cnt = n;
	long val = 0;
	
	while (cnt) {
		//  写内存, 把val的变量值，写入到dst指针指向的内存空间
		*dst = val;       
		// 读内存, 读取src指针指向的内存空间，并且加1后保存到变量val   
		val = (*src) + 1;  
		cnt--;
	}
}
```
为了让这段程序更明显的演变，我们构造它们的函数参数, 让它能够执行。在此之前，请留意上面我给的两行代码注释。

假定传递一个只有两个元素的数组a[-10,17]，函数调用方式如下:

示例1

```C
write_read(&a[0], &a[1]),3);
```
那么，模拟这个while循环的运行：

| | 初始状态| 第一次迭代| 第二次迭代 | 第三次迭代 |
---|---| ---| --- | ---|
变量 cnt | 3 | 2 | 1 | 0|
数组A | (-10, 17)| (-10, 0)| (-10,-9)| (-10,-9)|
变量val | 0 | -9 | -9 | -9 | -9|

注意观察: 

第三次迭代和第二次迭代相比，不会对数组a的数据的产生变动了。

那么从CPU设计者的角度出发，那我们就容易就有一个想法了：

**既然代码不会真正的改变内存，那么第2和3次迭代的过程就没有必要去写内存（不执行Store操作），也就是我们上面的结论2**.  如果不去执行这个Store操作, 就实现了代码的性能优化。

实际上，也确实如此。设计CPU的人非常聪明，他们想出的办法是：**使用存储单元(Store Unit)**。存储单元每个缓冲区，都包括了将要写入到内存的数据，数据结构由内存地址和具体的数据构成。我们用一对括号表示(内存地址，数据)。

![存储单元](https://i.loli.net/2021/02/24/3Ju5nvYOGoZhABb.png  "https://github.com/crb912/crb912.github.io/blob/master/my_private/image/IMG_2021-02-24%2019-44-37.png?raw=true")

存储单元由内存地址+数据构成，这很好理解，举个例子：我们写内存的操作，就如同去银行的ATM去存款。你的银行卡芯片里有你的银行卡号，它就相当一个一个地址；你带的现金，就是要存储的数据。要想写入成功，银行卡和现金两者缺一不可。(即便你无卡存款，也需要提供银行卡号)

既然引入了存储单元，我们Load操作(加载操作)，是不是也可以直接从存储单元去读呢？完全没有问题。**如果Load的的内存地址，刚好就在存储单元里找到，那么也就是说，直接从里面拿数据就可以了。这样就减少了一次内存的读取，减少了Load操作，也就是我们上面的结论1**。如果不去执行这个Load操作, 就实现了代码的性能优化。

这么一来，CPU设计了存储单元之后，我们的代码就有了性能的提升。

既然CPU的工程师已经帮我们实现了这种精妙的优化，那还需要程序员优化代码做什么。这种想法完全错误，接下来我们看另一个示例。

## 示例2--写读相关

如果我们对write_read函数使用另一个组参数。

注意，调用该函数的前两个参数的引用的都是a[0], 如下：

```C
write_read(&a[0], &a[0]),3);
```
那么我们继续模拟代码的行为, 得到:


| | 初始状态| 第一次迭代| 第二次迭代 | 第三次迭代 |
---|---| ---| --- | ---|
变量 cnt | 3 | 2 | 1 | 0|
数组A | (-10, 17)| (0, 17)| (1,17)| (2,17)|
变量val | 0 | 1 | 2 |3| 

显然地，每次迭代都会改变数组a[0]的值，每次都会对内存进行先写后读。

CPU没办法对这种进行情况就行优化，而且因为指针*dest和*src指向同一个内存位置。读写的地址相同，所以每次读的操作，都必须等待上一条写(Load操作)完成。**这种特殊的现象，我们称之为写/读相关(write/read dependency)**。**对于完全支持流水线(pipeline)技术的CPU而言，这种等待, 会造成浪费时间。**而前面的示例1就没有这种等待，它们的指令可以进入流水线并行。

CPU浪费时间，其后果就是程序性能的损失。所以作为程序员的目标，在我们的编程实践中，就要避免让代码出现这种写读相关。

下面拿一段“乍一看没什么问题的”代码作为例子，加以改变和修正。

## 修正一段代码

学以致用，了解这个既然是为了写出性能更好的代码，不妨实践一下。

欣赏一段不好的代码：

```C
void psum1(float a[], float p [], long n)
{
	long i;
	p[0] = a[0];
	for (i = 1; i < n; i++)
		p[i] = p[i-1]+a[i];
}
```

我们容易发现上述代码中：

for循环内用反复地从内存中去读p[i]的值。

为了避免写读相关性，如何修正它呢？

解决的办法--新增局部变量，修正后的代码：

```C
{
	long i;
	p[0] = a[0];
	float last_val = p[0] = a[0];
	for (i = 1; i < n; i++)
		val =last_val+a[i];
		p[i] = val;
		last_val = val;	
}
```
这样在上一次循环中记忆了p[i]的值，保存在last_val变量中，留给下个迭代使用，避免了Load操作。局部变量使用了寄存器，而不会去读内存，提高性能。

写到这里，这篇也就结束了。与CS:APP叙述方式不同的是，作者是从程序员的角度思考的，我是CPU设计者的角度思考的。两者之间有一些差异，而且我自认为解析的透彻和通俗易懂。我原先是打算加入流水线和汇编代码的这部分，但看来没有这种必要性，而且我的时间有限，不能面面俱到。


