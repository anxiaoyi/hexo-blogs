---
title: "android dex"
layout: post
date: 2016-03-20
tags:
- android-dex
categories:
- android
---

将class的jar包转化为dex需要用到命令`dx`, 该命令位置是(我的机器):`~/Android/Sdk/build-tools/23.0.2/dx`，`dx –dex –output=output.jar origin.jar`，Android支持动态加载的两种方式是：`DexClassLoader`和`PathClassLoader`，DexClassLoader可加载jar/apk/dex，且支持从SD卡加载；PathClassLoader据说只能加载已经安装在Android系统内APK文件。

- `ClassLoader` 只支持 `class` 的加载
- `DexClassLoader` 支持 `Dex`, `Apk`, `Jar` 的加载
- `PathClassLoader` 支持 `/data/app` 目录下的apk加载