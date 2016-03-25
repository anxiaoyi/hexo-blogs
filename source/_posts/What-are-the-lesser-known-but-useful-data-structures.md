---
title: "What are the lesser known but useful data structures?"
layout: post
date: 2015-10-25
tags:
- data structure
categories:
- software
---

## [What are the lesser known but useful data structures?](http://stackoverflow.com/questions/500607/what-are-the-lesser-known-but-useful-data-structures)

## [Tries](https://en.wikipedia.org/wiki/Trie)

trie, 通常被称作`digital tree`, `radix tree`, `prefix tree`,是一种有序树数据结构,通常被用来存储`dynamic set`或者一般键为字符串的`associative array`.`trie`取自单词`retrieval`.

树结构如下:

![Trie-Tree](https://upload.wikimedia.org/wikipedia/commons/b/be/Trie_example.svg)

### 应用

#### 取代其它数据结构

`trie`比`binary search tree`有数不清的优势,它也可以用来取代`hash table`.

- 在`trie`中查找字符串最坏的情况下,也非常快,为O(m),m是字符串的长度.
- 在`trie`中不会说有像`hash table`那样键冲突的问题.

`trie`劣势:

- `trie`有些情况下比`hash table`要慢
- 浮点数导致过长的链条或者前缀
- 需要更多的存储空间.

#### 表示

- 存储`predictive text`,或者`autocomplete`的字典
- `spell checking`
- `hyphenation`

## [Bloom filter](https://en.wikipedia.org/wiki/Bloom_filter)

### 算法描述

一个空的Bloom filter是一个拥有m个比特位的比特数组,全部初始化为0.此外,这里应该有k个不同的hash函数,每一个都将set中的元素映射到比特数组中去.

添加:每一个想要的添加的元素,经过k个hash函数,得到k个哈希值,然后将数组中对应的比特位设置为1.

查询(测试元素是否存在):对一个元素重新应用这k个hash function,如果所有位均为1,那么肯定存在

删除(不可能的)

有出错的概率: 0.61^(m/n): n是插入的数量, m是m个比特位

请看下图,如何在Bloom filter中表示集合{x,y,z}, w元素不在集合内

![Bloom_filter](https://upload.wikimedia.org/wikipedia/commons/a/ac/Bloom_filter.svg)

### 空间和时间优势

![Bloom_filter_speed](https://upload.wikimedia.org/wikipedia/commons/c/c4/Bloom_filter_speed.svg)

### 应用

- Google 的 `BigTable`, `Apache HBase`, `Apache Cassandra` 使用`Bloom filters`去减少不存在的行或者列的磁盘查找,提高数据查询操作的性能.
- Google Chrome 使用`Bloom filter`识别恶意的URLs. 任何URL先使用本地的Bloom filter去检查,通过之后然后再执行全检查.
- Squid Web 代理缓存使用`Bloom filter`为了`cache digests`.
- ...

## [Rope](https://en.wikipedia.org/wiki/Rope_%28data_structure%29)

### 一种字符串.

for cheap preends,取子字符串,中间插入,尾部添加. 它是由许多个小的字符串组成的,用来高效的存储和管理很长的字符串.例如一个文本编辑器可能会用到这样的数据结构.如此以来,插入,删除和随即访问效率大大的提升.

rope实际上是一种二叉树,只不过叶节点存放短字符串.

如下图所示,这个数据结构是基于"Hello_my_name_is_Simon"创建的.

![Vector_Rope_example](https://upload.wikimedia.org/wikipedia/commons/8/8a/Vector_Rope_example.svg)

## [Skip List](https://en.wikipedia.org/wiki/Skip_list)

skip list 主要是一种用来在有序序列里面快速搜索的数据结构. 快速搜索之所以变得可行,在于其维护了一个linked subsequences.

插入新元素:

![Skip_list_add_element_en](https://upload.wikimedia.org/wikipedia/commons/2/2c/Skip_list_add_element-en.gif)

## [Segment Tree](https://en.wikipedia.org/wiki/Segment_tree)

存储一段数据
