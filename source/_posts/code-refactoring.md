---
title: "Refactoring: Improving the Design of Existing Code"
layout: post
date: 2015-11-13
tags:
- refactor
categories:
- software design
---

## 重构原则

### 为何重构

- 改进软件设计
- 使软件更容易理解
- 帮助找到bug
- 提高编程速度

### 何时重构

我们希望程序：容易阅读，所有逻辑都只在唯一地点指定，新的改动不会危及现有行为，尽可能简单的表达条件逻辑。

- 添加功能时重构
- 修补错误时重构
- 复审代码时重构

## 代码的不良设计

### Duplicated Code (重复代码)

- 同一个类的两个函数含有相同的表达式：Extract Method
- 两个互为兄弟的子类内含有相同的表达式：Extract Method, 然后Pull Up Method到父类中，可能发现使用Form Template Method获得一个Template Method设计模式
- 两个互不相关的类出现相同代码：对其中一个使用Extract Class，将重复代码提炼到一个独立类中

### Long Method (函数过长)

OO语言几乎已经完全免除了进程内的函数调用开销，拥有短函数的对象活的时间会更长。

- 99%的场合里只要用Extract Method就可以将函数变小
- 如果生成了许多临时变量，那么考虑Replace Temp with Query
- 参数列表过长：Introduce Parameter Object 和 Preserve Whole Object
- 杀手锏：Replace Method with Method Object

提炼的技巧：

- 如何决定提炼哪一段代码呢？寻找注释点。就算只有一行，如果需要注释说明的话，也值得将它提炼到独立函数中去。
- 条件表达式和循环也常常是提炼的信号: Decompose Conditional处理条件表达式，循环：可以将循环和其内的代码提炼到一个独立的函数中

### Large Class (类过大)

- Extract Class
- Extract Subclass
- Extract Interface

### Long Parameter List (过长参数列)

- Replace Parameter with Method
- Preserve Whole Object
- Introduce Parameter Object

### Divergent Change (发散式变化)

什么叫做Divergent: 某个类经常因为不同的原因在不同的方向上发生变化. 

"如果新加入一个数据库，我必须修改这三个函数"，"如果新出现一种金融工具，我必须修改这四个函数"

针对某一外界变化的所有相应修改，都只应该发生在单一类中，Extract Class

### Shotgun Surgery (霰弹式修改)

每遇到某种变化，你都必须在许多不同的类内做出许多小修改: Move Method 或者 Move Field 把所有需要修改的代码放进同一个类，如果眼瞎没有合适的类可以放置这些代码，那么就创造一个，通常使用Inline Class可以把一系列相关行为放进同一个类.

- Divergent Change: 一个类受多种变化影响
- Shotgun Surgery: 一种变化引发多个类相应修改

### Feature Envy (依恋情结)

一个函数用到了几个类的功能，那么它应该被放置于何处？

判断哪个类拥有最多被此函数使用的数据，然后把这个函数和那些数据摆在一起。最根本的原则：把总是一起变化的东西放在一块儿

### Data Clumps (数据泥团)

两个类中相同的字段，许多函数签名中相同的参数，这些总是一起出现的数据应该拥有它们自己的对象

Extract Class, Introduce Parameter Object, Preserve Whole Object. 不必太久，所有的类都将在它们小小的社会中充分发挥价值。

### Primitive Obsession (基本类型偏执)

- Replace Data Value with Object
- Replace Type Code with Class
- Replace Type Code with Subclass
- Replace Type Code with State/Strategy

### Switch Statements (switch惊悚现身)

面向对象的一个最明显的特征：少用switch语句：同样的switch语句散布于不同地方

- Extract Method 将 switch 语句提炼到一个独立函数中，然后再以 Move Method 将它搬移到需要多态性的那个类里。此时必须决定是否使用：Replace Type Code with SubClass 或者 Replace Type Code with State/Strategy. 一旦这样的继承结构完成之后，那么就可以使用Replace Conditional with Polymorphism了
- 单一函数的话，那么考虑Replace Parameter with Explicit Methods

### Parallel Inheritance Hierarchies (平行继承体系)

