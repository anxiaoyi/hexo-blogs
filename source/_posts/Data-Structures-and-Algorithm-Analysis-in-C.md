---
title: Data Structures and Algorithm Analysis in C
date: 2016-03-26 21:07:32
tags: book
---

![Book](http://ec4.images-amazon.com/images/I/51U7%2BI63eWL.jpg)

<!-- more -->

## List

### 应用

- 多项式ADT
- 基数排序(radix sort)。 基数排序是桶排序(bucket sort)的推广。
- 多重表

## Stack

### 应用

- 平衡符号。编译器检查程序是否缺少右括号。
- 后缀表达式。形如`6 5 2 3 + 8 * + 3 + *`，数字入栈。
- 中缀到后缀的转换。`a + b * c + ( d * e + f ) * g`=>`a b c * + d e * f + g * +`，符号入栈。
- 函数(递归)调用

## Queue

## Tree

对于大量的输入数据，链表的线性访问时间太慢，不宜使用。而`Tree`的大部分操作的平均时间为`O(log N)`

### Binary Tree

#### 应用

- 二叉树=>表达式树(expression tree)。树叶存储操作数，其它节点存储操作符。中序遍历可以得到中缀表达式，后序遍历得到后缀表达式。
- 编译器的核心数据结构就是`parse tree`

### Binary Search Tree

对于树中每个节X，它的左子树的所有关键字小于X的关键字值，右子树所有关键字的值大于X的关键字值。

### AVL(Adelson-Velskii和Landis) Tree

每个节点的左子树和右子树的高度最多差1的二叉查找树。

### B-树

所有树叶都在相同深度上。

#### 应用

实际用于数据库系统。使用M阶B-树，那么磁盘访问次数是`O(logM^N)`。典型的B-树的深度只有2或3，而根可以放在主存中。
