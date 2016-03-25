---
title: "understanding javascript this"
layout: post
date: 2015-12-01
tags:
- javascript
- this
categories:
- javascript
---

## [how-does-the-this-keyword-work](http://stackoverflow.com/questions/3127429/how-does-the-this-keyword-work)

The interpreter updates the ThisBinding whenever establishing an execution context in one of only three different cases:

- Initial global execution context
- Entering eval code
- Entering function code

