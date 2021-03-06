---
title: "APUE"
layout: post
date: 2015-09-05
tags:
- linux
categories:
- linux
---

## 高级IO

对于一个给定的描述符有两种方法对其指定为非阻塞I/O.
1. `open()` with `O_NONBLOCK`
2. `fcntl()` to open file `O_NONBLOCK` file state flag

轮询浪费CPU时间，发出数千个`write()`，然而真正输出数据的却只有20多个，这称为`轮询`。

记录锁(record locking)，更适合的术语应该称为byte-range locking，当一个进程正在读或者修改一个文件的某个部分时，它可以阻止其他进程修改同一个文件区。

`readv` 和 `writev` 用于在一次函数调用中读、写多个非连续缓冲区，有时也称作散步读(scatter read) 和 聚集写(gather write)

`Memory-mapped IO`存储映射IO使一个磁盘文件和存储空间的一个缓冲区相映射，在不使用`read` 和 `write`的情况下，执行IO。

## 数据库函数库

大多数访问数据库的函数使用两个文件来存储信息：一个索引文件和一个数据文件。散列法和B+树是两种常用的技术用来组织索引文件，提高按键查询的速度和效率。

当有多个进程访问同一个数据库时候，有两种方法可以实现库函数：
集中式：由一个进程作为数据库管理者，所有的数据库访问工作由此进程完成，其他进城通过IPC机制与此进程联系。
非集中式：每个库函数独立申请并发控制（加锁），然后自己调用IO函数。

并发
建议性锁：粗锁(整个文件：比如0字节的读锁和写锁)和细琐(记录所在的散列链的读锁或者写锁)。