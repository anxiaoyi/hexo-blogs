---
title: "Node"
layout: post
date: 2015-11-07
tags:
- nodejs
categories:
- software
---

# 深入浅出Node.js

## Introduction to Node

### Node 特点
```javascript
//1. 异步IO
var fs = require('fs');
fs.readFile('/path', function(err, file) {
	console.log('读取完成');
});

//2. 事件和回调函数

//3. 单线程: 没有死锁,也没有线程上下文切换带来的性能上的开销. 弱点:无法利用多核CPU,错误会引起整个应用退出,大量计算将会占用CPU,导致无法继续调用异步	I/O

//4. 跨平台
借助`libuv`实现跨平台
```

### Node 的应用场景

- I/O密集型:不是启动一个线程,而是利用事件循环的处理能力.
- 是否不擅长CPU密集型业务
- 与遗留系统和平共处
- 分布式应用

## 模块

### 简单模块定义
```javascript
//circle.js
var PI = Math.PI;
exports.area = function(r) {
	return PI * r * r;
}
exports.circumference = function(r) {
	return 2 * PI * r;
};

//app.js
var circle = require('./circle.js');
console.log('The area of a circle of radius 4 is ' + circle.area(4));
```

## Node.js的事件机制

### 事件机制
```javascript
//程序员只需要关注error, data这些业务事件点即可,无需关注内部流程
var req = http.request(options, function(res) {
	res.on('data', function(chunk) {
		console.log('body:' + chunk);
	});
});
req.on('error', function(e) {
	console.log('problem:' + e.message);
});
```

### 利用事件队列解决雪崩问题

雪崩问题:缓存失效,大并发高访问量涌入数据库,数据库无法承受,导致网站前段相应缓慢

```javascript
// 1.改进:使用状态锁
var status = 'ready';
var select = function (callback) {
	if (status === 'ready') {
		status = 'pending';
		db.select('SQL', function(results) {
			callback(results);
			status = 'ready';
		});
	}
};

// 2.上述连续多个调用,只有第一个是有效的,所以要引入事件队列:
var proxy = new EventProxy();
var status = 'ready';
var select = function (callback) {
	proxy.once('selected', callback); //请求回调压入队列,执行一次就会移除
	if (status === 'ready') {
		status = 'pending';
		db.select('SQL', function(results) {
			proxy.emit('selected', results);
			status = 'ready';
		});
	}
};
```

## 异步I/O

实质:用户空间中的程序不用依赖内核空间中的I/O操作完成,即可进行后续任务.

```javascript
// getFileFromNet的调用不依赖getFile调用的结束
// 同步:m+n, 异步:max(m,n)
getFile('file_path');
getFileFromNet('url');
```

### 异步I/O必要性

为了使I/O可以并行,充分利用CPU,通常语言这样设计:

- 多线程单进程:

共享存储空间,实现并行处理任务,从而充分利用CPU.缺点:线程执行时上下文交换开销大,状态同步,编写调用复杂化.

- 单线程多进程:

调用简单.缺点:业务逻辑复杂的时(涉及多个I/O调用),因为业务逻辑不能分布到多个进程空间,事物处理要远远大于多线程模式

在当前大型Web应用中,单机的情形是十分稀少的,一个事务往往要跨越网络几次才能完成最终处理.所以,m,n会相应增大,同步I/O暴露出脆弱的状态和性能问题.

m + n + ... 和 max(m, n, ...)

故受众多云计算厂商青睐

### 操作系统对异步I/O的支持

阻塞/非阻塞 和 异步/同步 实际上是两回事

- I/O的阻塞和非阻塞
阻塞模式会造成应用程序等待,直到I/O完成. 操作系统也支持设置为非阻塞,可能在没有拿到真正数据的时候就已经返回了,为此你需要多次调用才能确认I/O完全完成.

- I/O的同步与异步
作阻塞I/O调用,那么就是同步.相反,非阻塞,就是异步

### 异步I/O与轮询

非阻塞需要多次轮询才能确保数据读取完成,可能会造成占用多CPU时间片,性能较为低下,现存的轮询:

- read
性能最低

- select
对文件描述符上的事件状态进行判断

- poll
多路复用

- epoll
多路复用

- pselect
- kqueue

因为应用程序仍然需要主动去判断I/O的状态,所以轮询也是一种同步

### 理想的异步I/O模型

不需要轮询,通过信号或者回调将数据传递回来即可,Linux存在这种异步非阻塞I/O方式:AIO,但是存在缺陷

Linux下没有完美的异步I/O支持,libeio实质是采用线程池和阻塞I/O模拟出来的异步I/O,Node.js采用libuv来与下层的libeio/libev及IOCP之间独立

window下采用IOCP

## Buffer

```javascript
var fs = require('fs');
var rs = fs.createReadStream('testdata.md');
var data = '';
rs.on('data', function(trunk) {
	data += trunk;
});
rs.on('end', function() {
	console.log(data);
});

//这样改进之后,我们会得到中文乱码的输出,因为data += trunk的问题
//data = data.toString() + trunk.toString()
var rs = fs.createReadStream('testdata.md', {
	bufferSize: 11
});

//解决
var rs = fs.createReadStream('testdata.md', {
	encoding: 'utf-8', 
	bufferSize: 11
});
```

## Connect 框架

Node 的一个中间件框架

### 中间件

```javascript
var http = require('http');
http.createServer(function(req, res) {
	res.writeHead(200, {
		'Content-Type', 'text/plain'
	});
	res.end('Hello World\n');
}).listen(1337, '127.0.0.1');
```