Shotgun Surgery 的特殊情况：一旦你为某个类增加一个子类，必须也为另外一个类相应增加一个子类。

- 让一个继承体系的实例引用另一个继承体系的实例

### Lazy Class (冗赘类)

一个类的存在不值其身价，那么就应该消失

- Collapse Hierarchy
- 几乎没用的组件：Inlinc Class 来对付它们

### Speculative Generality (夸夸其谈未来性)

- 某个抽象类其实并没有太大作用：Collapse Hierarchy
- 不必要的委托：Inline Class
- 参数未用上：Remove Parameter
- 函数名称带有多余的抽象：Rename Method, 让它现实一点

### Temporary Field (令人迷惑的暂时字段)

某个对象的某个实例变量仅为某种特定情况而设置。

- Extract Class

### Message Chains (过度耦合的消息链)

一长串的调用链: `a.b().c().d()...`

- Hide Delegate
- 先观察消息链最终得到的对象是用来干什么的，然后考虑Move Method将这个函数放到一个独立函数中

### Middle Man (中间人)

对象基本特征之一：封装，但是封装往往伴随着委托。某个类借口有一半的函数都委托给其他类：这就是过度使用委托

- Remove Middle Man: 直接和真正负责的对象打交道

### Inappropriate Intimacy (过于亲密的关系)

两个类耦合性太强

- Move Method, Move Field
- Extract Class
- Hide Delegate

### Alternative Classes with Different Interfaces (易曲同工的类)

两个函数有着不同签名，却做同一件事情

- Rename Method

### Incomplete Library Class (不完美的类库)

库的设计很艰巨，没有未卜先知的能力

- Introduce Foreign Method
- Introduce Local Extension

### Data Class (幼稚的数据类)

纯数据容器类：想让它们参与整个系统的工作，那么必须承担一定的责任

### Refused Bequest (被拒绝的遗赠)

子类如果不想继承超类的函数或者数据，那么应该怎么办呢？继承体系设计错误。

- 为子类新建一个兄弟类，再运用Push Down Method和Push Down Field把所有用不到的函数下推给那个兄弟

### Comments (过多的注释)

当你感觉需要撰写注释时，请先尝试重构，试着让所有注释都变得多余。

## 构筑测试体系

- "每天我都会添加一些新的功能，同时也添加相应的测试。那些日子里，我很少花一分钟以上的时间在调试上面"

- "不要因为测试无法捕捉所有bug就不写测试，因为测试的确可以捕捉到大多数bug"

## 重构列表

这本书就是作者重构的笔记

## 重新组织函数

### Extract Method

你有一段代码可以被组织在一起并独立出来. 

**将这段代码放进一个独立函数中，并让函数名称解释该函数的用途**

"函数多长才合适？在我看来，长度不是问题，关键在于函数名称和函数本体之间的语义距离"

### Inine Method

一个函数的本体与名称同样清楚易懂

**在函数调用点插入函数本体，然后移除该函数**

```java
int getRating() {
	return (moreThanFiveLateDeliveries()) ? 2 : 1;
}
boolean moreThanFiveLateDeliveries() {
	return _numberOfLateDeliveries > 5;
}
```

重构之后
```java
int getRating() {
	return (_numberOfLateDeliveries > 5) ? 2 : 1;
}
```

## Inline Temp

## Replace Temp with Query

**将临时变量的所有引用点替换为对新函数的调用**

```java
double basePrice = _quantity * _itemPrice;
if (basePrice > 1000) 
	return basePrice * 0.95
else
	return basePrice * 0.98
```

重构之后
```java
if (basePrice() > 1000)
	return basePrice() * 0.95
else 
	return basePrice() * 0.98
double basePrice() {
	return _quantity * _itemPrice;
}
```

你可能会担心性能问题。和其它性能问题一样，它十有八九不会造成任何影响，即使出了问题，也可以在优化时期解决它。

### Introduce Explaining Variable

你有一个复杂的表达式。

**将该复杂表达式(或者其中一部分)的结果放进一个临时变量，以变量名称解释用途**

```java
if ((platform.toUpperCase().indexOf("MAC") > -1) &&
	(browser.toUpperCase().indexOf("IE")) &&
	wasInitialized() && resize > 0) {
	// do something
}
```

