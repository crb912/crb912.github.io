---
published: true
title: 每日笔记
layout: post
author: RuiBing
category: Programming
---  
学习内容简记，方便复习和回顾。不保证每天都写，随机，尽量不中断。     
笔记中断的原因：要不是因为太忙没时间写，要不是有偷懒~     

---
## 03-04
#### 一个有趣的知识：恐龙灭亡
昨晚失眠的时候突然好奇恐龙怎么灭绝的，今天想起来了这茬，就Google了一下。大概是说在白垩纪(K)-古近纪(P)之间的时期(称为K-PG)，发现这个时期的地质中的钛含量异常高，而钛在小行星中比较常见。推断有发生小行星撞地球，造成了短期的生物大灭亡，75%的物种灭绝。我想说，人作为一个特殊的动物，有现在这样的文明，简直是一个奇迹。美丽的行星地球。而我们每个个体的一生, 又多渺小而微不足道。
#### 显式地移动和拷贝
If you want to be explicit about generating default implementations, you can:
```cpp
class Y {
public:
     Y(Sometype);
     Y(const Y&) = default;   // I really do want the default copy constructor
     Y(Y&&) = default;        // and the default move constructor
     // ...
};
```

**When a class has a pointer member, it is usually a good idea to be explicit about copy and move operations**. The reason is that a pointer may point to something that the class needs to `delete`, in which case the default memberwise copy would be wrong. Alternatively, it might point to something that the class must not `delete`. In either case, a reader of the code would like to know.

简单翻译下：**当类有一个指针成员，最好显示地移动或者拷贝**。因为指针可能指向了类需要销毁(delete)的资源或者对象，默认的按成员拷贝的方式会出错。即便不需要delete, 阅读代码的人也该知道(就是说，也要显示的default)。很好理解，因为虽然进行了拷贝和移动，然而有的资源仍是处于共享的状态。

####　拇指规则(rule of thumb)
又称经验规则，rule of zero。accu这篇文章写得不错: [ENFORCING THE RULE OF ZERO](https://accu.org/journals/overload/22/120/alday_1896/) 。It states that if a class defines a destructor it should almost always define a *copy constructor* and a* copy assignment operator*.  这是个好习惯，应该定义每个C++类的时候都该保持这个习惯，虽然编译器不强制我这这么做。这篇这个文章的那个Managed resources部分的List 2代码写得好秀，作者设计的这个类不允许拷贝，但允许移动。List３的代码开始没看懂，看完这篇[unique_ptr](https://xhy3054.github.io/cpp-unique-ptr/) 自定义deleter的写法也就懂了，主要是不熟悉这种写法。List3的deleter不是自定义的，下面还有自定义的写法:
```cpp
struct Widget{  };
// ...
auto deleter = []( Widget *p ) {
    cout << "delete Widget!" << endl;
    delete p;
};
unique_ptr<Widget, decltype(deleter)> ptr{ new Widget, deleter };
```

**rule of thumb的具体体现的意义：Polymorphic deletion / virtual functions**

！！！明天再写，熬不住了，睡觉了

[decltype (C++)](https://docs.microsoft.com/en-us/cpp/cpp/decltype-cpp?view=msvc-160) ，获取变量/函数/成员的类型或者表达式值类别，内括号会导致表达式求值。

我有注意到一个英语的习惯表达，术语/词语的“发明”通常用“coined by sb”，不是什么invent之类的。



## 03-02

不容易，终于写完了这篇[C++的左值和右值](http://ruibing.org/programming/2021/02/27/cpp-lvalue-rvalue.html)，几千字不到，写了好几天。写完也搞清楚了C++的很多东西，包括移动语义。明天继续读书。

刚顺手在StackOverflow上面回答了一个关于C++的问题 [What expressions create xvalues?](https://stackoverflow.com/a/66439358/7360457) ，写得简单的英文，应该没有语言错误。

---

## 03-01
写了一点东西，解释了C++“具有身份”和“移动”的语义，分别这两篇是：

-  [C++: 具有身份(has identity)](http://ruibing.org/programming/2099/02/24/computer-science-terms.html#has_indentity) 
-  [C++: 移动语义(can be moved from)](http://ruibing.org/programming/2099/02/24/computer-science-terms.html#can_be_moved_from)

要不是英语写的不如中文准确，我都想用英文写了。还得多练习和使用英语！


## 02-27
今天在写这篇[C++的左值和右值](http://ruibing.org/programming/2021/02/27/cpp-lvalue-rvalue.html) ，当是笔记吧。写东西好费脑子，查资料也很费时间。拖到晚上11点了，还没写完，困了就休息。**感觉还是不够专注，特别是下午容易疲惫**。

## 02-26
忙一些杂事去了，没学多少东西，笔记就不写了。

## 02-25
小结:  读完了 A Tour of C++的Class章节，涵盖了具体类，抽象类，虚函数这些知识。遇到问题记在纸上，阅读结束再查资料，效率高了许多。

**虚函数的匹配机制**
![vtbl](https://i.loli.net/2021/02/25/3mI8cJ7COAXktvz.png  "https://github.com/crb912/crb912.github.io/blob/master/my_private/image/cpp/2021-02-25-18-05-26.png?raw=true")

当函数调用Virtual Functions时，多个派生类(或者说子类)都实现了基类的虚函数，编译器要通过**vtbl(virtual function table)**纯虚函数表去选择相应的函数，才能保证不会调用出错。

虚函数调用的效率： 
1. 效率接近普通的函数调用，相差不超过25%
2. 空间开销(space overhead) A. 每个派生类的vtbl; B.每个对象需要一个额外的指针。这么看来空间开销很小。

**使用override的习惯**
派生类对虚函数进行override的时候，最好显式的使用`override`关键字，以避免拼写错误或者让代码更明显。

**抽象类的灵活性与不足**：抽象类提供的函数，可以简化派生类的函数实现。抽象类函数，甚至不需要知道实参的类型，通过引用或指针就能完成相应的操作。强大的灵活性，唯一的不足就是依赖引用或者指针。

**警惕抽象类的资源泄露**：由于抽象类灵活的特性，在派生类的destructor函数要保证资源能被释放，这太重要了。

英语固定搭配： especially **with respect to **error handling(固定搭配with respect to=就..而言/对于)

**dynamic_cast运算符**，有助于判定对象是否属于某个抽象类的类型，有点像Python的`is`, A是B的一个实例吗？。是一个问询，可以简化代码。如果无法转换向目标类时，想要它报错，就用`dynamic_cast` 作用于引用类型；不希望报错就，就作用于指针类型。

**unique_ptr**：new出来的“裸”指针，直接从函数返回是很危险的。最好用标准库的`unique_ptr`,例如`return unique_ptr<Shape>{new Circle{p,r}};`

**C++编程建议**：

1. Use concrete classes to represent simple concepts;
2. Prefer concerete classes over class hierarchies for performance-critical components; 高性能组件尽可能用具体类
3. Make a function a member only if it needs direct access to the representation of a class;
4. Declare a member function that does not modify the state of its object `cons`t;
5. If a class is a container, give it an initializer-list constructor; 
6. An abstract class typically doesn’t need a constructor; 
7. A class with a virtual function should have a virtual destructor;
8. Use `dynamic_cast` where class hierarchy navigation is unavoidable;类层次漫游行为不可避免时，使用它`dynamic_cast`
9. Use `unique_ptr` or `shared_ptr` to avoid forgetting to delete objects created using new;
