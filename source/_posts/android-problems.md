---
title: "android developer problems"
layout: post
date: 2015-11-29
tags:
- andorid
categories:
- android
---

# Android Developer Problems

## 默认RecyclerView Item就有点击效果

```xml
android:clickable="true"
android:focusable="true"
android:background="?attr/selectableItemBackground" 
```

如果想要自定义ripple effect:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ripple xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@color/colorHighlight">

    <item
        android:id="@android:id/mask"
        android:drawable="@color/windowBackground" />
</ripple>
```

旧版本：

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="@color/colorHighlight" />
        </shape>
    </item>
    <item>
        <shape android:shape="rectangle">
            <solid android:color="@android:color/transparent" />
        </shape>
    </item>
</selector>
```

## `RecycleView`怎样像`ListView`那样有`dividers`？

[ItemDecoration](http://stackoverflow.com/questions/24618829/how-to-add-dividers-and-spaces-between-items-in-recyclerview)