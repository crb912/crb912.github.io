---
published: true
title:  C++的左值和右值
layout: post
author: RuiBing
category: Programming
---  

首先我承认我是个小偷，我已经把别人的知识剽窃过来据为己有了。I confess~

被我偷过的无辜“受害者”列表(在此，我向他们的无私慷慨表示感谢):

- [Lvalues and Rvalues (C++)](https://docs.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp?view=msvc-160)
- [Understanding the meaning of lvalues and rvalues in C++](https://www.internalpointers.com/post/understanding-meaning-lvalues-and-rvalues-c)
- [What are rvalues, lvalues, xvalues, glvalues, and prvalues?](https://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues)
- [PDF: n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf)
- [Value Categories in C++17](https://medium.com/@barryrevzin/value-categories-in-c-17-f56ae54bccbe)

如果你想偷我的，不劳而获窃取最后的成果，欢迎你开始阅读；如果你有好耐心，又是一个不怕麻烦的小偷，那么上述列出的文章，建议不要错过。

正文如下：

---

## 1 背景(Background)
对历史背景不感兴趣或者没有耐心的读者，可直接跳转到下一部分: 2 基本概念

### 1.1 在其它语言中的历史
术语"lvalue"(左值)和“rvalue”(右值)，最早是由Christopher Strachey为CPL(B语言的祖先)编程语言引入。在他1967年这篇颇具影响力的讲义: [Fundamental Concepts in Programming Languages](https://classes.cs.uoregon.edu/14S/cis607pl/Papers/fundamental-1967.pdf), 他首次提到了`L-values`和`R-values`(见p15)。

引用他几句经典的论述：

>An L-value represents an area of the store of the computer. We call this a location...    
Some locations are addressable but some are not.     
The two essential features of a location are that it has a content—i.e. an associated R-value

其大意是说，L-value表示一个位置，R-value表示它的内容。用如今的话说，**L-value具有程序可访问的(内存)地址，R-value是内容**。可以有趣认为，左值就是一个盛水果的容器，而右值就是容器内的水果。他这一最初的概念，一直沿用至今。

然后，C语言之父丹尼斯·里奇(Dennis Ritchie)，使用"lvalue"的概念描述C（见[K&R,1978](http://www.ccapitalia.net/descarga/docs/1978-ritchie-the-c-programming-language.pdf), p183, APPENDIX A, Objects and lvalues)。但是他忽略了"rvalue"， 因为“lvalue”和非“lvalue”对C语言来说已经足够了。

### 1.2 在C++中的发展

再到后来的C++，草案也就有了“lvalue”和“ravlue”，起初，他们也在FCD中沿用了过去常规的含义。

C++规范对这两个术语的精确措辞很难，但是有助于解决一些已知的规范问题（与右值引用有关）。Stroustrup原先不打算做出更改，但是，CWG大多数人都不同意并坚持认为，必须有一些更改或使用新术语来解决已知问题和使规范保持一致。

简单地说，就是因为C++11即将引入的右值引用和移动语义的新特性，导致用原有旧术语lvalue和rvalue就不好解释了。所以他们要重新命名和定义新术语。

[这份PDF](https://www.stroustrup.com/terminology.pdf) ，Stroustrup记录了他们讨论和思考的过程。他们找到了问题的关键，因为每个表达式的值都具有两个属性。

#### 每个值都有两个独立的属性：

- 具有身份（has identity) --比如一个指针，一个地址，用户能决定这两份拷贝是具有身份的。
- 可移动(can safely be moved from)--就是允许移动之后，脱离拷贝的来源，其状态有效。

如果有人觉得不好理解什么是“具有身份”和“可被移动”，那就看我写的两个解释吧：

- [什么是具有身份](http://ruibing.org/programming/2099/02/24/computer-science-terms.html#has_indentity)
- [什么是可被移动--C++移动语义](http://ruibing.org/programming/2099/02/24/computer-science-terms.html#can_be_moved_from)

#### 基于这两个属性，就有了三种不同的组合结果：

- 具有身份+不可移动
- 具有身份+可移动
- 没有身份+可移动

排列组合，按理说是有第四种可能性的。但是对于第四种“没身份且不可移动”，这种值对C++没有用（以及其它任何语言）, 所以就舍弃了。

经过这样的思考也就得出了这样的结论：**值拥有两个独立属性，被划分成三个基本的分类；我可以简称为“两属性三类别”**。基于这样的概念，也就有了相应的命名。

![关系](https://docs.microsoft.com/en-us/cpp/cpp/media/value_categories.png?view=msvc-160, "Source:docs.microsoft.com") 

属性“具有身份”，对应了glvalue（广义的左值）
属性“可移动”，对应了ravlue

基于这两个属性的三种结果结果，分别命名：

- 具有身份+不可移动 -->  lvalue
- 具有身份+可移动  --> xvalue 
- 没有身份+可移动  --> prvalue

好了，C++表达式类型的历史演变也就解释完毕了。上面的结构图，也就是目前C++17使用的标准。

---

## 2 基本概念(Basic concepts)

在过去, C++左值和右值的概念相对简单，过去有一句经典的论述：
>Every expression is either an lvalue or an rvalue.
每个表达式不是左值，就是右值。

这句话经常在各种C++文章中看到，但是我得提醒你，自从2011年起(ISOC++11 , ISO/IEC 14882)，上面的那句经典论述已经不再成立。

发生了什么事情呢？ ISO C++11，新的C++标准引入了新的术语，并且定义了表达式的各种类别。新的分类如下：

| 值类别  |  定义  |  
|---|---|
|  lvalue  |  一个左值(在历史上，因为左值可能出现在赋值表达式的左侧)指定一个函数或者对象。【例子：如果p是一个表达式的指针类型，那么*p就是一个左值表达式，它引用p所指的一个对象或者函数。另一个例子：调用一个函数，它的返回值类型是左值引用，那么函数的结果是左值。】  |  
| xvalue | xvalue (an “eXpiring”过期的值)。它也引用一个对象，该对象通常接近它的生命周期(以至于它的资源可被移动)。xvalue是某些包含右值引用的表达式【示例：函数返回类型为右值引用的是xvalue。】 |
| glvalue |  (“generalized” lvalue， 广义的左值)，它是一个lvalue或者xvalue|
| rvalue |  （在历史上，因为右值可能出现在赋值表达式的右侧）是一个xvalue，一个临时对象或子对象，或没有与任何对象关联的值。|
| prvalue |  (“pure” rvalue) 是一个ralue，但不是xvalue. 【比如函数的调用结果，其返回值不是一个引用，那就是pvalue；再比如字面的12,7.e5, 或者ture 都是prvalue】|

详细的英文定义在这里： [n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf), 第五页。

下面的结构图展示了不同分类之间的关系，请仔细揣摩：

![relationships between the categories](https://docs.microsoft.com/en-us/cpp/cpp/media/value_categories.png?view=msvc-160) 


换韦恩图(Venn diagram)表示:

![exp_categlory](https://i.loli.net/2021/02/28/ZLhGuPVeNO7xy68.png  "https://github.com/crb912/crb912.github.io/blob/master/my_private/image/cpp/cpp17_exp_value_venn_diagram.png?raw=true")

上面是比较规范的定义，我用更为通俗的话重新定义一次：

| 值类别  |  通俗的解释  |  
|---|---|
| lvalue | 具有一个地址，并且程序可访问该地址。比如变量，指针，类成员函数 ...| 
|xvalue | 具有一个地址，但程序不再能访问。比如数组下标，函数返回的右值引用） |
| glvalue　| 具有地址　| 
| prvalue | 它没有一个地址 |
| rvalue  | 它没有一个地址，或者，它有地址但是程序不再能访问 | 

如果对这些术语有困惑，不必惊慌，随后会给出具体的示例。

## 3 值分类示例(Examples)
### 3.1 解释lvalue和pvalue

先看特别简单的表达式：
```C++
int y;
y = 10;   // 一个常见的表达式
```
先说结论：

- 作为右操作数的数字10，，是ravlue,　更是prvalue.
- 作为左操作数的变量y，是glvalue,　更是lvalue.

解释：　

1. 右边的数字10是rvalue，因为它是没有内存地址的。**更精确的说**，它是pvalue(由于它是rvalue且不是xvalue)。
2. 左边的变量 `y`是glvalue， 因为它是一个变量，它有内存地址。**更精确的说**，它是lvalue(由于这个变量不是临时变量,　程序可继续访问它的地址，意味着它不是xvalue)

这样一来，我们就解释了比较重要的lvalue和prvalue。后面如果能用“更精确的说”，那我们就尽可能用精确的值类别。这就无需关心glvalue和rvalue了。那么xvalue呢？　欲知后事如何，请看下文分解。

### 3.2 解释xvalue

```cpp
void move_test(){
    std::string s = "I'm here!";
    std::string m  =  std::move(s);  // move from <s> to <m>
    // s is now in an undefined, but valid state;  S被清空了，但是Ｓ状态有效
    std::cout << "s=" << s << "; &s=" << &s << std::endl;
    std::cout << "m = " << m << "; &m=" << &m << std::endl;
}
```
当`std::string m  =  std::move(s); `语句执行之后，s的值就成了空的，但是它的内存地址还在。就是说未定义但状态有效。

 这个表达式中的`std::move(s)`是ravlue，**更精确的说**，它是`xvalue`。它既不是ravlue，又不是prvalue。
 

### 

在C++中，什么是变量？ 变量是命名的对象。什么是对象？对象是某特定类型的内存空间。什么是类型？类型是一组可能值及其一组操作。 经过我这几个问题，你应该知道：**变量有内存空间的，它是左值(lvalue)**. 

反过来写行不行呢？

```C++
10 = y;    // error
```
不行，编译器会给出错误:

>error: lvalue required as left operand of assignment
作为赋值的左操作数，需要lvalue。



如果我们想了解新表达式值类别的概念，则必须知道C++11有右值和左值引用，观察下面C++11的代码：

```C++
int y = 10;
int& r_i = y;     // OK，正确的左值引用
int&& rr_i = y;   //  compile error

int& r_i2 = 7;    // compile error
int&& rr_i2 = 7;   // OK，正确的右值引用， C++11
```

然而在过去的C++98标准中，没有右值引用的概念，下面的写法是错误的。
```
int&& rr_i2 = 7;  //  compile error ， C++98
```
基于C++98的编译器会给出错误：
>expected unqualified-id before ‘&&’ token


## 左值向右值转换，可以


## 右值向左值转换，不可以

```C++
int y = 10;
int& yref = y; 
```

yref是左值引用(lvalue reference)，这是lvalue 表达式。

所以说，左值引用(lvalue reference)，顾名思义只能去引用左值(lvalue)，而不能试图去引用右值(ravlue). ** 为什么不能引用右值？ 因为右值没有内存地址呀** 。

引用右值是编译器禁止的行为。如果我天生头铁，一定要引用右值呢？

给你两个例子：

```C++
int& yref = 10;  // 可以吗?
```

当然，上述写法是不可以的，编译器会给出错误：
>
cannot bind non-const lvalue reference of type ‘int&’ to an rvalue of type ‘int’
// 不能把<左值引用>绑定到右值


包括下面这样的方式，试图引用右值，编译器会给出相同的错误提示。

```C++
void fnc(int& x)
{
}

int main()
{
    fnc(10);  // 不可以
}


```
但是我引用左值就没有任何问题，看这里
```C++
void fnc(int& x) {}

int main()
{
   int x = 10;
   fnc(x);
}

```
还没写完，今天接着写,.. 先更新一点
