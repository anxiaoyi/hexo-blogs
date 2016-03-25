---
title: "Python 2.7"
layout: post
date: 2015-11-05
tags:
- python
categories:
- software
---

## 源代码编码

```python
#!/usr/local/bin/python
# -*- coding: encoding -*-
```

## 对Python世界的简单介绍

### 字符串

```python
#'\n'不会被解释
print r'C:\some\name'

#*用来重复字符串
'un' * 3

#两个或者多个紧挨着的常量字符串,将会自动连接
python = ('Py' 
				'thon')

#字符串slicing, start总是被包含, end总是排除在外
s[:i] + s[i:] = s

#出错:
word[42]

#slicing不会出错
word[4:42]

#字符串不可变,出错
word[0] = 'J'

#unicode
u'中国'

#默认编码:ASCII
str(u'中国')

#将unicode字符串转为8-bit字符串
u'中国'.encode('utf-8')

#其它编码字符串转为Unicode
unicode('\xc3\xa4\xc3\xb6\xc3\xbc', 'utf-8')
```

### Lists

```python
squares = [1, 4, 9]
squares + [36, 49]

#添加元素
squares.append(7 ** 3)

#替换元素
letters[2:5] = ['C', 'D', 'E']

#删除元素
letters[2:5] = []

#清空
letters[:] = []
```

## 更多控制流

### `range()` 函数

```python
#[5,6,7,8,9]
range(5, 10)

#[-10, -40, -70]
range(-10, -100, -30)
```

### 定义函数

```python
# *arguments 参数必须放在 **keywords参数的前面
def cheeseshop(kind, *arguments, **keywords):
for arg in arguments:
print arg
for kw in keywords:
print kw, ":", keywords[kw]
cheeseshop("kind", "It's very runny", "It's really very runny",
				shopkeeper="Michael Palin",
				client="John Cleese",
				sketch="Cheese Shop Sketch")

# 拆包参数,将存放在list或者tuple中的数据拆分为单个单个的数据
range(3, 6)
[3, 4, 5]
args = [3, 6]
range(*args)

# 匿名函数:lambda
def make_increment(n):
	return lambda x: x + n

# 文档
def my_function():
	"""
	Do nothing, but document it.
	No, really, it doesn't do anything
	"""
	print my_function.__doc__

```

### 代码风格

```python
# 类名: CamelCase
# 函数: lower_case_with_underscores
# self 作为第一个方法参数
```

## 数据结构

### Lists

```python
#添加元素:a[len(a):] = [x]
list.append(x)

#将List添加至list末尾
list.extend(L)

#插入
list.insert(i, x)

#删除: 移除第一个出现x的项
list.remove(x)

#删除给定位置的元素
list.pop([i])

#取得元素的位置
list.index(x)

#得到x出现的次数
list.count(x)

#排序
list.sort(cmp=None, key=None, reverse=False)

#反序
list.reverse()

#队列
from collections import deque
queue = deque(["Eric", "John", "Michael"])
queue.append("Terry")
queue.popleft()

#函数编程工具:`filter()`,`map()`,`reduce()`
#`filter(function, sequence)`返回的是一个`function(item) == true`的`sequence`,如果`sequence`是`str`,`unicode`,`tuple`的话,那么返回的也是相同的类型
# `map(function, sequence)`返回的是一个`function(item)`的`sequence`
# `reduce(function, sequence)`:两两元素作`function(item1, item2)`运算

#List Comprehensions
squares = [x**2 for x in range(10)]
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]o

#zip
matrix = [
[1,2,3],
[4,5,6],
]
zip(*matrix)
[(1,2,3), (4,5,6)]
```

### del

```python
删除元素
del a[0]
del a[2:4]
del a[:]
del a
```

### Tuples and Sequences

```python
# tuples 就是跟逗号就对了!
>>> t = 12345, 54321, 'hello!'
>>> t[0]

>>> empty = ()
>>> singleton = 'hello',    # <-- note trailing comma
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```

### Sets

```python
# 使用`set()`来创建一个 set
set(['orange', 'pear', 'apple', 'banana'])
>>> 'orange' in fruit                 # fast membership testing
True
>>> 'crabgrass' in fruit
False

# 并集,差集,合集,或集操作
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # unique letters in a
set(['a', 'r', 'b', 'c', 'd'])
>>> a - b                              # letters in a but not in b
set(['r', 'd', 'b'])
>>> a | b                              # letters in either a or b
set(['a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'])
>>> a & b                              # letters in both a and b
set(['a', 'c'])
>>> a ^ b                              # letters in a or b but not both
set(['r', 'd', 'b', 'm', 'z', 'l'])
```

