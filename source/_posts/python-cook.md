---
title: "python-cook"
layout: post
date: 2015-08-15
tags:
- python
categories:
- software
---

# Python-Cook

## 类和对象

### 改变instance的字符表示

`__repr__()`表示对象代码级别的表示,`__str__()`表示`str()`和`print()`函数的输出

```python
p = Pair(3, 4)
print('p is {0!r}'.format(p)) # p is Pair(3,4), __repr__
print('p is {0}'.format(p)) # p is (3,4), __str__
```

```python
def __repr__(self):
    return 'Pair({0.x!r, 0.y!r})'.format(self)

def __repr__(self):
    return 'Pair(%r, %r)' % (self.x, self.y)
```

Example:
```python
class Pair:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return 'Pair({0.x!r, 0.y!r})'.format(self)

    def __str__(self):
        return '({0.x!s, 0.y!s})'.format(self)
```

### 自定义字符format

就像日期这样:
```python
from datetime import date
d = Date(2012, 12, 24)
format(d) # '2012-12-21'
format(d, '%A, %B %d, %Y') # 'Friday, December 21, 2012'
'The end is {:%d %b %Y}. Goodbye'.format(d)
```

答案就是`__format__`函数
```python
_formats = {
    'ymd': '{d.year}-{d.month}-{d.day}',
    'mdy': '{d.month}/{d.day}/{d.year}',
    'dmy': '{d.day}/{d.month}/{d.year}'
}

class Date:
    def __init__(self, year, month, day):
        self.year = year
        self.month = month
        self.day = day

    def __format__(self, code):
        if code == '':
            code == 'ymd'
        fmt = _format[code]
        return fmt.format(d=self)
```

使用:
```python
d = Date(2012, 12, 31)
format(d)
'The date is {:ymd}'.format(d)
```

### 使对象支持Context-Managment协议

已经有人翻译了:书中所有代码基于python 3.3,项目地址:
[python](http://python3-cookbook.readthedocs.org/zh_CN/latest/preface.html#id3)
