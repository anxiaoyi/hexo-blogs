---
title: "Deelpy understanding Java Class Loader"
layout: post
date: 2016-03-19
tags:
- Java
categories:
- Java
---

## 基本概念

类加载器负责加载 Java 类的字节代码到 Java 虚拟机中。一般来说，Java 虚拟机使用 Java 类的方式如下：Java 源程序（.java 文件）在经过 Java 编译器编译之后就被转换成 Java 字节代码（.class 文件）。**类加载器负责读取 Java 字节代码，并转换成 java.lang.Class类的一个实例。**每个这样的实例用来表示一个 Java 类。通过此实例的 newInstance()方法就可以创建出该类的一个对象。实际的情况可能更加复杂，比如 Java 字节代码可能是通过工具动态生成的，也可能是通过网络下载的。

**基本上所有的类加载器都是 `java.lang.ClassLoader` 类的一个实例**

`ClassLoader`的相关方法：

- getParent()	返回该类加载器的父类加载器。
- loadClass(String name)	加载名称为 name的类，返回的结果是 java.lang.Class类的实例。
- findClass(String name)	查找名称为 name的类，返回的结果是 java.lang.Class类的实例。
- findLoadedClass(String name)	查找名称为 name的已经被加载过的类，返回的结果是 java.lang.Class类的实例。
- defineClass(String name, byte[] b, int off, int len)	把字节数组 b中的内容转换成 Java 类，返回的结果是 java.lang.Class类的实例。这个方法被声明为 final的。
- resolveClass(Class<?> c)	链接指定的 Java 类。

一个典型的类加载器树状结构图：

![class-loader](/assets/image/class-loader/class-loader.jpg)

## 类加载器的代理模式

类加载器在尝试自己去查找某个类的字节代码并定义它时，会先代理给其父类加载器，由父类加载器先去尝试加载这个类，依次类推。**Java 虚拟机是如何判定两个 Java 类是相同的: Java 虚拟机不仅要看类的全名是否相同，还要看加载此类的类加载器是否一样。**代理模式是为了保证 Java 核心库的类型安全。所有 Java 应用都至少需要引用 java.lang.Object类，也就是说在运行的时候，java.lang.Object这个类需要被加载到 Java 虚拟机中。如果这个加载过程由 Java 应用自己的类加载器来完成的话，很可能就存在多个版本的 java.lang.Object类，而且这些类之间是不兼容的。通过代理模式，对于 Java 核心库的类的加载工作由引导类加载器来统一完成，保证了 Java 应用所使用的都是同一个版本的 Java 核心库的类，是互相兼容的。相同名称的类可以并存在 Java 虚拟机中，只需要用不同的类加载器来加载它们即可。这种技术在许多框架中都被用到。