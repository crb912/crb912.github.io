---
published: true
title:  C++ by Example
layout: post
author: RuiBing
category: programming
---


### Overloading operator + 

#### 1. simple example
Implementing `y = y1 + y2;`

```cpp
#include<iostream>

class Year{
    int year;

public:
    Year(int i): year{i} {};
    int echo() const { return year; }

    Year operator+ (Year y2){
        return Year(year+ y2.year);
    }
};

int main(){
    Year y1{5};
    Year y2{3};
    Year y = y1 + y2;
    std::cout << y.echo() << std::endl;
}

```

#### 2. opearands are two differet type, using `friend` function.
Implementing `y = 3 + y1;` and `y = y1 + 7;` ( they are two different case ).

```cpp
#include<iostream>

class Year{
    int year;

public:
    Year(int i): year{i} {};
    int echo() const { return year; }
    Year operator+ (int y2) const;
    friend Year operator+ (int y2, const Year& y);
};

Year Year::operator+ (int y2) const {
    return Year(year+ y2);
}

// `friend` function is NOT member function! we don't use `Year::` qualifier.
// But it can access private members.
Year operator+ (int y2, const Year& y){
    return Year(y.year+ y2);
}

int main(){
    Year y1{2};
    Year y = 3 + y1;
    Year z = y1 + 7;
    std::cout << y.echo() << "," << z.echo() << std::endl;
```

### Overloading operator <<
#### 1. simple example
Implementing `cout << Year(2021);`
```cpp
#include<iostream>

class Year{
    int year;

public:
    Year(int i): year{i} {};
    friend void operator << (std::ostream& os, const Year& y);

};

void operator << (std::ostream& os, const Year& y){
    os << "Year: " << y.year;
}
int main(){

    std::cout << Year(2021);
}
```
#### 2. return `ostream` object
Implementing `cout << Year(2021) << ", Happy every day!";`

NOTE: C++ reads the output statement from left to right, meaning it it equivalent to the following:

`(cout << Year(2021) ) << ", Happy every day!";`

So the friend function `operator <<`  must return a reference to an `ostream` object `cout`.

```cpp
#include<iostream>

class Year{
    int year;

public:
    Year(int i): year{i} {};
    friend std::ostream& operator << (std::ostream& os, const Year& y);

};

std::ostream& operator << (std::ostream& os, const Year& y){
    os << "Year: " << y.year;
    return os;
}
int main(){
    std::cout << Year(2021) << ", Happy every day!" << std::endl;
}
```
### Automatic conversions for class.
Implementing `int x =  Year(2021);`
	
```cpp
#include<iostream>

class Year{
    int year;

public:
    Year(int i): year{i} {};
    operator int(){ return year; }

};

int main(){
    int x =  Year(2021);
    std::cout << x << std::endl;
}
```



































