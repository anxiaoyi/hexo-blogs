---
title: "iOS learn"
layout: post
date: 2016-01-15
tags:
- iOS
categories:
- iOS
---

`iOS`开发过程中遇到的问题，Hint，解决方案等。

<!-- more -->

## 什么时候使用 `@class`

The @class directive minimizes the amount of code seen by the compiler and linker, and is therefore the simplest way to give a forward declaration of a class name. Being simple, it avoids potential problems that may come with importing files that import still other files. For example, if one class declares a statically typed instance variable of another class, and their two interface files import each other, neither class may compile correctly.

Another advantage: Quick compilation (编译更快速)

If you include a header file, any change in it causes the current file also to compile but this is not the case if the class name is included as @class name. Of course you will need to include the header in source file

## iOS 事件传递

[stackoverflow](http://stackoverflow.com/questions/4961386/event-handling-for-ios-how-hittestwithevent-and-pointinsidewithevent-are-r)

- 网友一答案：

The implementation of hitTest:withEvent: in UIResponder does the following:

It calls `pointInside:withEvent:` of self
If the return is NO, `hitTest:withEvent:` returns nil. the end of the story.
If the return is YES, it sends `hitTest:withEvent:` messages to its subviews. it starts from the top-level subview, and continues to other views until a subview returns a non-nil object, or all subviews receive the message.
If a subview returns a non-nil object in the first time, the first `hitTest:withEvent:` returns that object. the end of the story.
If no subview returns a non-nil object, the first `hitTest:withEvent:` returns self
This process repeats recursively, so normally the leaf view of the view hierarchy is returned eventually.

However, you might override `hitTest:withEvent` to do something differently. In many cases, overriding pointInside:withEvent: is simpler and still provides enough options to tweak event handling in your application.

```objective-c
+----------------------------+
|A                           |
|+--------+   +------------+ |
||B       |   |C           | |
||        |   |+----------+| |
|+--------+   ||D         || |
|             |+----------+| |
|             +------------+ |
+----------------------------+
```

Say you put your finger inside `D`. Here's what will happen:

`hitTest:withEvent`: is called on A, the top-most view of the view hierarchy.
`pointInside:withEvent`: is called recursively on each view.
`pointInside:withEvent`: is called on A, and returns YES
`pointInside:withEvent`: is called on B, and returns NO
`pointInside:withEvent`: is called on C, and returns YES
`pointInside:withEvent`: is called on D, and returns YES

On the views that returned YES, it will look down on the hierarchy to see the subview where the touch took place. In this case, from A, C and D, it will be D.
D will be the hit-test view

## `extern`关键字

`xxxx.h`文件里可能写着：

```objective-c
extern NSString *const ABCName;
extern CGFloat const ABCTextViewIndent;
```

[stackoverflow](http://stackoverflow.com/questions/538996/constants-in-objective-c)

怎样定义一个常量，正确做法：

```objective-c
// Constants.h
FOUNDATION_EXPORT NSString *const MyFirstConstant;
FOUNDATION_EXPORT NSString *const MySecondConstant;
//etc.
```

(you can use `extern` instead of `FOUNDATION_EXPORT` if your code will not be used in mixed C/C++ environments or on other platforms)

You can include this file in each file that uses the constants or in the pre-compiled header for the project.

You define these constants in a .m file like

```objective-c
// Constants.m
NSString *const MyFirstConstant = @"FirstConstant";
NSString *const MySecondConstant = @"SecondConstant";
```

Constants.m should be added to your application/framework's target so that it is linked in to the final product.

## `UI_APPEARANCE_SELECTOR` 啥意思？

## 带有`static`的变量有什么不同？

```objective-c
static CGFloat const ATLLineSpacing = 6;
CGFloat const ATLAddressBarTextContainerInset = 10.0f;
```

可以模拟`Java`或者`C++`里面的`class variables`

## `someView.translatesAutoresizingMaskIntoConstraints = NO`啥意思？

When using AutoLayout, you should never directly set the frame of a view. Constraints are used to do this for you. Normally if you want to set your own constraints in a view, you override the `updateConstraints` method of your UIViews. Make sure the content views for the page controller allow for their edges to be resized since they will be sized to fit the page view's frame. Your constraints and view setup will need to account for this, or you you will get unsatisfiable constraint errors.

[苹果官方文档](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/#//apple_ref/occ/instp/UIView/translatesAutoresizingMaskIntoConstraints)

A Boolean value that determines whether the view’s autoresizing mask is translated into Auto Layout constraints.

Discussion
If this property’s value is YES, the system creates a set of constraints that duplicate the behavior specified by the view’s autoresizing mask. This also lets you modify the view’s size and location using the view’s frame, bounds, or center properties, allowing you to create a static, frame-based layout within Auto Layout.当为`YES`的时候，将为你创建一个静态的，frame-based的布局，你可以通过修改view的frame，bounds或者center属性，来修改view的大小和位置

Note that the autoresizing mask constraints fully specify the view’s size and position; therefore, you cannot add additional constraints to modify this size or position without introducing conflicts. If you want to use Auto Layout to dynamically calculate the size and position of your view, you must set this property to NO, and then provide a non ambiguous, nonconflicting set of constraints for the view. 如果你想要动态计算你的view的大小和位置，那么请把它设置为`NO`，然后为这个view提供一个没有歧义不冲突的一个约束集合

By default, the property is set to YES for any view you programmatically create. If you add views in Interface Builder, the system automatically sets this property to NO. 凡是通过代码写的默认设置为`TRUE`，通过`Interface Builder`创建的默认为`NO`

Availability
Available in iOS 6.0 and later.

## `UIEdgeInsetsMake`

An inset is a margin around the drawing rectangle where each side (left, right, top, and bottom) can have a different value.

```objective-c
UIEdgeInsets UIEdgeInsetsMake ( CGFloat top, CGFloat left, CGFloat bottom, CGFloat right );
```

## `UIView`

[`UIView`](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/) ，当你想要继承`UIView`的时候，其官方文档告诉你几个你可能会选择去`override`的方法。

```objective-c
// The default implementation of this method returns the existing size of the view. Subclasses can override this method to return a custom value based on the desired layout of any subviews. For example, a UISwitch object returns a fixed size value that represents the standard size of a switch view, and a UIImageView object returns the size of the image it is currently displaying.This method does not resize the receiver.
- (CGSize)sizeThatFits:(CGSize)size

// Call this method when you want to resize the current view so that it uses the most appropriate amount of space. Specific UIKit views resize themselves according to their own internal needs. In some cases, if a view does not have a superview, it may size itself to the screen bounds. Thus, if you want a given view to size itself to its parent view, you should add it to the parent view before calling this method.
You should not override this method. If you want to change the default sizing information for your view, override the sizeThatFits: instead. That method performs any needed calculations and returns them to this method, which then makes the change.
- (void)sizeToFit

// Returns the natural size for the receiving view, considering only properties of the view itself.
// Custom views typically have content that they display of which the layout system is unaware. Overriding this method allows a custom view to communicate to the layout system what size it would like to be based on its content. This intrinsic size must be independent of the content frame, because there’s no way to dynamically communicate a changed width to the layout system based on a changed height, for example.
- (CGSize)intrinsicContentSize
```

## `NSLayoutConstraint`

```objective-c
item1.attribute1 = multiplier × item2.attribute2 + constant

// These equations produce identical constraints 相同结果
button2.leading = 1.0 × button1.trailing + 8.0
button1.trailing = 1.0 × button2.leading - 8.0
```

Additionally, constraints are not limited to equality relationships. They can also use greater than or equal to (>=) or less than or equal to (<=) to describe the relationship between the two attributes. Constraints also have priorities between 1 and 1,000. Constraints with a priority of 1,000 are required. All priorities less than 1,000 are optional. By default, all constraints are required (priority = 1,000). 
After solving for the required constraints, Auto Layout tries to solve all the optional constraints in priority order from highest to lowest. If it cannot solve for an optional constraint, it tries to come as close as possible to the desired result, and then moves on to the next constraint.
除了相等关系，还有>=和<=关系，另外Constraint还有`priority`属性，默认1000，即表示这个Constraint是必须的，小于1000的被认作是可选的。最后`Auto Layout`会进行排序

## `id` vs `instancetype`

## What does the `__block` keyword mean?`

[stackoverflow](http://stackoverflow.com/questions/7080927/what-does-the-block-keyword-mean)

It tells the compiler that any variable marked by it must be treated in a special way when it is used inside a block. Normally, variables and their contents that are also used in blocks are copied, thus any modification done to these variables don't show outside the block. When they are marked with `__block`, the modifications done inside the block are also visible outside of it.

Just like static, auto, and volatile, __block is a storage type. It tells the compiler that the variable’s storage is to be managed differently.

[The __block Storage Type](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/Blocks/Articles/bxVariables.html#//apple_ref/doc/uid/TP40007502-CH6-SW6)

You can specify that an imported variable be mutable—that is, read-write— by applying the __block storage type modifier.

```objective-c
__block int x = 123; //  x lives in block storage
 
void (^printXAndY)(int) = ^(int y) {
 
    x = x + y;
    printf("%d %d\n", x, y);
};
printXAndY(456); // prints: 579 456
// x is now 579
```

## `NSCache`

```objective-c
// Sets the value of the specified key in the cache, and associates the key-value pair with the specified cost.
// The cost value is used to compute a sum encompassing the costs of all the objects in the cache. When memory is limited or when the total cost of the cache eclipses the maximum allowed total cost, the cache could begin an eviction process to remove some of its elements. However, this eviction process is not in a guaranteed order. As a consequence, if you try to manipulate the cost values to achieve some specific behavior, the consequences could be detrimental to your program. Typically, the obvious cost is the size of the value in bytes. If that information is not readily available, you should not go through the trouble of trying to compute it, as doing so will drive up the cost of using the cache. Pass in 0 for the cost value if you otherwise have nothing useful to pass, or simply use the setObject:forKey: method, which does not require a cost value to be passed in.
- (void)setObject:(ObjectType)obj
           forKey:(KeyType)key
             cost:(NSUInteger)num
```

## `Grand Central Dispatch (GCD) Reference`

```objective-c
// Variables of this type must have global or static scope. The result of using this type with automatic or dynamic allocation is undefined. 
typedef long dispatch_once_t;

// Executes a block object once and only once for the lifetime of an application.
// This function is useful for initialization of global data (singletons) in an application. Always call this function before using or testing any variables that are initialized by the block.
// If called simultaneously from multiple threads, this function waits synchronously until the block has completed.
// The predicate must point to a variable stored in global or static scope. The result of using a predicate with automatic or dynamic storage (including Objective-C instance variables) is undefined.
void dispatch_once( dispatch_once_t *predicate, dispatch_block_t block);

// 像这样使用
+ (NSCache *)sharedImageCache
{
    static NSCache *_sharedImageCache;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _sharedImageCache = [NSCache new];
    });
    return _sharedImageCache;
}

// Submits a block for asynchronous execution on a dispatch queue and returns immediately.
void dispatch_async( dispatch_queue_t queue, dispatch_block_t block);

// Returns the serial dispatch queue associated with the application’s main thread.
dispatch_queue_t dispatch_get_main_queue(void);
```

GCD provides and manages FIFO queues to which your application can submit tasks in the form of block objects. Blocks submitted to dispatch queues are executed on a pool of threads fully managed by the system. No guarantee is made as to the thread on which a task executes. GCD offers three kinds of queues: 三种类型的Queue:

- Main: tasks execute serially on your application’s main thread 串行

- Concurrent: tasks are dequeued in FIFO order, but run concurrently and can finish in any order. 并行

- Serial: tasks execute one at a time in FIFO order FIFO队列

The main queue is automatically created by the system (操作系统创建Main Queue) and associated with your application’s main thread. Your application uses one (and only one) of the following three approaches to invoke blocks submitted to the main queue:

- Calling dispatch_main

- Calling UIApplicationMain (iOS) or NSApplicationMain (OS X)

- Using a CFRunLoopRef on the main thread

Use concurrent queues to execute large numbers of tasks concurrently. GCD automatically creates four concurrent dispatch queues (three prior to iOS 5 or OS X v10.7) that are global to your application and are differentiated only by their priority level. Your application requests these queues using the dispatch_get_global_queue function. 系统默认创建4个全局的并行Queue供你使用

```objective-c
NSURLSessionDownloadTask
```

继承自`NSURLSessionTask`

Download tasks directly write the server’s response data to a temporary file, providing your app with progress updates as data arrives from the server. When you use download tasks in background sessions, these downloads continue even when your app is suspended or is otherwise not running.当你在一个background sessions中去下载东东的时候，即使你的app被挂起或者没有运行，这些任务仍将继续执行～

You can pause (cancel) download tasks and resume them later (assuming the server supports doing so). You can also resume downloads that failed because of network connectivity problems.

## `UICollectionView`

The collection view’s data source object provides both the content for items and the views used to present that content. When the collection view first loads its content, it asks its data source to provide a view for each visible item. To simplify the creation process for your code, the collection view requires that you always dequeue views, rather than create them explicitly in your code. There are two methods for dequeueing views. 

Before you call either of these methods, you must tell the collection view how to create the corresponding view if one does not already exist. For this, you must register either a class or a nib file with the collection view. 

Creating Collection View Cells:

```objective-c
// Register a class for use in creating new collection view cells.
registerClass:forCellWithReuseIdentifier:

// Registers a class for use in creating supplementary views for the collection view.
registerClass:forSupplementaryViewOfKind:withReuseIdentifier:
```

The `UICollectionReusableView` class defines the behavior for all cells and supplementary views presented by a collection view. Reusable views are so named because the collection view places them on a reuse queue rather than deleting them when they are scrolled out of the visible bounds. Such a view can then be retrieved and repurposed for a different set of content.

可能会重写的方法

```objective-c
// Performs any clean up necessary to prepare the view for use again.
- (void)prepareForReuse
```

## [UI Design Do's and Don'ts](https://developer.apple.com/design/tips/)

## Building from the command Line with XCode

```bash
# Listing all information about your project
xcodebuild -list -project MyiOSApp.xcodeproj

# Building the <MyiOSApp> scheme
xcodebuild -scheme <your_scheme_name> build

# Building the MyiOSApp target with a configuration file
xcodebuild -target MyiOSApp -xcconfig configuration.xcconfig
```

[References](https://developer.apple.com/library/ios/technotes/tn2339/_index.html)

## App Transport Security has blocked a cleartext HTTP (http://) resource load si

在iOS9 beta中，苹果将原http协议改成了https协议，使用 TLS1.2 SSL加密请求数据。

解决方法：在`info.plist`加入key

```xml
<key>NSAppTransportSecurity</key>
<dict>
<key>NSAllowsArbitraryLoads</key>
<true/>
</dict>
```

## 特殊语句

```objective-c
NSAssert(NO, @"ERROR: required method not implemented: %s", __PRETTY_FUNCTION__);

#ifdef DEBUG
#   define DLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__)
#else
#   define DLog(...)
#endif

// ALog always displays output regardless of the DEBUG setting
#define ALog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__)
```

## ARC forbids synthesizing a property of an Objective-C object with unspecified ownership or storage attribute

声明`block`为`@property`的时候会出现这种情况

```objective-c
@property (nonatomic) ABlock configureCellBlock;
```

手动指定告诉ARC`copy`属性即可解决

```objective-c
@property (nonatomic, copy) ABlock configureCellBlock;

self.aBlock = [configureBlock copy];
```

## `SQLite`数据库`Error inserting batch: near "an": syntax error`

[Android SQLite](http://www.cnblogs.com/iOcean/archive/2012/03/02/2377648.html)
[FMDB insert single-quote error](https://github.com/ccgus/fmdb/issues/345)

## `[UIWindow endDisablingInterfaceAutorotationAnimated:]`

在`UITableView`中，当键盘弹起来的时候，滑动`UITableView`，致使键盘落下，那么就会给出这种警告。

```objective-c
-[UIWindow endDisablingInterfaceAutorotationAnimated:] called on <UITextEffectsWindow: 0x7ff30963dbf0; frame = (0 0; 414 736); opaque = NO; autoresize = W+H; layer = <UIWindowLayer: 0x7ff30963e210>> without matching -beginDisablingInterfaceAutorotation. Ignoring.
```

解决方法，需要在`- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView`中写上`[textView resignFirstResponder]`即可解决

## `UITableView` 与 `NSRangeException`

出现下面异常:

```objective-c
*** Terminating app due to uncaught exception 'NSRangeException', reason: '*** -[__NSOrderedSetM objectAtIndex:]: index 19 beyond bounds [0 .. 11]'
```

有可能是因为`UITableView`正在使用的`NSMutableArray`或`NSMutableOrderedSet`里面的内容，在文件的其它地方已经被修改了。然而`UITableView`并不知道这一切。所以应该确保数据源一旦被修改之后，立马调用`UITableView reloadData`方法，来及时通知`UITableView`更新。

千万不要像下面这样更新：

```objective-c
- (void)replaceDataWithNewData:(NSMutableArray*)newData {
    NSMutableArray *oldData = ...; //从其它地方拿到oldData
    [oldData removeObjectsInRange:NSMakeRange(0, msgs.count)];
    [oldData addObjectsFromArray:newData];
}
```

## Back button callback in navigationController in iOS

```objective-c
-(void) viewWillDisappear:(BOOL)animated {
    if ([self.navigationController.viewControllers indexOfObject:self]==NSNotFound) {
       // back button was pressed.  We know this is true because self is no longer
       // in the navigation stack.  
    }
    [super viewWillDisappear:animated];
}
```

## `UITableView` 去除空白`Cell`的`separator`

```objective-c
self.tableView.tableFooterView = [UIView new];
```

## duplicate symbols for architecture armv7

```objective-c
...
ld: 534 duplicate symbols for architecture armv7
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```

可能是因为在`TARGETS`-`General`-`Linked Frameworks and Libraries`中，既引用了`xxx.a`，又引用了相关的`xxx.Framework`，导致`symbols duplicate`

## Custom `UICollectionViewCell`

需要重写`initWithFrame:`这个方法，才能达到目的

```objective-c
- (instance) initWithFrame:(CGRect)frame {
    if ([self = super initWithFrame:frame]) {
        // do initlize things
    }
}
```

## 界面全部黑屏

可能原因：`translatesAutoresizingMaskIntoConstraints`调用对象错误。正确的`AutoLayout`使用方法应该是：`superView`无需调用`translatesAutoresizingMaskIntoConstraints`，其`subView`需要每个都调用`translatesAutoresizingMaskIntoConstraints`方法，才能工作正常

正是由于错误的在`UICollectionViewCell.contentView.translatesAutoresizingMaskIntoConstraints = NO`调用，所以才导致屏幕一片黑白，以为`UICollectionViewController`无法正常工作。

## Make `UICollectionView` scrollble

```objective-c
self.collectionView.alwaysBounceVertical = YES;
```

## API

```objective-c
[self.navigationViewController popToViewController:xxController animated:YES];
```

## `UILabel`文字重叠(新文字显示在旧文字之上)

可能是因为在其它`Queue`中更新了`UI`

[ios-ensure-execution-on-main-thread](http://stackoverflow.com/questions/11582223/ios-ensure-execution-on-main-thread)

```objective-c
void runOnMainQueueWithoutDeadlocking(void (^block)(void))
{
    if ([NSThread isMainThread])
    {
        block();
    }
    else
    {
        dispatch_sync(dispatch_get_main_queue(), block);
    }
}                     
```

可能是你既重写了`initWithFrame`又重写了`init`方法，导致添加了两个`UILabel`

```objective-c
- (id)init {
}

- (id)initWithFrame {
}
```

但其实`init`内部会调用`initWithFrame:CGRectZero`

[Why UIView calls both init and initWithFrame?](http://stackoverflow.com/questions/19423182/why-uiview-calls-both-init-and-initwithframe)

## `AutoLayout` 确定 UIView 自身的大小

直接在`init`方法中调用`self.frame = CGRectMake(0, 0, 宽度，高度)`完事

## `UITableViewCell + AutoLayout + HideView`

如果需要动态控制`UITableViewCell`内部某个`View`是否显示，那么不能够使用`View.hidden`来约束它，因为及时`hidden=YES`，那么它所占据的位置依然存在，`NSLayoutConstraints`依然生效。所以应该给这个`View`添加`NSLayoutConstraints`对其高度进行约束，通过设置`constants`为`0`，或为`固定高度`来达到隐藏显示`View`的目的。

注意在这个过程中，我们应该适当结合`UILayoutPriorityXXX`和`NSLayoutRelationXXX`来控制`NSLayoutConstraints`的表现行为，尤其是因为`UITableViewCell`在`UITableView`中是被复用的，后一个`UITableViewCell`将会打破前一个设置的`NSLayoutConstraints`，而如果不对`priority`或者`relation`适当放松的话，那么将会弹出warning: `NSLayoutConstraints will attempt break ...`

## `UITableViewCell`中的`UITextView`无法被选中，未收到相关事件

你需要覆盖`UITableViewCell`的`hitTest`方法，参考[stackoverflow](http://stackoverflow.com/questions/7719412/how-to-ignore-touch-events-and-pass-them-to-another-subviews-uicontrol-objects)

```objective-c
- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event
{
    UIView *hitView = [super hitTest:point withEvent:event];

    // If the hitView is THIS view, return the view that you want to receive the touch instead:
    if (hitView == self) {
        return otherView;
    }
    // Else return the hitView (as it could be one of this view's buttons):
    return hitView;
}
```

## 设置`UITextView`的`word break`模式

```objective-c
textView.textContainer.lineBreakMode = NSLineBreakXXX;
```

## `UITableViewCell` 设置点击之后无样式：

```objective-c
self.selectionStyle = UITableViewCellSelectionStyleNone;
```

## `Objective-C` 不小心（无意识）覆盖父类私有方法的问题:

```objective-c

// Parent class

@interface Fruit: NSObject
@end

@implementation Fruit:

- (instancetype)init {
  if (self = [super init]) {
     [self common_init];
  }
  return self;
}

- (void)common_init {
  NSLog(@"Fruit common init");
}

@end

// Child class
@interface Apple: Fruit
@end

@implementation Apple:

- (instancetype)init {
  if (self = [super init]) {
     [self common_init];
  }
  return self;
}

- (void)common_init {
  NSLog(@"Apple common init");
}

@end

// Execute
Fruit fruit = [Apple new];

// Execute Result
// NSLog: Apple common init
// 注意到了没有，子类无意识的覆盖了父类里面私有的用来初始化自己的方法
// 导致父类的初始化操作没有被执行
// This is a big problem that you should take cared ...

```

## Http Block

App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.

TARGETS-Info-add a row-`App Transport Security Settings`-`Allow Arbitrary Loads`:`YES`

## Cocoapods

### Register

pod trunk register user_email user_name --description='macbook pro'

### Deploy

```bash
pod spec lint xxx.podspec
pod trunk push xxx.podspec --verbose
```

If you find execute command `pod trunk push xxx.podspec` was too slow, then you can
`cd ~/.cocopods/repos/master; git pull origin master`, then re-execute the `pud trunk push xxx.podspec` command

### `Pod Update`

```bash
pod install
pod update
```

## Suppress warning “Category is implementing a method which will also be implemented by its primary class”

```objective-c
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wobjc-protocol-method-implementation"

// do your override

#pragma clang diagnostic pop
```

## 响应`UITableViewCell`内部`View`的事件

如果你设置了`self.tableView.allowsSelection = NO`，此时`UITableView`不能响应click事件，点击无反应，`didSelectRowAtIndexPath`也不会被调用。这个时候绑定了`UITapGestureRecognizer`给`UITableView`，那么这个事件是可以被响应的。

假设你的`UITableViewCell`如下所示:

```objective-c
---------------------------
|              ---------- |
|              | Button | |
|              ---------- |
---------------------------
```

当用户点击`Button`左边区域的时候，`didSelectRowAtIndexPath`方法被调用，事件响应者为`UITableViewCell`，如果点击`Button`区域，那么事件响应者仍然为`UITableViewCell`：那么怎么将用户点击事件传递给`Button`按钮呢？答案就是`UITableViewCell`的`hitTest`方法，你需要像下面这样复写它：

```objective-c
- (UIView*)hitTest:(CGPoint)point withEvent:(UIEvent *)event {
  UIView *button = [self.buttonView]; // get your button view
  CGPoint p = [self convertPoint:point toView:button];
  if (CGRectContainsPoint(button.bounds, p)) {
     return [button hitTest:p withEvent:event];
  }
  return [super hitTest:point withEvent:event];
}
```

这个时候，一旦用户点击的`CGPoint`落在`Button`的`bounds`范围内的话，那么`Button`将会响应这个事件，你绑定在`Button`上面的事件(如：`UITapGestureRecognizer`)就会被触发，当然`didSelectRowAtIndexPath`是不会被触发了。

## `kUTTypeImage` undeclared

- Link MobileCoreServices.framework

```objc
MobileCoreServices.framework
```

- import `MobileCoreServices.h`

```objc
#import <MobileCoreServices/MobileCoreServices.h>
```

## `UITableViewController reloadData`的更新问题

`AController` is a subclass of `UITableViewController`

Bussiness Logic:

1. `AController` present `UIImagePickerController` to select a `UIImage`.
2. After Selected, upload `UIImage` to server, and get the url: `http://xxx.png`.
3. Notify `AController` to refresh data by calling method: `[AController.tableView reloadData]`;

The problem happend at step 3.

After get the correct url, I try to call `[self.tableView reloadData]`, but it seems like `AController` not get update any more.

What's happend ??? Why ???

I print `self` address in the method `viewWillAppear`, and print `self` address in the position before execute `[self.tableView reloadData]`. Suddenly, I found current `self` is different from the origin one before we present the `UIImagePickerController`. Current `self` may get re-allocated, it's address has changed, and the origin one may be get deallocated by the iOS system.

But I still don't know how to find the correct way to solve the problem ... How to avoid `AController` get deallocated ?

After a short moment, I begin try to popup an `AlertView` when com back from `UIImagePickerController`, so the code like this:

```objc
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info {
  UIAlertController *alertController = createADummyController;
  [self presentViewController:alertController animated:NO completion:nil];
}
```

Then, `Build`, `Run`, ..., the `UIAlertController` not popup, instead, the XCode print the following warning info:

```
Warning: Attempt to present <UIAlertController: 0x7ff70a7614f0> on <AController: 0x7ff70a63de20> whose view is not in the window hierarchy!
```

What's the Funk ???? ... Think after a short moment, I noticed that the fact that current `self` is not a legal one. But the `AController` is visible, how can I get the current valid `self` or visible controller ? Suddenly, I rememberd I have store current visible controller when the method `didFinishLaunchinigWithOptions` get called in the `AppDelegate` class, so ... I know how to achive my goal ^_^

```
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *,id> *)info {
  UIAlertController *alertController = createADummyController;
  [self presentViewController:alertController animated:NO completion:nil];
}

- (AController*)currentVisibleController {
  AppDelegate *appDelegate = [[UIApplication sharedApplication] delegate];
  UIViewController *visibleController = appDelegate.visibleController;
  AController *aController = [self findControllerByVisibleController:visibleController];
  return aController;
}

- (AController*)findControllerByVisibleController:(UIViewController*)visibleController {
  // find AController by visibleController
  return find;
}
```