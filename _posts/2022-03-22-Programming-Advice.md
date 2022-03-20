---
published: true
title: 编程建议
layout: post
author: Eureka Bing
category: Programming
---

* 目录
{:toc}

### 基本类型
- 如果你的代码有太多cast，说明你已经搞砸了。
- Use `dynamic_cast` where class hierarchy navigation is unavoidable; 类层次漫游行为不可避免时，使用它`dynamic_cast`。 如果果无法转换向目标类时，想要它报错，就用`dynamic_cast` 作用于引用类型；不希望报错就，就作用于指针类型。
- Use `unique_ptr` or `shared_ptr` to avoid forgetting to delete objects created using new;
- new出来的裸指针，直接从函数返回是很危险的。最好用标准库的`unique_ptr`,例如`return unique_ptr<Shape>{new Circle{p,r}};`

### 类

- Use concrete classes to represent simple concepts;
- Prefer concerete classes over class hierarchies for performance-critical components; 高性能组件尽可能用具体类
- Make a function a member only if it needs direct access to the representation of a class; 仅当需要直接访问类的表示时，才使用成员函数。
- Declare a member function that does not modify the state of its object `const`;
- If a class is a container, give it an initializer-list constructor; 
- An abstract class typically doesn’t need a constructor; 
- A class with a virtual function should have a virtual destructor;
- 由于抽象类灵活的特性，在派生类的destructor函数要保证资源能被释放，这太重要了。