重构之后
```java
final boolean isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
final boolean isIEBrowser = browser.toUpperCase().indexOf("IE");
final boolean wasResized = resize > 0;
if (isMacOs && isIEBrowser && wasInitialized() && wasResized) {
}
```

### Split Temporary Variable

你的程序有某个临时变量被赋值超过一次，它既不是循环变量，也不被用于收集计算结果。

**针对每次赋值，创造一个独立、对应的临时变量**

### Remove Assignments to Parameters

代码对一个参数赋值(用以修正参数)，出参数的语言不用遵守这条规则

**以一个临时变量取代该参数的位置**

### Replace Method with Method Object

你有一个大型函数，其中对局部变量的使用使你无法采用Extract Method

**将这个函数放进一个单独对象中，如此依赖局部变量就成为了对象内的字段。然后你可以在同一个对象中将这个大型函数分解为多个小型函数。**

![Refactor-replace-method-with-method-object](/assets/image/refactor-replace-method-with-method-object.png)

### Substitute Algorithm

你想将某个算法替换为另一个更清晰的算法

**将函数本体替换为另一个算法**

![refactor-substitute-algorithm](/assets/image/refactor-substitute-algorithm.png)

## 在对象之间搬移特性

"在对象的设计过程中，决定把责任放在哪儿即使不是最重要的事情，也是最重要的事情之一。我使用对象技术已经十多年了，但还是不能决定一开始就保证做对。这曾经让我很苦恼，但现在我知道，在这种情况下，可以运用重构，改变自己原先的设计。"

### Move Method

你的程序中，有个函数与其所驻类之外的另一个类进行更多交流：调用后者，或者被后者调用

**在该函数最常引用的类中建立一个有着类似行为的新函数。将旧函数变成一个单纯的委托函数，或是将旧函数完全移除**

![refactor-move-method](/assets/image/refactor-move-method.png)

### Move Field

你的程序中，某个字段被其所驻类之外的另一个类更多的用到

**在目标类新建一个字段，修改源字段的所有用户，令它们改用新字段**

![refactor-move-field](/assets/image/refactor-move-field.png)

随着系统发展，这个星期看似合理而正确的设计决策，到了下个星期可能不再正确。

### Extract Class

某个类做了应该由两个类做的事

**建立一个新类，将相关的字段和函数从旧类搬移到新类**

![refactor-extract-class](/assets/image/refactor-extract-class.png)

### Inline Class

某个类没有做太多的事情

**将这个类的所有特性搬移到另外一个类中，然后移除原类**，Inline Class 和 Extract Class 恰好相反

![refactor-inline-class](/assets/image/refactor-inline-class.png)

### Hide Delegate

客户通过一个委托类来调用另外一个对象

**在服务上建立客户所需的所有函数，用以隐藏委托关系**，每个类应该尽可能少的了解系统其它部分，封装是对象的最关键特征之一。

![refactor-hide-delegate](/assets/image/refactor-hide-delegate.png)

### Remove Middle Man

某个类做了过多的简单的委托动作。每当客户使用委托类的新特性的时候，你就必须在服务端添加一个简单委托函数，随着受托类的特性功能越来越多，服务类慢慢的变成了一个"中间人",此时应该让客户直接调用委托类

**让客户直接调用委托类**

![refactor-remove-middle-man](/assets/image/refactor-remove-middle-man.png)

### Introduce Foreign Method

你需要为提供服务的类增加一个函数，但你无法修改这个类

**在客户类中建立一个函数，并以第一参数形式传入一个服务器实例**

![refactor-introduce-foreign-method](/assets/image/refactor-introduce-foreign-method.png)

### Introduce Local Extension

你需要为服务类提供一些额外函数，但你无法修改这个类

**建立一个新类，使他包含这些额外函数。让这个扩展品称为原类的子类或者包装类**

![refactor-introduce-local-extension](/assets/image/refactor-introduce-local-extension.png)

## 重新组织数据

### Self Encapsulate Field

你直接访问一个字段，但与字段之间的耦合关系逐渐变得笨拙。

**为这个字段简历取值/设值函数，并且只以这些函数来访问字段**

