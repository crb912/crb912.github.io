---
published: true
title: 修炼我的独孤九剑--C++
layout: post
author: Eureka Bing
category: Programming
---  
小师妹对林平之情有独钟，令狐冲情场失意，在思过崖上，经风清扬指点，学会了独孤九剑。

![令狐冲](http://www.luxst.com/uploads/article/201912/14/143317bwluknkwudlr55oq.jpeg)



我想每个程序员都有一个这样的心愿：有一门自己特别喜欢并且擅长的编程语言，就像令狐冲的独孤九剑。如果让我去选择一门编程语言，作为我的独孤九剑全心全意去修炼的话，我选择`C++`。

既然是修炼武功秘籍，那就有修炼的过程。或心得体会，或困惑，或一些备忘，全都记在这里好了。

在此声明：我不保证你能看懂我写的东西，我只保证我自己写的自己看懂。

---

# Table of Contents(目录)

- [Array Decay](#ArrayDecay)
- [什么是具有身份(has identity)](#has_indentity)
- [移动语义(can be moved from)](#can_be_moved_from)
- [RAII](#RAII)
- [const](#const)
- [Tagged Union](#tagged_union)

---

## RAII <a name="RAII"></a>

The technique of acquiring resources in a constructor and releasing them in a destructor, known as **Resource Acquisition Is Initialization** or RAII, allows us to eliminate “naked new operations,” that is, to avoid allocations in general code and keep them buried inside the implementation of well-behaved abstractions. 

The technique was developed for exception-safe resource management in C++ during 1984–89, primarily by Bjarne Stroustrup and Andrew Koenig, and the term itself was coined by Stroustrup.

对于一个用惯了Python的人，刚开始并不能理解为什么有这种技术--在constructor申请资源，在destructor释放资源。经过一些思考，也就能理解了。解释如下：

Python把对象的创建交给了程序员，把对象的销毁交给了garbage collection; 而C++把对象的创建和销毁都交给了程序员。所以写Python时不需要过于关心资源的释放的问题，而写C++的程序员就要明确自己释放资源的时机。

我们知道，C++的所有对象，除了在free store中申请的资源，对象和相应的资源都会在离开作用域时自动销毁。那么，为了防止程序员忘记了释放在free store中申请的资源，**C++设计者在对于OO的类进行设计时，应该为类显式地提供一个进入和离开的作用域，以便于程序员在离开作用域之前释放资源**。 Bjarne Stroustrup给出的解决方案是：C++为你提供构造函数(constructor)和析构函数(destructor)，也就有了明确的作用域(scope)，程序员有意识的在构造函数申请资源在析构函数释放资源。就不会因为离开作用域而忘掉资源的释放，就会让代码很好的安全性，避免了资源泄露。

我认为，C++完全可以不为我们提供析构函数(destructor)。但是，这就要求每个编写C++的**程序员都要有很好的自我修养--时刻清楚地知道资源是否有被安全地释放**。 Bjarne Stroustrup为C++引入的 RAII技术，就减少了我们写出不安全代码的可能性。也简化了资源管理的过程，简化了整体代码。我认为这是一个十分优秀的设计。

---

## 什么是具有身份(has identity) <a name="has_indentity"></a>

Stroustrup在这份[terminology.pdf](https://www.stroustrup.com/terminology.pdf)文档，记录了他们在重定义Ｃ＋＋ value category相关术语的过程。最后是基于表达式两个属性作为出发点。其中一个属性是“具有身份”(has identity)，这个描述让我有一些困惑，什么才是叫具有身份呢？

因为它没有精确的定义，我查了一些网络资料。包括stackoverflow上的这个回答[What's the meaning of “identity” in the definition of value categories in C++？](https://stackoverflow.com/questions/53443234/whats-the-meaning-of-identity-in-the-definition-of-value-categories-in-c) 。

我同意其中的一个回答，认为它是一个哲学概念，就像OO编程的“类”这一哲学概念。举例子说，小明和小红都是人，但是他们是不同的实体，拥有不同的**身份**。类比到编程语言中，比如:

```C++
int x, y;
x = 10;
y = 10;
```

比如x和y虽然它们的数字大小都是10，但是它们拥有不同的身份，在这里，最直观的体现是x和y拥有不同的内存地址。但数字10在两个不同的表达式中，看不到它的身份属性，它就是一个数字10，没有身份。

再比如，两个名字不同的函数，即便它们在功能上完全一样，却拥有不同的身份。又或者bit-field（虽然它没有内存地址）也是有身份的。

---

## 什么是可被移动　<a name="can_be_moved_from"></a>

上面提到Stroustrup在这份[terminology.pdf](https://www.stroustrup.com/terminology.pdf)文档，提到了表达式值的两个基本属性，上面已经解释一个属性，对于它的另一个属性：**can be moved from**，解释在下面.

文档中的解释很简单，就是它是被允许离开初始源的“拷贝”，但是它状态还会有效。就比如**把杯子中的水，从Ａ的杯子倒往Ｂ的杯子。我们就说这个杯子的水是可移动的**。这是移动语义最简单的解释了。

Herb Sutter 在他这篇博客【[Move, simply](https://herbsutter.com/2020/02/17/move-simply/) 】也解释过移动语义, 看他怎么说的。

> In C++, copying or moving from an object a to an object b sets b to a’s original value. The only difference is that copying from a won’t change a, but moving from a might.

> To pass a named object a as an argument to a && “move” parameter (rvalue reference parameter), write std::move(a). That’s pretty much the only time you should write std::move, because C++ already uses move automatically when copying from an object it knows will never be used again, such as a temporary object or a local variable being returned or thrown from a function.

>That’s it.

翻译过来就是：

在C++中，复制或者移动，从对象a到对象b，会把b设置成a的初始状态。唯一的不同的是，复制a，不会改变a；但是移动a会改变。

把命名的对象a作为实参(argument )，传递给一个`&&`move形参(argument )， 请写成`std::move(a)`，这个`std::move`你应该只写一次。因为C++进行复制时已经使用了move，a是一个不会在使用的对象了。

他的解释也非常不错。

#### 代码示例
纸上得来终觉浅，可能上面这样口头的解释还是不够好吧，那就看代码：

```C++

void move_test(){
    std::string s = "I'm here!";
    std::string v { s };  // copy s
    std::cout << "s = " << s << "; &s=" << &s << std::endl;
    std::cout << "v = " << v << "; &v=" << &v << std::endl;

    std::string m { std::move(s) };  // move from <s> to <m>
    // s is now in an undefined, but valid state;  S被清空了，但是Ｓ状态有效
    std::cout << "s=" << s << "; &s=" << &s << std::endl;
    std::cout << "m = " << m << "; &m=" << &m << std::endl;
}
```

调用函数`move_test()`，运行的结果：
```text
s = I'm here!; &s=0x7fffad0afcc0
v = I'm here!; &v=0x7fffad0afce0
s=; &s=0x7fffad0afcc0
m = I'm here!; &m=0x7fffad0afd00

```
我们发现，复制s到v，没有改变s;  移动s到m， 则改变了s。

当执行`std:move`之后，变量s就被清空了，但是状态有效。完成了移动的操作，新的值放在m当中。注：`std::move(s)`它是个xvalue。既不是ravlue，又不是prvalue。

这就是移动语义，so easy.



## Array Decay <a name="ArrayDecay"></a>
language: C\+\+ 
see: [C++ Core Guidelines: P.4 statically type safe](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#p4-ideally-a-program-should-be-statically-type-safe)


The loss of type and dimensions of an array is known as decay of an array.This generally occurs when we pass the array into function by value or pointer. What it does is, it sends first address to the array which is a pointer, hence the size of array is not the original one, but the one occupied by the pointer in the memory.

Arrays are not basically the same as pointers; they are completely different animals.

通常说的数组丢失了它的类型和维度，“腐败”(Decay)成了指针或值。这种情况发生在函数通过值或指针传递数组时，实际上，数组并没有发生改变，而是我们得到的指针是指向了数组的第一个元素。References 1给的代码示例，sizeof实际计算的是该指针的字节大小(在64位系统中，指针是8字节)。

错误的本质原因是：数组和指针完全是两种不同的事物。即便当指针指向数组时，程序员也不能理所应当地认为它们等同，至少编译器不认为它们等同。

**References**:
1. [What is Array Decay in C++? How can it be prevented?](https://www.geeksforgeeks.org/what-is-array-decay-in-c-how-can-it-be-prevented/)
2. [SO: What is array to pointer decay?](https://stackoverflow.com/a/1461449)


## const <a name="const"></a>
C++的const可修饰普通类型的变量，函数返回值，函数参数，修饰引用，修饰指针。这种常识我不必说，人人都知道。

**不过应当注意两个细节，我觉得这是程序员们容易犯错的地方**：

1.**const修饰指针时**，const 在语句中的位置决定了修饰的意义。
A.如果*const在星号的左边*，则修饰了指针的内容`const int *p = 8;`，则8不可改变。(在表达式中，前置一元运算符*表示了...的内容)

B. 如果*const在星号的右边*，则修饰指针本身。修饰之后则不能篡改此指针指向的内存地址（某个对象)。通俗的话，const修饰之后，指针就不能改了。

2.当const**修饰类成员函数时**，表明该函数的行为不可以改变对象本身。注意它的写法，const在函数名与函数定义之间，语法:'`fct（）const {函数体}`。如果你想要一部分成员变量可改，一部分不可改，cpp允许你把该成员变量用`mutable`关键字修饰。cpp给足了类设计者充分的自由，保证了不错的灵活性。
```C++
class Test
{
public:
    Test(int _m,int _t):_cm(_m),_ct(_t){}
    void Kf() const
    {
        // 错误
        // Cannot assign to non-static data member within const member function
        ++_cm;
        ++_ct; // 正确
    }
private:
    int _cm;
    mutable int _ct;
};
```

## Tagged Union <a name="tagged_union"></a>
我在阅读这篇[ Avoid “naked” unions](https://www.modernescpp.com/index.php/c-core-guidelines-rules-for-unions)时，看到`tagged union`的概念。我知道自己曾经看过这个名词，但就是一时想不起来再哪本书看到过，最后还是在《A Tour of C++》(2.4 Unions)这本书上找到了。

书中提到，一定不要用“裸”Union，因为会得到一个未定义的行为(undefined behaviour)。时刻保持类型域（type field）字段，如下面代码的t。
```C++
struct Entry {
     string name;
     Type t;
     Value v;  // use v.p if t==ptr; use v.i if t==num
};

void f(Entry* pe)
{
     if (pe−>t == num)
           cout << pe−>v.i;
     // ...
}
```
Maintaining the correspondence between a type field (here, t) and the type held in a union is error-prone. To avoid errors, **we can enforce that correspondence by encapsulating the union and the type field in a class** and offer access only through member functions that use the union correctly.  

因为维护一个type filed是容易出错的，所以作者建议我们把`union`和`type field`封装到一个类中。如何封装成类？我看这里的代码 --> [C.182: Use anonymous unions to implement tagged unions](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Ru-anonymous)，（出自 TC++PL4，pp216-218）


这篇暂时不是写，没时间。
