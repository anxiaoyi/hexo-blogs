---
title: "Learning JavaScript Design Patterns"
layout: post
date: 2015-11-15
tags:
- javascript design patterns
categories:
- javascript
---

# [Learning JavaScript Design Patterns](http://addyosmani.com/resources/essentialjsdesignpatterns/book/)

## JavaScript Design Patterns

### Constructor Pattern

创建对象的三种常见的方式：

```javascript
// Each of the following options will create a new empty object:
 
var newObject = {};

// or
var newObject = Object.create( Object.prototype );

// or
var newObject = new Object();
```

## Design Patterns in jQuery

### The Composite Pattern

**The Composite Pattern** describes a group of objects that can be treated in the same way a single instance of an object may be.

```javascript
// Single elements
$( "#singleItem" ).addClass( "active" );
$( "#container" ).addClass( "active" );

// Collections of elements
$( "div" ).addClass( "active" );
$( ".item" ).addClass( "active" );
$( "input" ).addClass( "active" );
```

### The Adapter Pattern

**The Adapter Pattern** translates an interface for an object or class into an interface compatible with a specific system.

One example of an adapter we may have used is the jQuery `jQuery.fn.css()` method. It helps normalize the interfaces to how styles can be applied across a number of browsers, making it trivial for us to use a simple syntax which is adapted to use what the browser actually supports behind the scenes:

```javascript
// Cross browser opacity:
// opacity: 0.9; Chrome 4+, FF2+, Saf3.1+, Opera 9+, IE9, iOS 3.2+, Android 2.1+
// filter: alpha(opacity=90); IE6-IE8
 
// Setting opacity
$( ".container" ).css( { opacity: .5 } );

// Getting opacity
var currentOpacity = $( ".container" ).css('opacity');
```

## The Facade Pattern

**Facade Pattern** provides a simpler abstracted interface to a larger (potentially more complex) body of code.

The following are facades for jQuery's `$.ajax()`:

```javascript
$.get( url, data, callback, dataType );
$.post( url, data, callback, dataType );
$.getJSON( url, data, callback );
$.getScript( url, callback );
```

## The Observer Pattern

**Observer (Publish/Subscribe) pattern**. This is where the objects in a system may subscribe to other objects and be notified by them when an event of interest occurs.

In earlier versions of the library, access to these custom events was possible using `jQuery.bind()` (subscribe), `jQuery.trigger()` (publish) and `jQuery.unbind()` (unsubscribe), but in recent versions this can be done using `jQuery.on()`, `jQuery.trigger()` and `jQuery.off()`.

```javascript
// Equivalent to subscribe(topicName, callback)
$( document ).on( "topicName", function () {
    //..perform some behaviour
});

// Equivalent to publish(topicName)
$( document ).trigger( "topicName" );

// Equivalent to unsubscribe(topicName)
$( document ).off( "topicName" );
```

你可以对它们进行一下封装：

```javascript
(function( $ ) {
 
   var o = $({});
    
   $.subscribe = function() {
		o.on.apply(o, arguments);
   };

   $.unsubscribe = function() {
		o.off.apply(o, arguments);
   };

   $.publish = function() {
		o.trigger.apply(o, arguments);
   };

}( jQuery ));
```

## The Iterator Pattern

The **Iterator** is a design pattern where iterators (objects that allow us to traverse through all the elements of a collection) access the elements of an aggregate object sequentially without needing to expose its underlying form.

```javascript
$.each( ["john","dave","rick","julian"], function( index, value ) {
  console.log( index + ": "" + value);
});
   
$( "li" ).each( function ( index ) {
  console.log( index + ": " + $( this ).text());
});
```
