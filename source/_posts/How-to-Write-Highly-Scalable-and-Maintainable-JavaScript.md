---
layout: post
title: "How to Write Highly Scalable and Maintainable JavaScript"
date: 2015-11-14
tags: 
- javascript
- software design
categories:
- javascript
---

## [Namespacing](http://www.innoarchitech.com/scalable-maintainable-javascript-namespacing/)

```javascript
var myMasterNS = myMasterNS || {}; 
myMasterNS.mySubNS = myMasterNS.mySubNS || {}; 

myMasterNS.mySubNS.someFunction = function(){
	//insert logic 
};
```

## [Modules](http://www.innoarchitech.com/scalable-maintainable-javascript-modules/)

实现`JavaScript Module`有很多模式，每种模式都有它自己的优缺点，没有哪一种是最好的。

- Pojo module pattern

一个简单的JavaScript对象其实就可以看做一个JavaScript模块。但是请注意，因为没有`closure`，下面这个例子实际上是污染了global namespace.

```javascript
var myModule = {
	propertyEx: "This is a property on myModule",
	functionEx: function() {
		//insert functionality here
	}
}
```

- Scoped module pattern

为了创建一个闭包，并确保所有的变量和方法在自己的模块中，一些专业开发人员通常会使用IIFE(immediately-invoked function expression)来封装一个模块。

```javascript
var myModule = (function () { 
	var module;

	module.varProperty = "This is a property on myModule"; 
	module.funcProperty = function(){
		//insert code here 
	};

	return module; 
})();
```

- Revealing module pattern

Revealing module pattern 能够隐藏私有变量，并暴漏公开接口。

```javascript
var myRevealingModule = (function () { 

	var privateVar = "Alex Castrounis", 
		publicVar = "Hi!"; 

	function privateFunction() { 
		console.log( "Name:" + privateVar ); 
	} 
	
	function publicSetName( strName ) { 
		privateVar = strName; 
	} 
	
	function publicGetName() {
		privateFunction(); 
	} 
	
	return { 
		setName: publicSetName, 
		greeting: publicVar, 
		getName: publicGetName 
	};

})();
```

- Prototype Pattern

一种拥抱JavaScript prototypal特性的模式

```javascript
var myPrototypeModule = (function (){ 

	var privateVar = "Alex Castrounis", 
		count = 0; 
		
	function PrototypeModule(name){
		this.name = name; } 
	
	function privateFunction() {
		console.log( "Name:" + privateVar );
		count++; 
	} 
	
	PrototypeModule.prototype.setName = function(strName){
		this.name = strName; 
	};
	
	PrototypeModule.prototype.getName = function(){
		privateFunction(); 
	}; 
	
	return PrototypeModule; 

})();
```

一篇好文章：[JavaScript Module Pattern In Depth](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)

一本好书：[Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)

## [Coupling](http://www.innoarchitech.com/scalable-maintainable-javascript-coupling/)

Order 和 Delivery 模块高耦合

```javascript
// Order module definition 
var orderModule = (function() {
	var module = {}, 
		deliveries = myApp.deliveryModule; 
		
	module.createOrder = function(orderData) {
		var orderResult; 
		orderResult = // Code to actually create the order 
		orderResult.estimatedDeliveryTime = deliveries.getDeliveryTime(orderData); 
		return orderResult; 
	}; 

	return module; 

})();
```

Patterns to Reduce Coupling: `observer pattern`, `Pub/Sub`, `Publish/Subscribe`

```javascript
document.addEventListener("DOMContentLoaded", function(event) {
	var orderModule = (function() {
		var orders = {}, 
		EST_DELIVERY = 'current estimated delivery time', 
		estimatedDeliveryTime; 
		
		PubSub.subscribe(EST_DELIVERY, function(msg, data) {
			console.log(msg); 
			estimatedDeliveryTime = data; 
		});
	
		return orders; 
	})();

	var deliveryModule = (function() {
		var deliveries = {}, 
		EST_DELIVERY = 'current estimated delivery time';
		deliveries.getEstimatedDeliveryTime = function() {
			var estimatedDeliveryTime = 1; // Hard-coded to 1 hour, but likely an API call. 
			PubSub.publish(EST_DELIVERY, estimatedDeliveryTime); 
		}; 
		return deliveries; 
	})();

	deliveryModule.getEstimatedDeliveryTime(); 
});
```
