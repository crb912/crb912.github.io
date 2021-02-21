---
published: true
title: C++，爷的独孤九剑
layout: post
author: Eureka Bing
category: Programming
---  
![令狐冲](http://www.luxst.com/uploads/article/201912/14/143317bwluknkwudlr55oq.jpeg)

小师妹对林平之情有独钟，令狐冲情场失意，在思过崖上，经风清扬指点，学会了独孤九剑。

刚好，我最近也犯了好多错误，还处于思过的状态。这思过崖我是去不了了，但这“过”我还得思。我也需要孤独九剑来拯救一下自己，C++就理所当然成了我孤独九剑的不二之选。眼下需要心无旁骛的修炼，等我稍稍进步了一点，就可以出山闯荡江湖。

令狐冲虽学了独孤九剑，也不能就去杀了林平之，抢回小师妹。不过从此好歹也有了一技傍身，吃喝不愁。没准就此遇到了任盈盈，吃吃软饭那也是不错的。这么一想，就有满满的动力去学好C++了 。

下面是我修炼Ｃ++这“独孤九剑”的武功心得体会。慢慢更。

# Table of Contents(目录)
1. [const](#const)
2. [Tagged Union](#tagged_union)


## const <a name="const"></a>
C++的const可修饰普通类型的变量，函数返回值，函数参数，修饰引用，修饰指针。这常识我不必说，人人都知道。**不过应当注意两个细节，我觉得这是程序员们容易犯错的地方**：

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
