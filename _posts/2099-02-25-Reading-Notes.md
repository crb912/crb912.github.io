---
published: true
title: 读书笔记
layout: post
author: RuiBing
category: Programming
---  

# Table of Contents(目录)

- [算法导论](#introdion_to_algorithm)
-- [1. The Role of Algorithms in Computing](#dfe4)
-- [Code Implement of Introduction to Algorithms](#code1)


## 算法导论 <a name="introdion_to_algorithm"></a>

### 1. The Role of Algorithms in Computing <a name="dfe4"></a>
#### 1.1 算法
1. 算法是把输入转化成输出的一系列步骤的过程，算法的唯一要求就是能精确的描述它所遵从的计算步骤。
2. 大多数算法都有候选的其它方案，因此要学会举一反三。
3. 离散傅里叶变换可以将信号分解成频率谱，显示与频率对应的幅值大小。

#### 1.2 算法作为一门艺术
书中给出了一个例子：
- 电脑A：每秒运行指令 10 bilion, 即 10 * 10^10 （比B快1000倍）
- 电脑B：每秒运行指令 10 million, 即 10 * 10^7

1. 在A上运行插入排序，假设是世界上最熟练和灵巧的程序员，用机器语言实现的代码。排序n个数： 只需要2n^2条指令
2. 在B上运行归并排序，假设B是一个高级语言实现的，并且编译器不怎么高效，一个普通程序员写的代码。排序n个数： 需要50nlgn条  (lgn是2为底数的)

运行时间 = 总指令条数/主频
当排序10 milion条数字时，A超过5小时，B不足20分钟。
当排序100 million条时， A超过了23天，B不足4小时。

这个对比可以说是相当有说服力了，论算法效率的重要性和艺术性

#### X Code Implement of Introduction to Algorithms <a name="code1"></a>
**1. Insertion sort **   
**Input**: A sequence of n numbers < a1, a2,...an>.
**Output**: A permutaion (reordering) < a1', a2',...an'> of the input sequence such that a1' <= a2' <= an'.

**Pseudocode**:
```
// INSERTION-SORT(A)
for j = 2 to A.length
    key = A[j]
    // Insert A[j] into the sorted sequence A[1..j -1].
    i = j - 1
    while key < A[i] and i > 0
        a[i+1] = a[i]
        i = i - 1
    A[i+1] = key
```

**Python**:
```
A = [5, 2, 4, 6, 1, 3]
for j in range(1, len(A)):
    key = A[j]
    i = j - 1
    while key < A[i] and i >= 0:
        A[i+1] = A[i]
        i -= 1
    A[i+1] = key
```
**C\+\+**:
```
int A[6] = {5, 2, 4, 6, 1, 3};
	int length = sizeof(A)/sizeof(int);
	for(int j = 1;j < length; ++j){
		int key = A[j];
		int i = j - 1;
		while (key < A[i] and i >= 0)
		{
			A[i+1] = A[i];
			--i;
		};
		A[i+1] = key;
	}
```
1.1 


