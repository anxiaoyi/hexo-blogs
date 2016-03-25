---
title: "ios dev tutorial"
layout: post
date: 2015-09-21
tags:
- iOS
categories:
- software
---

## ios tutorial (chuanzhiboke)

### 选中storyboard-->右边属性-->Size-->选择对应的手机尺寸，暂时不做屏幕适配。

### 按住Option键，拖动：复制。

### 每一个 UIView 都是一个容器，能容纳其他 UIView。

### Command + Shift + H + H : 最近运行任务

### 键盘消失：
```objective-c
- resignFirstResponder
- [self.view endEditing:YES];
```

### 文本框：clear button 属性

### ios5之前，苹果使用xib文件来描述UI界面。

### 模拟器：放大，缩小：Ctrl + 1, Ctrl + 2, Ctrl + 3

### StoryBoard 双击一下就变小了。

### 作图最好做成 png 文件

### 代码封装

```objective-c
switch (sender.tab) { 
	case 10:
	break;

	case 20:
	break;

	...
}
```

### UIButton
UIButton: frame, center, bounds, transform

### Animation

- 头尾式

```objective-c
//开启动画
[UIView startAnimation:nil context:nil]
[UIView setAnimationDuration:2];

//要执行的代码

//提交动画
[UIView commitAnimations];
```

- block式

```objective-c
[UIView animationWithDuration:0.5 animation:^{ 
}];
```

### 动态创建按钮

```objective-c
UIButton *button = [[Button] alloc] init];
//UIButton *button = [UIButton buttonWithType:xxx];
[button setTitle:@"title" forState:UIControlStateNormal];
button.frame = CGRectMake(50,100,100,100);
[button addTarget:self action:@selector(buttonClick) forControlEvents:UIControlEventTouchUpInside];
[self.view addSubView:button];
```

### Documentation

Help-->Documentation
