---
title: "Interview Question"
layout: post
date: 2015-11-10
tags:
- web
categories:
- website
---

## http状态码

301: Move Permanently
302: Found: 资源已经分配了新的Url
303: See Other
304: Not Modifined
307: Temporary Redirect

## C++类大小

```c++
// 空类一般都是1
class A{
};
// 内存对齐：8
class B{
	char ch;
	int x;
};
// 还是1
class C{
	public:
		void Print(void){}
};
// 虚函数，会生成虚函数表，添加指针，32位：4, 64位：8
class D
{
	public:
		virtual void Print(void){}
};
```
