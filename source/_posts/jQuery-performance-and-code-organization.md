---
title: "jQuery Performance And Code Organization"
layout: post
date: 2015-11-28
tags:
- jQuery
- performance
- code organization
categories:
- javascript
---

# [jQuery Performance](http://learn.jquery.com/performance/)

## Append Outside of Loops

像下面这样添加元素有很严重的性能问题：

```javascript
$.each( myArray, function(i, item) {
	var newListItem = '<li>' + item + '</li>';
	$('#ballers').append(newListItem);
})
```

解决方法一，先把所有元素添加到`fragment`上，而不是`DOM`上，然后直接将`fragment`添加到`DOM`上(The `DocumentFragment` interface represents a minimal document object that has **no parent**)：

```javascript
var frag = document.createDocumentFragment();
 
$.each( myArray, function( i, item ) {

	var newListItem = document.createElement( "li" );
	var itemText = document.createTextNode( item );

	newListItem.appendChild( itemText );

	frag.appendChild( newListItem );

});

$( "#ballers" )[ 0 ].appendChild( frag );
```

解决方法二，当然是创建字符串了：

```javascript
var myHtml = "";
 
$.each( myArray, function( i, item ) {
	myHtml += "<li>" + item + "</li>";
});

$( "#ballers" ).html( myHtml );
```

## Cache loop length

```javascript
var myLength = myArray.length;
 
for ( var i = 0; i < myLength; i++ ) {
	// do stuff
}
```

## Detach Elements to Work with Them

越少操作DOM就越好：`detach()`方法：allowing you to remove an element from the DOM **while you work with it**

```javascript
var table = $( "#myTable" );
var parent = table.parent();
 
table.detach();

// ... add lots and lots of rows to table

parent.append( table );
```

## Don't Act on Absent Elements

```javascript
// Bad: This runs three functions before it
// realizes there's nothing in the selection
$( "#nosuchthing" ).slideUp();
 
// Better:
var elem = $( "#nosuchthing" );

if ( elem.length ) {
	elem.slideUp();
}

// Best: Add a doOnce plugin.
jQuery.fn.doOnce = function( func ) {
	this.length && func.apply( this );
	return this;
}

$( "li.cartitems" ).doOnce(function() {
   // do something
});
```

## Optimize Selectors

ID-Based Selectors: Beginning your selector **with an ID** is always best

```javascript
// Fast:
$( "#container div.robotarm" );
 
// Super-fast:
$( "#container" ).find( "div.robotarm" );
```

Specificity: `.class tag.class` 这种形式要好许多：

```javascript
// Unoptimized
$("div.data .gonzalez");
// Optimized
$(".data td.gonzalez")
```

Avoid Excessive Specificity:

```javascript
$( ".data table.attendees td.gonzalez" );
 
// Better: Drop the middle if possible.
$( ".data td.gonzalez" );
```

Avoid the Universal Selector:

```javascript
$( ".buttons > *" ); // Extremely expensive.
$( ".buttons" ).children(); // Much better.
 
$( ".category :radio" ); // Implied universal selection.
$( ".category *:radio" ); // Same thing, explicit now.
$( ".category input:radio" ); // Much better.
```

## Use Stylesheets for Changing CSS on Many Elements

If you're changing the CSS of more than 20 elements using `.css()`, consider **adding a style tag** to the page instead for a nearly 60% increase in speed.

```javascript
// Fine for up to 20 elements, slow after that:
$( "a.swedberg" ).css( "color", "#0769ad" );
 
// Much faster:
$( "<style type=\"text/css\">a.swedberg { color: #0769ad }</style>")
	.appendTo( "head" );
```

## Don’t Treat jQuery as a Black Box

