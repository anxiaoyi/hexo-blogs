---
title: "SVG Introduction"
layout: post
date: 2015-10-25
tags:
- svg
categories:
- software
---

## [SVG Colors, Patterns & Gradients](http://www.it-ebooks.info/book/6407/)

## 首先你得知道…

- `SVG`是使用代码画的
- `SVG`总是开源的，它是一段 human-readable 文件
- `SVG`是`XML`
- 图像是一系列`Shapes`元素的集合.

```xml
<rect>
<circle>
<ellipse>
<line>
<polyline>
<polygon>
<path>
```

- `Images`可以嵌套
- `SVG`有自己的结构，样式，可以移动，改变…

## 刷子

```xml
<svg xmlns="http://www.w3.org/2000/svg"
xmlns:xlink="http://www.w3.org/1999/xlink"
 viewBox="0 0 400 200" width="4in" height="2in">
 <title xml:lang="en">Fill-rule comparison</title>
 <rect fill="lightSkyBlue" height="100%" width="100%" />
 <polygon id="p"
 fill="blueViolet" stroke="navy"
 points="20,180 20,20 180,20 180,180 60,60 140,60" />
 <use xlink:href="#p" x="50%" fill-rule="evenodd" />
</svg>
```
