---
published: true
title: 每日笔记
layout: post
author: RuiBing
category: Programming
---  
每日的学习内容简记，方便复习和回顾

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