![refactor-self-encapulate-field](/assets/image/refactor-self-encapulate-field.png)

### Replace Data Value with Object

你有一个数据项，需要与其它数据和行为一起使用才有意义

**将数据变成对象**

![refactor-replace-data-value-with-object](/assets/image/refactor-replace-data-value-with-object.png)

### Change Value to Reference

你从一个类衍生出许多彼此相等的实例，希望将它们替换为同一个对象。

**将这个值对象(new 出来的)变成引用对象(这个对象在别个地方,而且应该是不可变的)**

### Change Reference to Value

你有一个引用对象，很小且不可变，而且不易管理。

**将它变成一个值对象**，值对象应该是不可变的，无论何时何地得到的都应该是同样的结果。但是假如是可变的，你就必须确保对某一个对象的修改会自动更新其它代表相同事物的对象。这太痛苦了，与其如此还不如把它变成引用对象。

### Replace Array with Object

你有一个数组，其中的元素各自代表不同的东西

**以对象替换数组。对于数组中的每个元素，以一个字段来表示**

### Duplicate Observed Data

**Observer模式**

### Change Unidirectional Association to Bidirectional

两个类都需要使用对方特性，但期间只有一条单向连接。

**添加一个反向指针，并使修改函数能够同时更新两条连接**

### Change Bidirectional Association to Unidirectional

一个类不再需要另外一个类的特性

### Replace Magic Number with Symbolic Constants

![refactor-replace-magic-number-with-symbolic-constant](/assets/image/refactor-replace-magic-number-with-symbolic-constant.png)

### Encapsulate Field

`public`字段设置为`private`，然后提供相应的访问函数

### Encapsulate Collection

有个函数返回一个集合

**让这个函数返回该集合的一个只读副本，并在这个类中提供添加/移除集合元素的函数**

![refactor-encapsulate-collection](/assets/image/refactor-encapsulate-collection.png)

### Replace Record with Data Class

### Replace Type Code with Class

类之中有一个数值类型码，但它并不影响类的行为。

**以一个新的类替换该数值类型码**

![refactor-replace-type-code-with-class](/assets/image/refactor-replace-type-code-with-class.png)

### Replace Type Code with Subclasses

你有一个不可变的类型码，它会影响子类的行为

**以子类取代这个类型码**

![refactor-replace-type-code-with-subclass](/assets/image/refactor-replace-type-code-with-subclass.png)

### Replace Type Code with State/Strategy

你有一个类型码，它会影响类的行为，但你无法通过继承手法消除它

**以状态对象取代类型码**

![refactor-replace-type-code-with-state-strategy](/assets/image/refactor-replace-type-code-with-state-strategy.png)

### Replace Subclass with Fields

你的各个子类的唯一差别只在"返回常量数据"的函数身上

**修改这些函数，使它们返回超类中的某个(新增)字段，然后销毁子类**建立子类的目的，是为了增加新特性或变化其行为。如果子类中只有常量函数，实在没有足够的存在价值

![refactor-replace-subclass-with-fields](/assets/image/refactor-replace-subclass-with-fields.png)

## 简化条件表达式

### Decompose Conditional

![refactor-decompose-conditional](/assets/image/refactor-decompose-conditional.png)

### Consolidate Conditional Expression

![refactor-consolidate-compose-expression](/assets/image/refactor-consolidate-compose-expression.png)

### Consolidate Duplicate Conditional Fragments

![refactor-consolidate-duplicate-conditional-fragments](/assets/image/refactor-consolidate-duplicate-conditional-fragments.png)

### Remove Control Flag

**以`break`或`return`语句取代控制标记**

### Replace Nested Conditional with Guard Clauses

![refactor-replace-nested-conditional-with-guard-clauses](/assets/image/refactor-replace-nested-conditional-with-guard-clauses.png)

### Replace Conditional with Polymorphism

![refactor-replace-conditional-with-polymorphism](/assets/image/refactor-replace-conditional-with-polymorphism.png)

### Introduce Null Object

![refactor-introduce-null-object](/assets/image/refactor-introduce-null-object.png)

### Introduce Assertion

## 简化函数调用

