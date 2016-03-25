---
title: "The basics concept of object-oriented javascript"
layout: post
date: 2015-07-29
tags:
- javascript
categories:
- javascript
---

# The basics concept of object-oriented javascript

## [the first article](http://code.tutsplus.com/tutorials/the-basics-of-object-oriented-javascript--net-7670)

### two ways to create object:
- Constructor functions

```javascript
function myObject() {
};
```

- Literal notation


```javascript
var myObject = {
};
```

### define methods and properties
- Constructor version:
```javascript
function myObject() {
	this.iAm = 'an object';
	this.whatAmI = function() {
		alert('I am ' + this.iAm);
	}	
}
```

- Literal version:
```javascript
var myobject = {
	iAm: 'an object';
	whatAmI: function() {
		alert('I am ' + this.iAm);
	}
};
```

### how to use:
- Constructor version:
```javascript
myObject.whatAmI();
```

- Literal version:
```javascript
var myNewObject = new myObject();
myNewObject.whatAmI();
```

### To Instantiate or not to Instantiate
when a change is made to an Object Literal it affects that object across the entire script, whereas when a Constructor function is instantiated and then a change is made to that instance, it won't affect any other instances of that object. 

## So what does 'this' reference? 
well in a browser, 'this' references the window object, so technically the window is our global object. If we're inside an object, 'this' will refer to the object itself however if you're inside a function, this will still refer to the window object and likewise if you're inside a method that is within an object, 'this' will refer to the object. 浏览器中:this 指向window, object中:指向object, function中:指向window, object的一个方法中:指向object.

## [The second article](http://javascript.info/tutorial/object-oriented-programming)

在javascript中,继承是prototype-based的,也就是说没有classes,取而代之的是an object inherits from another object.

继承意味着什么:
```javascript
rabbit.__proto__ = animal; //inherit: animal is a prototype of rabbit
```

当孩子与父亲有相同属性的东西时,那么父亲的被忽略

`__proto__` 是不标准版本, 标准版本两种:
- `Object.create(proto[,props])`
```javascript
rabbit = Object.create(animal)// 注意此时rabbit内容为:{}
Object.getPrototypeOf(rabbit) === animal //true
```

- prototype
```javascript
function Rabbit(){
}
Rabbit.protoype = animal;
```

- 跨浏览器的Crossbrowser Object.create(proto)
```javascript
function inherit(proto) {
  function F() {}
  F.prototype = proto
  return new F
}

var rabbit = inherit(animal);
```

上面那段代码在库和框架里面被大量使用

`hasOwnProperty`检查某个属性是否属于自己的prototype
```javascript
function Rabbit(name) {
  this.name = name
}
Rabbit.prototype = { eats: true }
var rabbit = new Rabbit('John')
alert( rabbit.hasOwnProperty('eats') ) // false, in prototype
alert( rabbit.hasOwnProperty('name') ) // true, in object
```

遍历属性:
```javascript
function Rabbit(name) {
  this.name = name
}
Rabbit.prototype = { eats: true }
var rabbit = new Rabbit('John')
for(var p in rabbit) {
  alert (p + " = " + rabbit[p]) // outputs both "name" and "eats"
}
```

下面介绍三种在javascript中实现OOP模式的技术:

### Pseudo-classical pattern 伪类
在这种模式中, object在构造方法中创建,方法被放到prototype中, 所有objects共享__proto__

```javascript
function Hamster() {  }
Hamster.prototype = {
  food: [],
  found: function(something) {
    this.food.push(something)
  }
}

// Create two speedy and lazy hamsters, then feed the first one
speedy = new Hamster()
lazy = new Hamster()

speedy.found("apple")
speedy.found("orange")

alert(speedy.food.length) // 2
alert(lazy.food.length) // 2 (!??)
```

解决它:
```javascript
function Hamster() { 
  this.food = []
}
Hamster.prototype = {
  found: function(something) {
    this.food.push(something)
  }
}

speedy = new Hamster()
lazy = new Hamster()

speedy.found("apple")
speedy.found("orange")

alert(speedy.food.length) // 2
alert(lazy.food.length) // 0(!)
```

调用父类构造器:
```javascript
function Rabbit(name) {
  Animal.apply(this, arguments)
}
```

重写方法:Overriding a method (polymorphism)

```javascript
Rabbit.prototype.sit = function() {
  alert(this.name + ' sits in a rabbity way.')
}
```

//或者更加简单一些
```javascript
Rabbit.prototype.sit = function() {
  alert(this.name + ' sits in a rabbity way.')
}
```

调用父类方法:

```javascript
Rabbit.prototype.sit = function() {
  alert('calling superclass sit:')
Animal.prototype.sit.apply(this, arguments)
}
```

在上面代码中,我们直接在构造器或者方法中调用父亲的东西,其实我们不应该那么做反射会改变父类的name或者引入intermediate class,应该是采用类似`parent.method()`或者`super()`这样的代码...

然而我们可以模拟它们

```javascript
function extend(Child, Parent) {
  Child.prototype = inherit(Parent.prototype)
  Child.prototype.constructor = Child
  Child.parent = Parent.prototype
}

function Rabbit(name) {
  Rabbit.parent.constructor.apply(this, arguments) // super constructor
}

extend(Rabbit, Animal)

Rabbit.prototype.run = function() {
    Rabbit.parent.run.apply(this, arguments) // parent method
    alert("fast")
}
```

私有字段或者方法就是通过在名字前面加上'_'来声明,我这是私有字段:`_prop`, `_method`

静态方法:
```javascript
function Animal() { 
  Animal.count++
}
Animal.count = 0

new Animal()
new Animal()

alert(Animal.count) // 2
```

