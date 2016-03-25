---
title: "Famous technology blog"
layout: post
date: 2015-09-05
tags:
- facebook opensource
categories:
- opensource
---

## [FaceBook Code](https://code.facebook.com/posts/)

### [Improving Facebook on Android](/assets/pdf/Improving-Facebook-On-Android.ppt)

### Fast Rendering News Feed on Android

#### Why ListView Gets Complicated

To get your list to appear as if it scrolls smoothly, you need to render about 60 frames of it per second. Basic arithmetic translates this challenge into rendering one frame in under 16.7 milliseconds.

Whenever a new view goes into the viewport in your ListView, the ListView will call its adapter's `getView` method. That method, in turn, has to get a view, and bind all its content into it in less than 16.7ms. (In fact, since other actions have to happen, such as measuring and drawing, there's even less time).

It becomes challenging when trying to render rich content which contains complicated views — a lot of text and images. Android's simple tool to solve this is view recycling: The adapter's getView method will get an existing `convertView` to use if one was made before. This trick works very well, but only when your views are similar to each other.

#### How it used to work

Our design was based on the following concept: For every piece of a story in the feed, we created a custom view class. That custom view class would have a method called `bindModel` which would take the object this view was supposed to display and update the view to display it.

So, this meant to display a story we had a `StoryView` class, which among other things, had a `HeaderView` inside it to display the header of the story.

```java
public class NewsFeedStoryView extends LinearLayout {
    //...
    public void bindModel(NewsFeedStory story) {
        //...
        mHeaderView.bindModel(story);
        mMessageView.bindModel(story);
        mAttachmentsView.bindModel(story);
        //...
    }
}
```

```java
public class HeaderView extends RelativeLayout {
    //...
    public void bindModel(NewsFeedStory story) {
        //...
        mTitle.setText(story.getTitle());
        //...
    }
}
```

但是缺点来了：

- Android's recycling mechanism does not work well in this case.
- Complicated hierarchies. 耦合了业务逻辑和布局逻辑
- Deep-View hierarchy. 
- Computationally-intensive method: 没有一个较为便捷的方法在`bindModel`之前去执行代码，`bindModel`是在滑动的时候执行，所以，it's a pretty a bad time to do heavy work.

With that in mind, we decided to rewrite our rendering code to avoid these pitfalls.(自己开始写渲染代码了)

#### Making News Feed Scrolling Smoother

For that purpose we considered a new idea: Splitting each story in the News Feed to several items in the ListView. (将一个Story切分为多个Item) this nice trick results in various advantages right away. 有两个优点：

- First, this makes Android's recycling effective again. (局部Recycling)
- The second big advantage is that splitting the views lets us store less views in memory, and bind a story over several frames. (节省内存)

The other idea we incorporated to the design is decoupling the binding logic from the custom Views themselves. (另外一个想法就是：从自定义View中解耦binding逻辑)