### Dictionaries

```python
>>> tel = {'jack': 4098, 'sape': 4139}

# 字典键
>>> tel.keys()
['guido', 'irv', 'jack']

# 检查 key 是否存在
>>> 'guido' in tel
True

>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

### 循环

```python

# 循环Lists
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print i, v
...
0 tic
1 tac
2 toe

# 循环字典
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.iteritems():
...     print k, v
...
gallahad the pure
robin the brave

```

## Modules 模块

```python
>>> import fibo
>>> fibo.fib(1000)
1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
# ithin a module, the module’s name (as a string) is available as the value of the global variable __name__.
>>> fibo.__name__
'fibo'

# 当做脚本运行
if __name__ == "__main__":
import sys
fib(int(sys.argv[1]))

# dir() 函数
>>> import fibo
# 找到一个模块定义了哪些name
>>> dir(fibo)
['__name__', 'fib', 'fib2']

# Packages
# The __init__.py files are required to make Python treat the directories as containing packages;

# 像下面这样引用的话,必须使用全称
import sound.effects.echo
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

# 当前路径,父路径
from . import echo
from .. import formats
from ..filters import equalizer

```

## 输入输出

### 理想的formating

```python
# Python has ways to convert any value to a string: pass it to the repr() or str() functions.
>>> s = 'Hello, world.'
>>> str(s)
'Hello, world.'
>>> repr(s)
"'Hello, world.'"

# str.format() 基础用法
>>> print 'We are the {} who say "{}!"'.format('knights', 'Ni')
We are the knights who say "Ni!"

>>> print 'The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',
...                                                    other='Georg')
The story of Bill, Manfred, and Georg.
```

### 操作文件

```python
# mode: 'r':只读, 'w':只写, 'a':添加, 'r+':读写, 默认只读.在windows上, 'b'以二进制打开文件, 'rb', 'wb', 'r+b'
>>> f = open('workfile', 'w')

# 读取指定的大小字节
f.read(size) 

# 读取一行
f.readline()

# 也可以这样读取行
>>> for line in f:
print line,

# 读取所有行到list中
list(f)
f.readlines()

# 写
>>> value = ('the answer', 42)
>>> s = str(value)
>>> f.write(s)

# 当前光标位置,以字节为单位
f.tell()

# 改变光标位置: from_what: 0: 开始, 1:当前位置, 2:文件结尾
f.seek(offset, from_what)

# 关闭文件,释放资源
f.close()

# 使用`with`操作文件是一个好习惯, 比`try-finally`要简洁好多
>>> with open('workfile', 'r') as f:
...     read_data = f.read()
>>> f.closed
True

# 使用`json`保存结构化数据

# Object --> Json String 
>>> json.dumps([1, 'simple', 'list'])
'[1, "simple", "list"]'

# 序列化x object到一个可写的文件
json.dump(x, f)

# 解码
x = json.load(f)
```

## 错误和异常

```python
for arg in sys.argv[1:]:
try:
f = open(arg, 'r')
except IOError:
print 'cannot open', arg
else:
print arg, 'has', len(f.readlines()), 'lines'
f.close()

# 用户自定义的Exception:
class Error(Exception):
"""Base class for exceptions in this module."""
pass

class InputError(Error):
	"""Exception raised for errors in the input.

	Attributes:
	expr -- input expression in which the error occurred
	msg  -- explanation of the error
	"""

	def __init__(self, expr, msg):
		self.expr = expr
		self.msg = msg

```

## 类

### 命名空间

The namespace containing the built-in names is created when the Python interpreter starts up, and is never deleted.
The global namespace for a module is created when the module definition is read in; normally, module namespaces also last until the interpreter quits.

### Class Objects

```python
# 构造函数
>>> class Complex:
...     def __init__(self, realpart, imagpart):
...         self.r = realpart
...         self.i = imagpart
...
>>> x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

### Class and Instance Variables

```python
class Dog:
kind = 'canine'         # class variable shared by all instances

def __init__(self, name):
self.name = name    # instance variable unique to each instance

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind                  # shared by all dogs
'canine'
>>> e.kind                  # shared by all dogs
'canine'
>>> d.name                  # unique to d
'Fido'
>>> e.name                  # unique to e
'Buddy'
```

### Random Remarks

object type: `object.__class__`

### 继承

if a requested attribute is not found in the class, the search proceeds to look in the base class. This rule is applied recursively if the base class itself is derived from some other class.(如果子类没有找到某个属性,那么将会去寻找父类)