### All-in-one constructor
The object is fully described by it’s constructor.

声明:
```javascript
function Animal(name) {
  this.name = name
this.run = function() {
    alert("running "+this.name)
  }
}

var animal = new Animal('Foxie')
animal.run()
```

继承:
```javascript
function Rabbit(name) {

  Animal.apply(this, arguments) // inherit

  this.bounce = function() {
    alert("bouncing "+this.name)
  }
}
rabbit = new Rabbit("Rab")
rabbit.bounce() // own method
rabbit.run()    // inherited method
```

调用任意构造器参数:
```javascript
function Rabbit(name) {
Animal.call(this, "Mr. " + name.toUpperCase())
  // ..
}
rabbit = new Rabbit("Rab")
rabbit.run()
```

重写方法:
```javascript
function Rabbit(name) {

  Animal.apply(this, arguments) 

  var parentRun = this.run  // keep parent method

  this.run = function() {  
    alert("bouncing "+this.name)
parentRun.apply(this)  // call parent method
  }
}
rabbit = new Rabbit("Rab")
rabbit.run()    // inherited method
```

私有/保护 方法:
A local function or variable are private. 

```javascript
function Rabbit(name) {

  Animal.call(this, "Mr. " + name.toUpperCase()) 

  var created = new Date()  // private

  function sayHi() {  // private
    alert("I'm talking rabbit " + name)
  }

  this.report = function() {  
    sayHi.apply(this)
    alert("Created at " + created)
  }
}
rabbit = new Rabbit("Rab")
rabbit.report()
```

与pseudo-classical模式相比较:
- rabbit instanceof Animal doesn’t work here. That’s because Rabbit does not inherit from Animal in prototype-sense.
- Slower creation, more memory for methods, because every object carries all methods in it, without prototype as shared storage. But on frontend programming we shouldn’t create many objects. So that’s not a big problem.
- Inheritance is joined with parent constructor call. That’s architectural inflexibility, because one can’t call parent method before parent constructor. Not so fearful though.

- Private methods and properties. That’s safe and fast. Especially because JavaScript compressors shorten them.
- There are no “occasionaly static” properties in prototype.

### Factory constructor pattern

向python-style那样的写法:
```javascript
var animal = Animal("fox")
var rabbit = Rabbit("rab")
```

声明:
返回一个new object
```javascript
function Animal(name) {
    
return {
    run: function() {
      alert(name + " is running!")
    }
  }

}
```

继承:
```javascript
function Rabbit(name) {    
  var rabbit = Animal(name) // make animal

  rabbit.bounce = function() { // mutate
    this.run()
    alert(name + " bounces to the skies! :)")
  }

  return rabbit // return the result
}

var rabbit = Rabbit("rab")
rabbit.bounce()
```

封装:
```javascript
function Bird(name) {
  var speed = 100                    // private prop
  function openWings() { /* ... */ } // private method

  return {
    fly: function() {
      openWings()
      this.move()
    },
    move: function() { /*...*/ }    
  }
}
```

外面的`openWings`方法应该如何调用move方法呢?
```javascript
function Bird(name) {
    
  function doFly() { 
    openWings()
    self.move()
  } // private method


  var self ={
    fly: function() { doFly() },
    move: function() { /*...*/ }    
  }
  return self
}
```

## Early and Late Binding
javascript在运行的时候动态的设置this的值

### 错误使用
```javascript
function Menu(elem) {

  setTimeout(function() {
alert(this)  // window, not menu! 因为setTimeout执行环境是window
  }, 1000)

}

new Menu(document.createElement('div'))
```

另外一个例子:
```javascript
function Menu(elem) {

  elem.onclick = function() {
alert(this)  // elem, not menu!
  }
}
```

再来一个例子:
```javascript
function Menu(elem) {

  function privateMethod() {
alert(this)  // window, not menu!
  }

  // ... call private method
  privateMethod()
}

new Menu(document.createElement('div'))
```

### 通过使用self = this
```javascript
function Menu(elem) {

  var self = this

  setTimeout(function() {
alert(self)  // object! (menu)
  }, 1000)

}

new Menu(document.createElement('div'))
```

### Early binding
我们可以写一段脚本来强制使用this

```javascript
function bind(func, fixThis) {  
  return function() {
    return func.apply(fixThis, arguments)
  }
}
```

如何使用?
```javascript
function Menu(elem) {

  elem.onclick = bind(function() {
    alert(this)  // object! (menu)
  }, this)

}
```

#### Function.prototype.bind
`Function.prototype.bind`方法也可以做相同的事情,大多数浏览器都支持:

```javascript
Function.prototype.bind = Function.prototype.bind || function(fixThis) {
  var func = this  
  return function() {
    return func.apply(fixThis, arguments)
  }
}
```

说实在的,这通常不是什么好的方法,但是修改`Function.prototype`是可以接受的
```javascript
function Menu(elem) {

  setTimeout(function() {
    alert(this)  // object! (menu)
  }.bind(this), 1000)

}

new Menu(document.createElement('div'))
```

不能在FunctionDeclaration中bind
```javascript
function Menu(elem) {

  function privateMethod() {
    alert(this) 
  }.bind(this) // syntax error, can't call method in-place

}
```

但是可以在FunctionExpression中bind
```javascript
function Menu(elem) {

  var privateMethod = function() {
    alert(this) 
  }.bind(this)

  // ... now all fine, "this" is fixed
  privateMethod()
}

new Menu(document.createElement('div'))
```

## [Object-oriented Programming](http://eloquentjavascript.net/1st_edition/chapter8.html)
