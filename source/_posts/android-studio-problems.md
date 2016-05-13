---
title: "Android Studio Problems"
layout: post
date: 2015-11-28
tags:
- android studio
categories:
- android studio
---

## Unable to run mksdcard SDK tool

![Unable to run mksdcard SDK tool](/assets/image/androidstudio/unable-to-run-mksdcard-sdk-tool.png)

假设你的机器是64位的，那么执行下面命令就OK：

```bash
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0 lib32stdc++6
```

## Change background color

`Preferences`->`Editor`->`Colors & Fonts`->`General`->`Default text`->`Checkout Background`

## Android Studio Proxy by Lantern

Host name: 127.0.0.1, Port number: 8787

## Extract Hard-Coded String Literals

```bash
Alt + Enter
```

## Delete a module in Android Studio

`File`->`Project Structure`, Select `Modules`, Click `Minus` icon

## ImageView in ListView, placeholder can not use the same ColorDrawable

width and height may be fixed