### Rename Method

### Add Parameter

### Remove Parameter

### Separate Query from Modifier

某个函数既返回对象状态值，又修改对象状态

**建立两个不同的函数，其中一个负责查询，另一个负责修改**

![refactor-separate-query-from-modifier](/assets/image/refactor-separate-query-from-modifier.png)

### Parameterize Method

![refactor-parameterize-method](/assets/image/refactor-parameterize-method.png)

### Replace Paremeter with Explicit Methods

![refactor-replace-parameter-with-explicit-methods](/assets/image/refactor-replace-parameter-with-explicit-methods.png)

### Preserve Whole Object

![refactor-preserve-whole-object](/assets/image/refactor-preserve-whole-object.png)

### Repleace Parameter with Methods

如果函数可以通过其它途径获得参数值，那么它就不应该通过参数取得该值。过长的参数列会增加程序阅读者的理解程度。

![refactor-replace-parameter-with-methods](/assets/image/refactor-replace-parameter-with-methods.png)

### Introduce Parameter Object

某些参数总是很自然的同时出现

![refactor-introduce-parameter-object](/assets/image/refactor-introduce-parameter-object.png)

### Remove Setting Method

类中的某个字段应该在对象创建的时候被设置，然后就不再改变。

### Hide Method

有一个函数，从来没有被其它类用到过

### Replace Constructor with Factory Method

![refactor-replace-constructor-with-factory-method](/assets/image/refactor-replace-constructor-with-factory-method.png)

### Encapsulate Downcast

![refactor-encapsulate-downcast](/assets/image/refactor-encapsulate-downcast.png)

### Replace Error Code with Exception

![refactor-replace-error-code-with-exception](/assets/image/refactor-replace-error-code-with-exception.png)

### Replace Exception with Test

## 处理概括(generalization)关系

### Pull Up Field

![refactor-pull-up-field](/assets/image/refactor-pull-up-field.png)

### Pull Up Method

![refactor-pull-up-method](/assets/image/refactor-pull-up-method.png)

### Pull Up Constructor Body

### Push Down Method

![refactor-push-down-method](/assets/image/refactor-push-down-method.png)

### Push Down Field

![refactor-push-down-field](/assets/image/refactor-push-down-field.png)

### Extract Subclass

类中的某些特性只被某些(而非全部)实例用到

![refactor-extract-subclass](/assets/image/refactor-extract-subclass.png)

### Extract Superclass

![refactor-extract-superclass](/assets/image/refactor-extract-superclass.png)

### Extract Interface

![refactor-extract-class](/assets/image/refactor-extract-class.png)

### Collapse Hierarchy

超类和子类无太大区别

![refactor-collapse-hierarchy](/assets/image/refactor-collapse-hierarchy.png)

### Form TemPlate Method

![refactor-form-template-method](/assets/image/refactor-form-template-method.png)

### Replace Inheritance with Delegation

某个子类只使用超类接口的一部分，或者根本不需要继承而来的数据

![refactor-replace-inheritance-with-delegation](/assets/image/refactor-replace-inheritance-with-delegation.png)

### Replace Delegation with Inheritance

你在两个类中使用委托关系，并经常为整个接口编写许多极简单的委托函数

![refactor-replace-delegation-with-inheritance](/assets/image/refactor-replace-delegation-with-inheritance.png)

## 大型重构

### Tease Apart Inheritance (梳理并分解继承体系)

某个继承体系同时承担两项责任

**建立两个继承体系，并通过委托关系让其中一个可以调用另一个**

![refactor-tease-apart-inheritance](/assets/image/refactor-tease-apart-inheritance.png)

### Convert Procedural Design to Objects

### Seprate Domain from Presentation

### Extract Hierarchy

你有某个类做了太多工作，其中一部分工作是以大量条件表达式完成的

**建立子类，以一个子类表示一种特殊情况**

![refactor-extract-hierarchy](/assets/image/refactor-extract-hierarchy.png)

## 重构，复用与现实

### 为什么开发者不愿意重构它们的程序

- 你不知道如何重构
- 这些利益是长远的，何必现在付出努力呢？
- 额外工作
- 可能破坏现有程序
