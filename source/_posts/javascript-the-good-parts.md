---
title: "javascript: The Good Parts"
layout: post
date: 2015-10-17
tags:
- javascript
categories:
- software
---

## part 2: Syntax

### Number

```javascript
//64-bits
1 == 1.0
100 == 1e2
isNaN(number)
Infinity
```

## part 3: Objects

### Object Literals

```javascript
var empty_object = {};
var stooge = {
    "first-name": "Jerome",
    "last-name": "Howard",
}
```

### Retrieval

```javascript
stooge["first-name"]
flight.departure.IATA

stooge["middle-name"] //undefined
```

### Reference

Objects passed by reference, they will never be copied.

```javascript
Object.prototype
```

### Reflection

```javascript
typeof xxx
```

- 'number'
- 'string'
- 'object'
- 'undefined'
- 'function'

```javascript
flight.hasOwnProperty('number');
```

### Enumeration

var name;
for (name in another_stooge) {
    if (typeof anotyer_stooge[name] !== 'function') {
       xxx
    }
}

### Delete

```javascript
delete another_stooge.nickname;
```

### Global Abatement

```javascript
var MYAPP = {};
MYAPP.stooge = {};
MYAPP.flight = {};
```

## Functions

### Function Objects

```javascript
Function.prototype
```

### Function Literal

```javascript
var add = function(a, b) {
    return a+b;
}
```

### Invocation

Each function will receive two extra arguments: `this` and `arguments`

#### The Method Invocation Pattern

```javascript
var myobject = {
    value: 0,
    increment: function(inc) {
    	 this.value += typeof inc === 'number' ? inc : 1;
    }
}
myobject.increment(); //method invocation pattern
```

#### The Function Invocation Pattern

```javascript
var sum = add(3, 4); // Oops --> this was bind to the global object..

myobject.double = function() {
    var that = this;
    var helper = function() {
	that.value = add(that.value, that.value);
    }
    helper();
}
myobject.double();
console.log('myobject.value:', myobject.value);
```

#### The Constructor Invocation Pattern

```javascript
var Quo = function(state) {
    this.status = state;
}
Quo.prototype.get_status = function() {
    return this.status;
}
var myQuo = new Quo("confused");
```

#### The Apply Invocation Pattern

```javascript
var array = [3, 4];
var sum = add.apply(null, array); //the first param represents for the value you want to bind to `this`

var statusObject = {
    status 'A-OK',
};
//although `statusobject` doesn't has `get_status` function, we still could call it.
var status = Quo.prototype.get_status.apply(statusObject);
```

### Arguments

`arguments` is not a real array, it\'s an `array-like` object

```javascript
var sum = function() {
    var i, sum = 0;
    for (i=0; i<arguments.length; i+=1) {
	sum += arguments[i];
    }
    return sum;
}
```

### Exceptions

```javascript
var add = function(a, b) {
    if (typeof a !== 'number' || typeof b !== 'number') {
	throw {
	    name: 'TypeError',
	    message: 'add needs numbers',
	};
    }
    return a+b;
}

var try_it = function() {
    try {
	add("seven");
    } catch (e) {
	document.writeln(e.name + ':' + e.message);
    }
}

try_it();

```

### Augmenting Types

```javascript
// by add function to `Function.prototype`, we can use this `func` in all functions
Function.prototype.method = function(name, func) {
    this.prototype[name] = func;
    return this;
}

Number.method('integer', function() {
    return Math[this < 0 ? 'ceil' : 'floor'](this);
});
(-10/3).integer(); //-3

String.method('trim', function() {
    return this.replace(/^\s+|\s+$/g, '');
});

Function.prototype.method = function (name, func) {
    if (!this.prototype[name]) {
	this.prototype[name] = func;
    }
    return this;
}

```

### Closure

inner function can access outer function\'s params and variables;

```javascript
var myObject = (function(){
    var value = 0; // it runs like private variable
    return {
	increment: function(inc) {
	    value += typeof inc === 'number' ? inc : 1;
	},
	getValue: function() {
	    return value;
	}
    }
}());

var quo = function(status) {
    return {
	get_status: function() {
	    return status;//private
	}
    }
};
var myQuo = quo("amazed");
myQuo.get_status();

```

**note:**inner function can visit outer function\'s variable, and without copy.
