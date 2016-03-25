---
title: "Android Memory Leak"
layout: post
date: 2016-03-20
tags:
- android-memory-leak
categories:
- android
---

## 怎样知道内存泄露了

使用[Memory Monitor](http://developer.android.com/tools/performance/memory-monitor/index.html)来监视内存情况，如果内存不是平稳的(直线的)，而是一直增长，就可能意味着存在内存泄露。

## 怎样防止内存泄露

### 不要把`Context`传递到你的`Activity`,`Fragment`所能控制的范围之外

### 永远永远不要把`Context`, `View`声明为`static`的。这是你内存泄露的第一个信号。

```java
private static TextView textView; //DO NOT DO THIS
private static Context context; //DO NOT DO THIS
```

### 切记在`onPause()/onDestroy()`中取消注册`listeners`

### 在`AsyncTask`或者`background threads`中不要持有指向`Activity`的强引用。你的`Activity`或许已经关闭，但是`AsyncTask`将会继续运行。

### 在`Activity`中，尽量使用`getApplicationContext()`来替代`Context`。

### 尽量不要使用`non-static inner class`，如果你能避免的话。在内部类中，如果持有`View`或者`View`的强引用，那么就有可能导致内存泄露。请使用`WeakReference`来代替`Strong Reference`

## 我该怎样解决内存泄露问题

- 打开`Android Studio`，打开`Android Monitor`标签
- 然后打开你的应用，执行一些操作
- 你可以点击`garbage truck icon`来模拟`GC`垃圾收集行为
- 点击`Dump Java Heap`,等待几秒钟，之后便会生成一个`.hprof`文件，你可以分析这个文件得到内存的具体适用情况
- 不幸的是，`Android Studio`不具备`Eclipse Memory Analyzer`所具备的所有功能，所以你需要手动安装一个`MAT`
- 运行下列命令，来将`.hprof`文件转换为`MAT`所能理解的东东。`./hprof-conv path/file.hprof exitPath/heap-converted.hprof`(`.hprof-conv`命令位于`platform-tools`文件夹下面)
- 转换成功之后，然后在`MAT`中打开，选择`Leak Suspects Report`，然后点击完成。
- 点击那个`3 blue bars`图标，你将会看到所有对象占用的内存
- 你可以使用`class name`来过滤这些对象
- 我们现在可以看到9个`VideoDetailActivity`,很显然不对，右击然后选择`Merge Paths to Shortest GC Root`，然后点击`exclude all phantom/weak/soft etc.references`，所有持有`VideoDetailActivity`引用的线程都将会成现在你的眼前。
- 我们可以看到原来是因为我们忘记`unregister listener`了

并不是所有的内存泄露找起来都如此轻松，有一些找起来实在让人捉急…

## 参考

[Fixing Memory Leaks in Android – OutOfMemoryError](http://riggaroo.co.za/fixing-memory-leaks-in-android-outofmemoryerror/?subscribe=error#blog_subscription-2)