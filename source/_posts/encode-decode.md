---
title: "Encode & decode"
layout: post
date: 2016-03-18
tags:
- encode decode
categories:
- encode
---

## Encode & Decode

Encode: `编码`

Computers cannot handle text directly; they can only deal with numbers. To represent text (a string of characters) as (a string of) numbers in a computer, we specify a mapping from characters into numbers. This is called an `encoding`.[1](https://www.objc.io/issues/9-strings/unicode/)

Decode: `解码`

## JavaScript

### escape

`escape()`

The deprecated escape() function computes a new string in which certain characters have been replaced by a hexadecimal escape sequence. Use `encodeURI` or `encodeURIComponent` instead.

The escape function is a property of the global object. Special characters are encoded with the exception of: @*_+-./

The hexadecimal form for characters, whose code unit value is 0xFF or less, is a two-digit escape sequence: %xx. For characters with a greater code unit, the four-digit format %uxxxx is used.

```javascript
// output: "%u4E2D%u56FD"
escape('中国');
```

### encodeURI

`encodeURI`

The encodeURI() function encodes a Uniform Resource Identifier (URI) by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

encodeURI replaces all characters except the following with the appropriate UTF-8 escape sequences:

Type	Includes
Reserved characters	; , / ? : @ & = + $
Unescaped characters	alphabetic, decimal digits, - _ . ! ~ * ' ( )
Number sign	#

```javascript
// output: "%E4%B8%AD%E5%9B%BD"
encodeURI("中国")
```

### encodeURIComponent

`encodeURIComponent(str)`

The encodeURIComponent() function encodes a Uniform Resource Identifier (URI) component by replacing each instance of certain characters by one, two, three, or four escape sequences representing the UTF-8 encoding of the character (will only be four escape sequences for characters composed of two "surrogate" characters).

encodeURIComponent escapes all characters except the following: alphabetic, decimal digits, - _ . ! ~ * ' ( )

```javascript
// output: "%E4%B8%AD%E5%9B%BD"
encodeURIComponent("中国")
```

### 解码

[decodeURIComponent](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)
[decodeURI](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)
[unescape](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/unescape)

## Python

### urllib.quote

`urllib.quote(string[, safe])`

Replace special characters in string using the %xx escape. Letters, digits, and the characters '_.-' are never quoted. By default, this function is intended for quoting the path section of the URL. The optional safe parameter specifies additional characters that should not be quoted — its default value is '/'.

[Stackoverflow](http://stackoverflow.com/questions/946170/equivalent-javascript-functions-for-pythons-urllib-quote-and-urllib-unquote)

```python
// output: "%E4%B8%AD%E5%9B%BD"
a = urllib.quote('中国', safe='~@#$&()*!+=:;,.?/\'');
print a
```

```python
// output: "%E4%B8%AD%E5%9B%BD"
a = urllib.quote('中国', safe='~()*!.\'');
print a
```

### urllib.unquote

`urllib.unquote(string)`

Replace %xx escapes by their single-character equivalent.