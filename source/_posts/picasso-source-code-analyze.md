---
title: "Picasso source code analyze"
layout: post
date: 2015-04-26
tags:
- opensource
categories:
- android
---

# Picasso 源码解析

## 使用Builder初始化

Picasso的初始化是使用Picasso.Builder进行创建的，默认创建的组件有：

- Downloader [作用：下载图片，默认：Utils.createDefaultDownloader(context)]
- Cache [作用：缓存，默认：LruCache]
- ExecuteService [默认：PicassoExecutorService]
- RequestTransformer [默认：RequestTransformer.IDENTITY]
- Stats
- Dispatcher

## Downloader的创建

在Utils中我们可以看到创建Downloader的这部分代码：

```java
static Downloader createDefaultDownloader(Context context) {
    if (SDK_INT >= GINGERBREAD) {
        try {
          Class.forName("com.squareup.okhttp.OkHttpClient");
          return OkHttpLoaderCreator.create(context);
        } catch (ClassNotFoundException ignored) {
        }
    }
    return new UrlConnectionDownloader(context);
  }
```
在 3.0 系统以上，如果有OkHttpLoader，那么就会默认采用这个去创建，否则采用UrlConnectionDownloader。
首先来看一下 Downloader接口中的两个方法：
```java
//从uri处下载图片
Response load(Uri uri, int networkPolicy) throws IOException;
//关闭
void shutdown();
```
其中 Response 是对结果的一种封装：包括以下几个字段：
- InputStream
- bitmap
- cached
- contentLength
然后在UrlConnectionDownloader中使用了 HttpURLConnection 来创建连接 connection, 并返回
```java
return new Response(connection.getInputStream(), fromCache, contentLength);
```
这样就拿到了输入流了。

## Cache 的构建

### Cache 接口

Cache 接口很简单，就是为了取和放图片
```java
public interface Cache{
       Bitmap get(String key);
       void set(String key, Bitmap bitmap);
       ...
}
```

### Cache的默认实现LruCache

LruCache 是一种使用了 least-recently used 机制的缓存，基于LinkedHashMap来实现。LinkedHashMap 是一种 保持插入顺序的HashMap，迭代顺序可预知， 调用此构造方法
```java
LinkedHashMap(int initialCapacity,  float loadFactor,  boolean accessOrder)
```
将最后一个参数设置为true，即可得到下面的一个这样顺序的 HashMap
[访问最少......访问最多]

#### LruCache的最大缓存大小

```java
static int calculateMemoryCacheSize(Context context) {
    ActivityManager am = getService(context, ACTIVITY_SERVICE);
    boolean largeHeap = (context.getApplicationInfo().flags & FLAG_LARGE_HEAP) != 0;
    int memoryClass = am.getMemoryClass();
    if (largeHeap && SDK_INT >= HONEYCOMB) {
      memoryClass = ActivityManagerHoneycomb.getLargeMemoryClass(am);
    }
    // Target ~15% of the available heap.
    return 1024 * 1024 * memoryClass / 7;
  }
```
最大大小为程序所能使用的堆的最大值的 15%

#### getBitmap 获取Bitmap

```java
return map.get(key);
```

#### setBitmap 放置图片，复杂一些

- map.put(key, bitmap) 先把图片放进去再说
- trimToSize(maxSize) 然后调用此方法，得把当前已经缓存的大小控制maxSize以内。
具体实现就是：
```java
while (true) {
        1.从map中不停的remove
        2.计算size
        3.当size < maxSize的时候，break
}
```

## 构建ExecutorService

ExecutorService 相当于是 Picasso的引擎吧，其默认采用了PicassoExecutorService来实现。
PicassoExecutorService 继承自 ThreadPoolExecutor，默认构造参数：
```java
super(DEFAULT_THREAD_COUNT, DEFAULT_THREAD_COUNT, 0, TimeUnit.MILLISECONDS,new PriorityBlockingQueue<Runnable>(), new Utils.PicassoThreadFactory());
```
主要的构造参数：
- 默认持有3个线程
- 默认采用优先阻塞队列来调度任务
- 默认ThreadFactory创建的Thread优先级如下：THREAD_PRIORITY_BACKGROUND
```java
Process.setThreadPriority(THREAD_PRIORITY_BACKGROUND);
```
提交执行的Runnable被强转成了BitmapHunter，依据BitmapHunter.getPriority()来对比任务的优先级。

## 构建Transformation

Transformation 主要是用来 当Bitmap加载完成之后，显示不同效果的，对Bitmap做一个转换

## 比较核心的类：

- RequestCreator 创建加载参数
- Dispatcher