```python
class DerivedClassName(modname.BaseClassName):

# 调用父类方法
BaseClassName.methodname(self, arguments).

# built-in function: `isinstance()`: 当`obj.__class__`是`int`的时候,`isinstance(obj, int)` 为`True`

# built-in function: `issubclass`: `issubclass(bool int)`

# 多继承
class DerivedClassName(Base1, Base2, Base3):
```

### 私有属性

以`_`(`_spam`)开头的属性通常应该被认为是`non-public`的

```python
# Name mangling 对于想让子类能够重写方法,但同时又不影响内部调用,非常有用.`_Mapping__update`
class Mapping:
	def __init__(self, iterable):
	self.items_list = []
	self.__update(iterable)

	def update(self, iterable):
	for item in iterable:
		self.items_list.append(item)

	__update = update   # private copy of original update() method

class MappingSubclass(Mapping):

	def update(self, keys, values):
	# provides new signature for update()
	# but does not break __init__()
	for item in zip(keys, values):
		self.items_list.append(item)
```

### 迭代

```python
for element in [1, 2, 3]:
    print element
for element in (1, 2, 3):
	print element
for key in {'one':1, 'two':2}:
    print key
for char in "123":
    print char
for line in open("myfile.txt"):
    print line,

# 像上面的迭代,主要是因为调用了这几个方法`iter()`, `next()`, raisse`StopIteration` exception

>>> s = 'abc'
>>> it = iter(s)
>>> it
<iterator object at 0x00A1DB50>
>>> it.next()
'a'
>>> it.next()
'b'
>>> it.next()
'c'
>>> it.next()
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
      it.next()
StopIteration

# 给你的类添加迭代能力
class Reverse:
    """Iterator for looping over a sequence backwards."""
    def __init__(self, data):
        self.data = data
        self.index = len(data)
    def __iter__(self):
        return self
    def next(self):
        if self.index == 0:
            raise StopIteration
        self.index = self.index - 1
		return self.data[self.index]

>>> rev = Reverse('spam')
>>> iter(rev)
<__main__.Reverse object at 0x00A1DB50>
>>> for char in rev:
...     print char
...
m
a
p
s
```

### Generators

Generators 是一种用来创建迭代的强大的工具,使用`yield`返回数据
```python
def reverse(data):
    for index in range(len(data)-1, -1, -1):
	    yield data[index]

>>> for char in reverse('golf'):
...     print char
...
f
l
o
g

# 能用Generators做的事情,使用class-based的迭代器也照样能够完成,让Generatos如此紧凑的原因是:`__iter__()`,`next()`方法是自动合成的
```

## 标准库的简单旅行

### 操作系统接口

```python
>>> import os

# dir()返回位于一个module中的所有方法
# help()返回一个module中根据docstring创建的manual page

# 文件和目录拷贝,可以使用:
import shutil
```

### 文件通配符

```python
# 使用通配符搜索文件
>>> import glob
>>> glob.glob('*.py')
['primes.py', 'random.py', 'quote.py']
```

### 命令行参数

```python
>>> import sys
>>> print sys.argv
['demo.py', 'one', 'two', 'three']

# `getopt` 模块使用Unix风格的`getopt()`来处理`sys.argv`

# 更加强的处理工具可以用`argparse`来处理
```

### 错误重定向和程序终止
```python
>>> sys.stderr.write('Warning, log file not found starting a new one\n')
Warning, log file not found starting a new one

# 使用`sys.exit()`来终止程序
```

### 字符串模式匹配
```python
>>> import re
>>> re.findall(r'\bf[a-z]*', 'which foot or hand fell fastest')
['foot', 'fell', 'fastest']
>>> re.sub(r'(\b[a-z]+) \1', r'\1', 'cat in the the hat')
'cat in the hat'

>>> 'tea for too'.replace('too', 'two')
'tea for two'
```

### 数学运算

```python
>>> import math

>>> import random
>>> random.choice(['apple', 'pear', 'banana'])
>>> random.sample(xrange(100), 10)   # sampling without replacement
>>> random.randrange(6)    # random integer chosen from range(6)
```

### 网络访问
```python
>>> import urllib2
urllib2.urlopen('http://tycho.usno.navy.mil/cgi-bin/timer.pl')
```

### 日期和时间

```python
>>> # dates are easily constructed and formatted
>>> from datetime import date
>>> now = date.today()
>>> now
datetime.date(2003, 12, 2)
>>> now.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
'12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

>>> # dates support calendar arithmetic 日历算法
>>> birthday = date(1964, 7, 31)
>>> age = now - birthday
>>> age.days
14368
```

### 数据压缩
```python
# 四个模块:zlib, gzip, bz2, zipfile, tarfile
```