多看`jQuery`[源代码](https://code.jquery.com/jquery/)

***

# [Code Organization](http://learn.jquery.com/code-organization/)

## 代码组织核心概念

### 核心概念

- Your code should be divided into units or functionality -- modules, services, etc. 不要把你所有代码扔到`$(document).ready()`里面

- Don't repleat your self. 提取相同的代码，使用继承来避免重复代码

- 不是所有的代码一定要包含`DOM`，尽管`jQuery`是以`DOM`为中心的

- 松耦合，每一个functionality应该都能单独存在，在这些单独的模块之间应该通过messaging system例如`pub/sub`模式来沟通，尽量避免直接沟通

### 封装

#### The Object Literal

将下面这段`jQuery`代码转变为`The Object Literal`模式：

```javascript
// Clicking on a list item loads some content using the
// list item's ID, and hides content in sibling list items
$( document ).ready(function() {
	$( "#myFeature li" ).append( "<div>" ).click(function() {
		var item = $( this );
		var div = item.find( "div" );
		div.load( "foo.php?item=" + item.attr( "id" ), function() {
			div.show();
			item.siblings().find( "div" ).hide();
		});
	});
});
```

转变之后：

```javascript
// Using an object literal for a jQuery feature
var myFeature = {
    init: function( settings ) {
        myFeature.config = {
            items: $( "#myFeature li" ),
            container: $( "<div class='container'></div>" ),
            urlBase: "/foo.php?item="
        };
 
        // Allow overriding the default config
        $.extend( myFeature.config, settings );
 
        myFeature.setup();
    },
 
    setup: function() {
        myFeature.config.items
            .each( myFeature.createContainer )
            .click( myFeature.showItem );
    },
 
    createContainer: function() {
        var item = $( this );
        var container = myFeature.config.container
            .clone()
            .appendTo( item );
        item.data( "container", container );
    },
 
    buildUrl: function() {
        return myFeature.config.urlBase + myFeature.currentItem.attr( "id" );
    },
 
    showItem: function() {
        myFeature.currentItem = $( this );
        myFeature.getContent( myFeature.showContent );
    },
 
    getContent: function( callback ) {
        var url = myFeature.buildUrl();
        myFeature.currentItem.data( "container" ).load( url, callback );
    },
 
    showContent: function() {
        myFeature.currentItem.data( "container" ).show();
        myFeature.hideContent();
    },
 
    hideContent: function() {
        myFeature.currentItem.siblings().each(function() {
            $( this ).data( "container" ).hide();
        });
    }
};
 
$( document ).ready( myFeature.init );
```

#### The Module Pattern

Module Pattern 克服了 Object Literal 模式的几个限制：

```javascript
// Using the module pattern for a jQuery feature
$( document ).ready(function() {
    var feature = (function() {
        var items = $( "#myFeature li" );
        var container = $( "<div class='container'></div>" );
        var currentItem = null;
        var urlBase = "/foo.php?item=";
 
        var createContainer = function() {
            var item = $( this );
            var _container = container.clone().appendTo( item );
            item.data( "container", _container );
        };
 
        var buildUrl = function() {
            return urlBase + currentItem.attr( "id" );
        };
 
        var showItem = function() {
            currentItem = $( this );
            getContent( showContent );
        };
 
        var showItemByIndex = function( idx ) {
            $.proxy( showItem, items.get( idx ) );
        };
 
        var getContent = function( callback ) {
            currentItem.data( "container" ).load( buildUrl(), callback );
        };
 
        var showContent = function() {
            currentItem.data( "container" ).show();
            hideContent();
        };
 
        var hideContent = function() {
            currentItem.siblings().each(function() {
                $( this ).data( "container" ).hide();
            });
        };
 
        items.each( createContainer ).click( showItem );
 
        return {
            showItemByIndex: showItemByIndex
        };
    })();
 
    feature.showItemByIndex( 0 );
});
```

## Beware Anonymous Functions

Anonymous bound everywhere, 难debug, 维护， 测试和重用，使用object literal去好好组织一下：
```javascript
// BAD
$( document ).ready(function() {
 
    $( "#magic" ).click(function( event ) {
        $( "#yayeffects" ).slideUp(function() {
            // ...
        });
    });
 
    $( "#happiness" ).load( url + " #unicorns", function() {
        // ...
    });
 
});
 
// BETTER
var PI = {
 
    onReady: function() {
        $( "#magic" ).click( PI.candyMtn );
        $( "#happiness" ).load( PI.url + " #unicorns", PI.unicornCb );
    },
 
    candyMtn: function( event ) {
        $( "#yayeffects" ).slideUp( PI.slideCb );
    },
 
    slideCb: function() { ... },
 
    unicornCb: function() { ... }
 
};
 
$( document ).ready( PI.onReady );
```

## Keep Things DRY

Don't repeat yourself; if you're repeating yourself, **you're doing it wrong**.

```javascript
// BAD
if ( eventfade.data( "currently" ) !== "showing" ) {
    eventfade.stop();
}
 
if ( eventhover.data( "currently" ) !== "showing" ) {
    eventhover.stop();
}
 
if ( spans.data( "currently" ) !== "showing" ) {
    spans.stop();
}
 
// GOOD!!
var elems = [ eventfade, eventhover, spans ];
 
$.each( elems, function( i, elem ) {
    if ( elem.data( "currently" ) !== "showing" ) {
        elem.stop();
    }
});
```

## Feature & Browser Detection

如果你想要判断用户的浏览器是否支持某些特性，通常有以下两种方法：

- Browser Detection
- Specific Feature Detection

我们推荐您使用**Specific Feature Detection**, 因为即使相同的浏览器也可能因为版本不同而有问题，况且`User Agents`也是不可完全依赖的。Specific Feature Detection Checks if a specific feature is avaliable, instead of developing against a specific browser.Specific Feature Detection 同样有以下两种方法：

- Straight JavaScript
- A Helper Library ([Modernizr](https://modernizr.com/))

## Deferreds

### jQuery Deferreds

```javascript
function getLatestNews() {
    return $.get( "latestNews.php", function( data ) {
        console.log( "news data received" );
        $( ".news" ).html( data );
    });
}
 
function getLatestReactions() {
    return $.get( "latestReactions.php", function( data ) {
        console.log( "reactions data received" );
        $( ".reactions" ).html( data );
    });
}
 
function prepareInterface() {
    return $.Deferred(function( dfd ) {
        var latest = $( ".news, .reactions" );
            latest.slideDown( 500, dfd.resolve );
            latest.addClass( "active" );
    }).promise();
}
 
$.when(
    getLatestNews(),
    getLatestReactions(),
    prepareInterface()
).then(function() {
    console.log( "fire after requests succeed" );
}).fail(function() {
    console.log( "something went wrong!" );
});
```

### Deferred examples