---
title: "JavaScript Design Patterns"
layout: post
date: 2015-10-25
tags:
- javascript
categories:
- software
---

## Objects in JavaScript

```javascript
var functionObject = {
    greeting: "hello world",
	doThings: function() {
		console.log(this.greeting);
		this.doOtherThings();
	},
	doOtherThings: function() {
		console.log(this.greeting.split(""));
	},
}
functionObject.doThings();

//不能这样调用，因为这是object, not a function
//var o = new functionObject();
```

你可以下面这样定义的话，就可以使用`new`了

```javascript
var ThingDoer = function(){
 	this.greeting = "hello world";
  	this.doThings = function() {
	   console.log(this.greeting);
   		 this.doOtherThings();
	};
	this.doOtherThings = function() {
		console.log(this.greeting.split("").reverse().join(""));
	};
}
var instance = new ThingDoer();
instance.doThings();//prints hello world then dlrow olleh
```

但是，请注意，这样是极其耗费内存的！请看下面这个例子：

```javascript
var Castle = function(name){
 	this.name = name;
  	this.build = function() {
   		console.log(this.name);
    };
}
var instance1 = new Castle("Winterfell");
var instance2 = new Castle("Harrenhall");
instance1.build = function(){ console.log("Moat Cailin");}
instance1.build(); //prints "Moat Cailin"
instance2.build(); //prints "Harrenhall" to the console
```

`instance1`的`build`方法改变了，`instance2`的`build`方法竟然没有改变！！！所以正确的方法应该是下面这样定义：

```javascript
var Castle = function(name){
 	this.name = name;
}
Castle.prototype.build = function(){ console.log(this.name);}
var instance1 = new Castle("Winterfell");
instance1.build();
```

将`functions`绑定到`prototype`的优势就是只有一份拷贝，节省内存～。但是需要注意的是：`name`属性还是直接绑定到`instance`上的。

### 继承

下面再说说继承。在javascript中没有继承，但是我们可以讲一些方法组合进来～也就是说讲父类的prototype上面的所有属性拷贝到子类的prototype上来。

```javascript
var Castle = function(){};
Castle.prototype.build = function(){
	console.log("Castle build");
};

var Winterfell = function(){};
Winterfell.prototype.build = Castle.prototype.build;
Winterfell.prototype.addGodsWood = function(){}

var winterfell = new Winterfell();
winterfell.build();
```

这种方法看起来或多或少有点不爽……我们还可以做的更好一些：
```javascript
function clone(source, destination) {
	for (var attr in source.prototype) {
		destination.prototype[attr] = source.prototype[attr];
	}
}
```

这样上面就可以这样继承了：
```javascript
clone(Castle, Winterfell);
```

但其实这样的方案也不成熟，有时候也会失败.

### 模块

```javascript
Westeros = {}; //绑定到全局变量上了
Westeros = Westeros || {}; //更好一点的是提前检查一下
Westeros.Strustures = Westeros.Structures || {};
Westeros.Structures.Castle = function(name) {
	this.name = name;
};

//更简单一点的：
var Castle = (function() {
	function Castle(name) {
		this.name = name;
	}
	Castle.prototype.Build = function() {
		console.log("Castle built:" + this.name);
	};
	return Castle;
})();
Westeros.Structures.Castle = Castle;

//使用这样的架构，继承也变得简单了：
var BaseStructure = (function() {
	function BaseStructure() {
	}
	return BaseStructure;
})();
Structures.BaseStructure = BaseStructure;
var Castle = (function(_super) {
	__extends(Castle, _super);
	function Castle(name) {
		this.name = name;
		_super.call(this);
	}
	Castle.prototype.Build = function() {
		console.log("Castle built:" + this.name);
	};
	return Castle;
})(BaseStructure);

//你也许注意到了，上面使用了一个__extends帮助方法用来继承，下面是其实现
var __extends = this.__extends || function(d, b) {
	for (var p in b) {
		if (b.hasOwnProperty(p)) {
			d[p] = b[p];
		}
	}
	function __() {
		this.constructor = d;
	}
	__.prototype = b.prototype;
	d.prototype = new __();
};
```

下面是我们这个`module`的所有实现：

```javascript
var Westeros;
(function(Westeros) {
	(function(Structures) {
		var Castle = (function() {
			function Castle(name) {
				this.name = name;
			}
			Castle.prototype.Build = function() {
				console.log("Castle built " + this.name);
			}
			return Castle;
		})();
		Structures.Castle = Castle;
	})(Westeros.Structures || (Westeros.Structures = {}));
	var Structures = Westeros.Structures;
})(Westeros || (Westeros = {]));
```

## 创建者模式

### 抽象工厂模式

```javascript

/**
 * 产品1
 */
var KingJoffery = (function() {
	function KingJoffery() {
		console.log('New KingJoffery');
	}
	KingJoffery.prototype.makeDecision = function() {
		console.log('KingJoffery makeDecision');
	};
	KingJoffery.prototype.marry = function() {
		console.log('KingJoffery marry');
	};
	return KingJoffery;
})();

/**
 * 产品2
 */
var LordTywin = (function(){
	function LordTywin() {
		console.log('New LordTywin');
	}
	LordTywin.prototype.makeDecision = function() {
		console.log('LordTywin makeDecision');
	};
	return LordTywin;
})();

/**
 * 工厂1
 */
var LannisterFactory = (function() {
	function LannisterFactory() {
		console.log('New LannisterFactory');
	}
	LannisterFactory.prototype.getKing = function() {
		return new KingJoffery();
	};
	LannisterFactory.prototype.getHandOfTheKing = function() {
		return new LordTywin();
	};
	return LannisterFactory;
})();

/**
 * 工厂2
 */
var TargaryenFactory = (function() {
	function TargaryenFactory() {
	}
	TargaryenFactory.prototype.getKing = function() {
		return new KingAerys();
	};
	TargaryenFactory.prototype.getHandOfTheKing = function() {
		return new LordConnington();
	};
	return TargaryenFactory;
})();


/**
 * 然后再动态决定使用哪个工厂
 */
var CourtSession = (function() {
	function CourtSession(abstractFactory) {
		this.abstractFactory = abstractFactory;
		this.COMPLAINT_THRESHOLD = 10;
	}
	CourtSession.prototype.complaintPresented = function(complaint) {
		if (complaint.severity < this.COMPLAINT_THRESHOLD) {
			this.abstractFactory.getHandOfTheKing().makeDecision();
		} else {
			this.abstractFactory.getKing().makeDecision();
		}
	};
	return CourtSession;
})();

/**
 * 注入不同的工厂，生产不同的产品
 */
 var courtSession1 = new CourtSession(new TargaryenFactory());
 courtSession1.complaintPresented({severity:8});
 courtSession1.complaintPresented({severity:12});

 var courtSession2 = new CourtSession(new LannisterFactory());
 courtSession2.complaintPresented({severity:8});
 courtSession2.complaintPresented({severity:12});

```

## [JavaScript Design Patterns](https://carldanley.com/javascript-design-patterns/)