### 性能测试
```python
# 四个模块:timeit, profile, pstats
```

### 质量控制
```python
def average(values):
    """Computes the arithmetic mean of a list of numbers.

  	>>> print average([20, 30, 70])
	40.0
	"""
	return sum(values, 0.0) / len(values)

import doctest
doctest.testmod()   # automatically validate the embedded tests

import unittest

class TestStatisticalFunctions(unittest.TestCase):

    def test_average(self):
        self.assertEqual(average([20, 30, 70]), 40.0)
        self.assertEqual(round(average([1, 5, 7]), 1), 4.3)
        with self.assertRaises(ZeroDivisionError):
            average([])
        with self.assertRaises(TypeError):
            average(20, 30, 70)

unittest.main() # Calling from the command line invokes all tests
```

## 标准库之旅（二）

### 输出格式化

```python
import repr
import pprint
import textwrap
import locale
```

### Templating

```python
>>> from string import Template
>>> t = Template('${village}folk send $$10 to $cause.')
>>> t.substitute(village='Nottingham', cause='the ditch fund')
```

### Working with Binary Data Record Layouts

### 多线程

```python
import threading, zipfile

class AsyncZip(threading.Thread):
	def __init__(self, infile, outfile):
		threading.Thread.__init__(self)
		self.infile = infile
		self.outfile = outfile
		def run(self):
	f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
	f.write(self.infile)
	f.close()
	print 'Finished background zip of: ', self.infile

background = AsyncZip('mydata.txt', 'myarchive.zip')
background.start()
print 'The main program continues to run in foreground.'

background.join()    # Wait for the background task to finish
print 'Main program waited until background was done.'
```

Applications using `Queue.Queue` objects for inter-thread communication and coordination are easier to design, more readable, and more reliable.

### Logging

```python
import logging
logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')
```

### Weak References

```python
import logging
logging.debug('Debugging information')
logging.info('Informational message')
logging.warning('Warning:config file %s not found', 'server.conf')
logging.error('Error occurred')
logging.critical('Critical error -- shutting down')
```

### Weak References

Python 的自动内存管理：

引用计数和垃圾收集消除环引用，`weakref`跟踪某个对象不创建一个reference

```python
>>> import weakref, gc
>>> class A:
...     def __init__(self, value):
...         self.value = value
...     def __repr__(self):
...         return str(self.value)
...
>>> a = A(10)                   # create a reference
>>> d = weakref.WeakValueDictionary()
>>> d['primary'] = a            # does not create a reference
>>> d['primary']                # fetch the object if it is still alive
10
>>> del a                       # remove the one reference
>>> gc.collect()                # run garbage collection right away
0
>>> d['primary']                # entry was automatically removed
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
      d['primary']                # entry was automatically removed
  File "C:/python26/lib/weakref.py", line 46, in __getitem__
	  o = self.data[key]()
KeyError: 'primary'
```

### Lists的一些工具

```python
from array import array
from collections import deque
import bisect
from heapq import heapify, heappop, heappush
```

### Decimal Floating Point 运算

```python
>>> from decimal import *
>>> x = Decimal('0.70') * Decimal('1.05')
>>> x
Decimal('0.7350')
>>> x.quantize(Decimal('0.01'))  # round to nearest cent
Decimal('0.74')
>>> round(.70 * 1.05, 2)         # same calculation with floats
0.73

>>> Decimal('1.00') % Decimal('.10')
Decimal('0.00')
>>> 1.00 % 0.10
0.09999999999999995

>>> sum([Decimal('0.1')]*10) == Decimal('1.0')
True
>>> sum([0.1]*10) == 1.0
False
```

# 下面是[标准类库](https://docs.python.org/2/library/index.html)

**Keep this under your pillow**

下面这些将会简要涉及，需要点链接查看更多更全的内容。

## 内置类型

### Truth Value 测试

`None`,`False`,`0, 0L, 0.0, 0j`, `'', (), [], {}`: 全部被认为 false

### Boolean Operations

```python
x or y
x and y
not x
```

### 比较
```python
<
<=
>
>=
== # equal
!= # not equal
is # object identity, 对象对比
is not # negated object identity 
```

### 算数类型
```python
int, float, long, complex
```

### 整数按位操作
```python
| ^ & << >> ~
```

### Sequence Types
```python
str, unicode, list, tuple, bytearray, buffer, xrange

# 所有Sequence Types 都具有如下操作：
x in s, x not in s
s + t
s * n, n * s
s[i],s[i:j],s[i:j:k]
len(s), min(s), max(s)
s.index(x)
s.count(x)
```
