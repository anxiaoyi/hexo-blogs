---
title: "Ios Dev"
layout: post
date: 2015-09-16
tags:
- iOS
categories:
- software
---

- [ios demo](http://www.jianshu.com/p/7d4710b815c2)

- [苹果官方入门](https://developer.apple.com/library/prerelease/ios/referencelibrary/GettingStarted/RoadMapiOSCh/index.html#//apple_ref/doc/uid/TP40012668)

- [objc-singleton](http://www.idev101.com/code/Objective-C/singletons.html)

- [ios-Populating UITableView content asynchronously](https://developer.apple.com/library/ios/samplecode/LazyTableImages/Introduction/Intro.html)

- [ios博客](http://xuexuefeng.com/autolayout/)

- [在ios中使用第三方类库](http://mobile.51cto.com/iphone-407056.htm)

- [cocoapods]

- [cocoapods-制作](http://www.360doc.com/content/14/0309/10/11029609_358969425.shtml)

- [如何制作framework](http://www.cocoachina.com/ios/20150127/11022.html)

- [Auto Layout 使用心得-博客](http://lvwenhan.com/ios/449.html)

- [Understanding AutoLayout](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/AutolayoutPG/index.html)

- [Dynamic-table-view-cell-height-auto-layout](http://www.raywenderlich.com/73602/dynamic-table-view-cell-height-auto-layout)

- install cocoapods:
```sh
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
```

- [ios dev blog](http://www.raywenderlich.com/2696/instruments-tutorial-for-ios-how-to-debug-memory-leaks)

- [Async Operations in iOS](http://jeffreysambells.com/2013/03/01/asynchronous-operations-in-ios-with-grand-central-dispatch)

- [Unicode And NSString](http://objccn.io/issue-9-1/)

- [iOS Visual format language](http://commandshift.co.uk/blog/2013/01/31/visual-format-language-for-autolayout/)

`[button]`: Views
`|`: SuperView
`-`: Spacing

`|-[button]-|`: 标准左右间距
`|-[button]`: 标准左间距

`|-30.0-[button]-30.0-|`: 距离左右边距 30.0 points
`|-spacing-[button]-spacing-|`: spacing is a key from `metrics` dictionary for reusability.

`[button(200.0)]`: 200 points wide
`[button(buttonWidth)]`: buttonWidth is a key in the metrics dictionary.
`|-[button1(button2)]-[button2]-|`: it can also be replaced with the name of another view.

`V:|-[button(50.0)]`: vertical layouts, the button must be 50.0 points high.

`<= >= ==(default)`
`V:|-(==padding)-[imageView]->=0-[button]-(==padding)-|`

`|[button(==200@750)]-[label]|`:The button's width should be 200 points, with a priority of 750.
`|-30.0@200-[label]`: The label should be spaced 30 points from the left of the superview, with a priority of 200.
```objective-c
NSDictionary *metrics = @{@"lowPriority":@(UILayoutPriorityDefaultLow)};
[self.view addConstraints:[NSLayoutConstraint constraintsWithVisualFormat:@"|-30.0@lowPriority-[label]" options:0 metrics:metrics views:NSDictionaryOfVariableBindings(label)];
```

- [iOS Blocks Programming Topics](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Blocks/Articles/bxGettingStarted.html#//apple_ref/doc/uid/TP40007502-CH7-SW1)

- [iOS Table View Programming Guide](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/TableView_iPhone/TableViewCells/TableViewCells.html)

- [iOS Using Segues](https://developer.apple.com/library/prerelease/ios/featuredarticles/ViewControllerPGforiPhoneOS/UsingSegues.html)

- [Document Interaction Programming Topics for iOS](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010409-SW1)

- [iOS Customizing Existing Classes](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/CustomizingExistingClasses/CustomizingExistingClasses.html)

- [iOS How To Create Framework](http://www.cocoachina.com/ios/20150127/11022.html)

- iOS开发中的问题：时间问题(差8个小时)、UITableViewCell AutoLayout 问题 (隐藏显示时间戳)、数据更新设计问题
