---
published: true
title:  C++17的左值和右值
layout: post
author: RuiBing
category: art
hide: true
---  

首先我承认自己是个小偷，我已经把别人的知识剽窃过来据为己有了。被我偷过的无辜“受害者”列表 (在此，我向他们的无私慷慨表示感谢):

- [Lvalues and Rvalues (C++)](https://docs.microsoft.com/en-us/cpp/cpp/lvalues-and-rvalues-visual-cpp?view=msvc-160)
- [Understanding the meaning of lvalues and rvalues in C++](https://www.internalpointers.com/post/understanding-meaning-lvalues-and-rvalues-c)
- [What are rvalues, lvalues, xvalues, glvalues, and prvalues?](https://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues)
- [PDF: n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf)
- [Value Categories in C++17](https://medium.com/@barryrevzin/value-categories-in-c-17-f56ae54bccbe)
- [The deal with C++14 xvalues March 19, 2017](https://blog.jeaye.com/2017/03/19/xvalues)
- [What expressions create xvalues?](https://stackoverflow.com/questions/11581903/what-expressions-create-xvalues)

如果你想不劳而获窃取我最后的成果，欢迎你开始阅读。

## 摘要

本篇主要是讲述C++11之后新的表达式值类别的概念，我会解释以下术语:
`lvalue`, `ravlue`, `prvalue`, `xvalue`, `glvalue`; 并且我将会给出足够丰富的代码示例。

我文章的标题是不严谨的，因为我不仅仅解释了左值(`lvalue`)和右值(`ravlue`)，实际上我解释了五个所有的值类别。使用这样的标题仅处于旧的惯例。

我将先从`lvalue`和`rvalue`的历史起源开始说起，然后逐步过渡到C++11之后引入的新的值类别概念。如果您对它们的历史背景不感兴趣，或者您是没有耐心的读者，那么您可以直接跳转到下一部分阅读:  [2 基本概念](#basic_concepts)。

---
## 目录

[1 背景](#Background)
	- [1.1 在其它语言中的历史](#history_in_other)
	- [1.2 在C++中的发展](#dev_in_cpp)
[2 基本概念](#basic_concepts)
[3 值分类示例](#examples)
	- [3.1 解释lvalue和prvalue](#lvalue_prvalue)
	- [3.1.1 赋值的左操作数需要lvalue](#lvalue)
	- [3.1.2 不可以去prvalue取址](#prvalue)
	- [3.1.3 更多的lvalue示例和左值引用](#more_examples)
	- [3.2 解释xvalue](#xvalue)


---

## 1 背景(Background) <a name="Background"></a>


### 1.1 在其它语言中的历史 <a name="history_in_other"></a>
术语"lvalue"(左值)和“rvalue”(右值)，最早是由Christopher Strachey为CPL(B语言的祖先)编程语言引入。在他1967年这篇颇具影响力的讲义: [Fundamental Concepts in Programming Languages](https://classes.cs.uoregon.edu/14S/cis607pl/Papers/fundamental-1967.pdf), 他首次提到了`L-values`和`R-values`(见p15)。

引用他几句经典的论述：

>An L-value represents an area of the store of the computer. We call this a location...    
Some locations are addressable but some are not.     
The two essential features of a location are that it has a content—i.e. an associated R-value

其大意是说，L-value表示一个位置，R-value表示它的内容。用如今的话说，**L-value具有程序可访问的(内存)地址，R-value是内容**。可以有趣认为，左值就是一个盛水果的容器，而右值就是容器内的水果。他这一最初的概念，一直沿用至今。

然后，C语言之父丹尼斯·里奇(Dennis Ritchie)，使用"lvalue"的概念描述C（见[K&R,1978](http://www.ccapitalia.net/descarga/docs/1978-ritchie-the-c-programming-language.pdf), p183, APPENDIX A, Objects and lvalues)。但是他忽略了"rvalue"， 因为“lvalue”和非“lvalue”对C语言来说已经足够了。

### 1.2 在C++中的发展 <a name="dev_in_cpp"></a>

再到后来的C++，草案也就有了“lvalue”和“ravlue”，起初，他们也在FCD中沿用了过去常规的含义。

C++规范对这两个术语的精确措辞很难，但是有助于解决一些已知的规范问题（与右值引用有关）。Stroustrup原先不打算做出更改，但是，CWG大多数人都不同意并坚持认为，必须有一些更改或使用新术语来解决已知问题和使规范保持一致。

简单地说，就是因为C++11即将引入的右值引用和移动语义的新特性，导致用原有旧术语lvalue和rvalue就不好解释了。所以他们要重新命名和定义新术语。

[这份PDF](https://www.stroustrup.com/terminology.pdf) ，Stroustrup记录了他们讨论和思考的过程。他们找到了问题的关键，因为每个表达式的值都具有两个属性。

#### 每个值都有两个独立的属性：

- 具有身份（has identity) --比如一个指针，一个地址，用户能决定这两份拷贝是具有身份的。
- 可移动(can safely be moved from)--就是允许移动之后，脱离拷贝的来源，其状态有效。

如果有人觉得不好理解什么是“具有身份”和“可被移动”，那就看我写的两个解释吧：

- [什么是具有身份](https://snippets.cacher.io/snippet/17bd7f37e8fa257cee1c#F0_MH--%E4%BB%80%E4%B9%88%E6%98%AF%E5%85%B7%E6%9C%89%E8%BA%AB%E4%BB%BD-has-identity)
- [什么是可被移动--C++移动语义](https://snippets.cacher.io/snippet/17bd7f37e8fa257cee1c#F0_MH--%E4%BB%80%E4%B9%88%E6%98%AF%E5%8F%AF%E8%A2%AB%E7%A7%BB%E5%8A%A8)

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

## 2 基本概念(Basic concepts) <a name="basic_concepts"></a>

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
| prvalue |  (“pure” rvalue) 是一个ralue，但不是xvalue. 【比如函数的调用结果，其返回值不是一个引用，那就是prvalue；再比如字面的12,7.e5, 或者ture 都是prvalue】|

详细的英文定义在这里： [n3055](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2010/n3055.pdf), 第五页。

下面的结构图展示了不同分类之间的关系，请仔细揣摩：

![relationships between the categories](https://docs.microsoft.com/en-us/cpp/cpp/media/value_categories.png?view=msvc-160)


换韦恩图(Venn diagram)表示:

![exp_categlory](https://i.loli.net/2021/02/28/ZLhGuPVeNO7xy68.png  "https://github.com/crb912/crb912.github.io/blob/master/my_private/image/cpp/cpp17_exp_value_venn_diagram.png?raw=true")

**从集合关系：prvalue是rvalue的子集，lvalue是glvalue的子集**。

前面给出了比较规范的定义，我用更为通俗的话重新定义一次：

| 值类别  |  通俗的解释  |  
|---|---|
| lvalue | 具有一个地址，并且程序可访问该地址。比如变量，指针，类成员函数 ...|
|xvalue | 具有一个地址，但程序不再能访问。 |
| glvalue　| 具有地址　|
| prvalue | 它没有一个地址 |
| rvalue  | 它没有一个地址，或者它有地址但是程序不再能访问 |

如果对这些术语有困惑，不必惊慌，随后会给出具体的示例。

## 3 值分类示例(Examples)  <a name="examples"></a>
### 3.1 解释lvalue和prvalue <a name="lvalue_prvalue"></a>

先看特别简单的表达式：
```C++
int y;
y = 10;   // 一个常见的表达式
```
先说结论：

- 作为右操作数的数字10，，是ravlue,　更是prvalue.
- 作为左操作数的变量y，是glvalue,　更是lvalue.

解释：　

1. 右边的数字10是rvalue，因为它是没有内存地址的。**更精确的说**，它是prvalue(由于它是rvalue且不是xvalue)。
2. 左边的变量 `y`是glvalue， 因为它是一个变量，它有内存地址。**更精确的说**，它是lvalue(由于这个变量不是临时变量,　程序可继续访问它的地址，意味着它不是xvalue)

#### 3.1.1 赋值的左操作数需要lvalue <a name="lvalue"></a>

继续看示例。如果把上面的表达式反过来写呢？
```C++
10 = y;    // error!
```
不行，编译器会给出错误:

```text
error: lvalue required as left operand of assignment
作为赋值的左操作数，需要lvalue。
```
如果我这样写呢？
```
int i = 2;
i * 4 = 7;  // error!
```
同样编译失败，因为`i*4`得到了一个prvalue。

如果我耍个滑头，试图用函数返回的prvalue作为左操作数呢？如下：
```cpp
int setValue()
{
    return 6;
}
// ... somewhere in main() ...
setValue() = 3; // error!
```
编译器会给出同样的错误，想蒙混过关，欺骗编译器，那是不可能的！

下面这样写，完全就没有问题：
```cpp
int my = 100;

int& set_my_value()
{
    return my;
}
// ... somewhere in main() ...
 set_my_value() = 400; // OK
```

#### 3.1.2 不可以对prvalue取址 <a name="prvalue"></a>

再继续做一些蠢操作，对数字使用`&`取地址：
```cpp
int* y = &666; // error!
```
编译器给出错误：
```text
lvalue required as unary ‘&’ operand
作为一元运算符&的操作数，必须是lvalue
```
因为666是prvalue，因此就不满足这个条件。


这样一来，经过这些示例，我们就解释了比较重要的lvalue和prvalue。后面如果能用“更精确的说”，那我们就尽可能用精确的值类别。这就无需关心glvalue和rvalue了。

那么xvalue呢？　欲知后事如何，请看下文分解。

#### 3.1.3 更多的lvalue示例和左值引用 <a name="more_examples"></a>
左值表达式的示例包括变量名，包括const变量，数组元素，位字段，union和类成员等等...我不打算给出全部的示例, 我想下面的示例已经足够了吧。

```cpp
int i, j, *p;
i = 7;   // the variable i is lvalue
const j = 7;   //  the constant j is a non-modifiable lvalue

struct X { int n; };
X var_a;
var_a.n = 5;   // an class/strut member is lvalue

struct S {
 // three-bit unsigned field,
 // allowed values are 0...7
 unsigned int b : 3;
};
S s = {6};
++s.b; //  `s.b` bit field  is lvalue
...
```

我们注意的是，一种特殊的左值: lvalue reference（左值引用）

```cpp
    int y = 10;
    int& yref = y;
    yref++;        // both y and yref now are 11;
    y++;        // both y and yref now are 12;

    const int& yref_2 = y;
    yref_2++;        //  error!  cannot modify through reference to const!
    y++;        // y ，yref, yref_2;   both three variables increase by 1
```    

`yref`它是一个左值引用，`yref_2`是const修饰的左值引用。我们可以认为非const修饰的`yref`是`y`两个完全等价的别名。【Lvalue references can be used to alias an existing object (optionally with different cv-qualification)】

函数返回的左值引用：

```cpp
int y = 10;
int& z = y;

int& fct_lvalue_ref(){
    z++;
    return z;
}

fct_lvalue_ref()++;   // now  z is 12
```
上面的例子只是告诉你，从C++语法的角度，左值引用的是可以这么写的。但是我不建议你在C++代码中使用这么古怪的写法。大多数时候，我们使用左值引用的最多的情况是--函数的传引用(准确说法是：传左值引用)，这么做只是为了避免对象的拷贝：

```cpp

struct Widget{int n;} ;
void foo(Widget& w){};   // Pass by rvalue reference
void c_foo(const Widget& w){};    // Pass by const rvalue reference
```

**wrap up**:
关于使用左值引用去得到别名，我不认为以这样的方式增加一个对象的别名具有显著的意义。可能在我们维护代码的时候有一些小小的帮助，尤其是对一个作用域比较广的全局变量，使用别名可以很好区分”新变动的代码“与”旧代码“之间的改动区别，容易识别代码的改动是基于哪个别名的。

### 3.2 解释xvalue  <a name="xvalue"></a>

An xvalue (an “eXpiring” value) also refers to an object, usually near the end of its lifetime (so that its resources may be moved, for example). An xvalue is the result of certain kinds of expressions involving rvalue references. [Example: The result of calling a function whose return type is an rvalue reference is an xvalue.]

x表示即将过期。即便是C++官方的草案，它用的是'usually'和'the result of certain kinds of '的说法，官方也没办法把这个定义的十分清晰和准确。

从我理解的角度: 只要一个对象生命周期即将结束，并且出现在表达式的右边(或者作为返回值)。就可以称这样的值为xvalue。
比如:

```
class Person { // 类Person定义了一个人的：姓名，年龄，身高 ...}; 

Person xiao_ming;
xiao_ming = Person("小明"，15, 175); // 临时对象, xavlue
```
显然的，这个`Person("小明"，15, 175)`即将要过期了，他是符合定义的。它的资源被清除了，清除也算“may be moved”。 上面这个赋值语句涉及了两个操作：

- 构造临时对象
- copy assignment operator 。

其中copy assignment operator的过程涉及到了左值引用，因此官方提到的“An xvalue is the result of certain kinds of expressions involving rvalue references.” 也是满足的。

#### 移动语义中的xvalue
其次移动语义也有xvalue。

```cpp
void move_test(){
    std::string s = "I'm here!";
    std::string m  =  std::move(s);  // move from <s> to <m>
    // s is now in an undefined, but valid state;  S被清空了，但是Ｓ状态有效
    std::cout << "s=" << s << "; &s=" << &s << std::endl;
    std::cout << "m = " << m << "; &m=" << &m << std::endl;
}
```

运行结果：

```
s=; &s=0x7ffceba87480  // s被清了
m = I'm here!; &m=0x7ffceba874a0
```

当`std::move`那条语句执行之后，**注意s的值就清空了**，但是它的内存地址还在。就是说未定义但状态有效。

 这个表达式中的`std::move(s)`是`xvalue`，因为`std::move`返回了一个右值引用。每个xvalue都是glvalue，也是rvalue。

 我们应该注意的是，不是所有的右值引用都是`xvalue`，比如这种：

 ```cpp
 int&& rr_i = 7;    // rr_i is lvalue
 ```
