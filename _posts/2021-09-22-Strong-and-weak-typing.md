---
published: true
title:  维基百科中关于类型强弱的错误
layout: post
author: RuiBing
category: programming
---  


维基百科中关于类型强弱的错误，具体页面：
https://en.wikipedia.org/wiki/Strong_and_weak_typing

引入错误的编辑： 
https://en.wikipedia.org/w/index.php?title=Strong_and_weak_typing&diff=1013358561&oldid=994579975

具体的错误表述是：
>
"Note that in dynamically typed languages, values have types, not variables".

上述这句话有严重的表述错误和误导倾向，因为值(values) 和 变量（variables）两者完全是不同的概念和实体，两者不可以混为一谈，这和语言是动态还是静态无关。这句话好像就是在说“中国人有头发，不是猫”，听的人很容易以为“西方人没头发”或者“西方人是猫”。在任何情况下中国人都不是猫，因此没必要把“风马牛不相及”的事物联系在一起。

我有两条表述：
1. 不分静态类型语言和动态类型语言，值和变量都有类型。
2. 不分静态类型语言和动态类型语言，值都不是变量。

## 对于1
在静态语言中，比如C++，它使用的任何实体都是具有类型的，包括值。值有类型。值在本质上都是一组二进制位，它的解释基于它的类型。如果一个值丢失了它的类型，那这个值本身就失去了其存在的意义，因为它可能有无限种可能的方式去解析。这就会造成值的多义性，这在计算机科学中是不可以接受的。类型赋予了值的生命力。比如对于不加修饰的值 `111011`，你能告诉我它是什么含义吗？不能，你必须要基于它的类型才能得到解释。

还是以C++为例：
```c
 int a = "c";
```
编译器会得到错误：`invalid conversion from ‘const char*’ to ‘int’`, 显然的，编译器会字符“c”这个值认为是const char*’的类型。
再比如：

```C
 int a{2.5};
```
同样会编译错误，`narrowing conversion of ‘2.5e+0’ from ‘double’ to ‘int’ inside { } [-Wnarrowing]`，2.5在C++是一个double类型。

---

上面我举出了静态语言的例子，至于动态语言，就以Python为例：
```shell
Python 3.8.10

>>> type(23)
<class 'int'>

```
可以看见，对于值（此处是数字23），Python的值同样是有类型的。变量的类型取决于每次赋的值；我们说“动态编程语言”，“动”的精髓在于此处，它不是一成不变的。

值是编程语言类型系统的基石。因为值有了类型，操作（赋值或者运算）的结果就有了类型，这样的类型在一层层操作中传递下去，构成了整个的类型系统。

我无法想象在一门高级编程语言中缺少类型系统，尤其是值。它是赋值语句重要的构成部分，而赋值，通常是计算机编程一切操作的开始。如果值没有类型，那就无法知道赋值的操作会成功还是失败，换句话说，一切赋值都将是成功的。这会导致一种灾难，我们无法保证计算机下一步程序每个语句执行后的正确性，很容易因为类型的缺失而计算出错。那为什么不用汇编语言呢。

## 对于2

关于“变量”这一个概念，我们可以一篇有影响力的讲义中找到，Strachey, Christopher 的“ Fundamental Concepts in Programming Languages ”，1967，他的讲义中写到：

>
One important characteristic of mathematics is our habit of using names for things. Curiously enough mathematicians tend to call these things ‘variables’ although their most important property is precisely that they do not vary. We tend to assume automatically that the symbol x in an expression such as 3x2 + 2x + 17 stands for the same thing (or has the same value) on each occasion it occurs. This is the most important consequence of referential transparency...

他在此处解释了变量这一概念的由来：**变量是事物的名字，是一个符号**。

名字是没有类型的，这显而易见。变量只是引入的一个符号，但是，**这个符号指代了事物本身，事物本身有类型的，所以变量就有了类型**。如何去理解这句话呢？我们说“小明”是一个人的名字，这个人他是一个人类，所以我们说“小明”是一个人类。当我们说“小明”的时候，我把这个名字与他本人紧紧联系在一起的，两者是等价的。

**变量是事物的名字，变量的类型是事物本身的类型，通常是在声明语句所指定的变量和变量类型; 对于不需要声明语句的动态语言，变量的类型基于值**。

在C++中，我们说，变量是一个命名的对象，对象就是上面我们说的事物。并不是所有的对象都有名字的（匿名的对象，如new出来的，临时对象），并不是所有的对象只有一个名字（变量别名）。C++的类型推断系统，auto关键字，它是把值的类型，推断成变量的类型上了。这样做对编译器来说完全合理，但是程序员在阅读代码的时候，他可能就不方便知道变量的类型了，好在许多IDE提供了这种便利。

变量的类型通常取决于我们的声明。如果声明的类型与值的类型不匹配，编译器尝试做隐式的类型转换。当然，你也可以禁止它这么做。上面我们说“通常”，因为当如果我们使用类型推断，那么变量的类型，将取决与值的类型。

## 为什么作者会有这种误解？

他显然的认为在动态编程语言中，变量是没有类型的。

因为在动态类型语言中，变量的类型是可以在运行时动态改变的，所以导致一些人认为变量没有类型。变量有类型，只是它的类型可改变，dynamic binding。但是在静态类型的语言中，变量的类型不可以改变, static binding。

Python 示例:
```
>>> a = 2
>>> print(type(a))
<class 'int'>
>>> a = 3.14
>>> print(type(a))
<class 'float'>

```
在C++中，变量的类型不会因为赋值而改变, 下面的写法都会导致错误。
```
int a = 3;
float a = 3.14;  // conflicting declaration ‘float a’：
int b = 2;
b = 3.14;  // type of b is int,  not double
```

不论是动态语言还是静态语言，值和变量都有类型！

2021-09