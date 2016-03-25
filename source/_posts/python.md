---
title: "The Python Tutorial"
layout: post
date: 2015-08-22
tags:
- python
categories:
- software
---

## The Python Tutorial

##  don’t want characters prefaced by `\` to be interpreted as special characters
```python
>>> print r'C:\some\name'  # note the r before the quote
C:\some\name
```

## In addition to indexing, slicing is also supported.
```python
>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
'Py'
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
'tho'
```

### Attempting to use a index that is too large will result in an error:
```python
>>> word[42]  # the word only has 6 characters
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

### However, out of range slice indexes are handled gracefully when used for slicing:
```python
>>> word[4:42]
'on'
>>> word[42:]
''
```

### Python strings cannot be changed — they are immutable.

## Unicode string
```python
>>> u'Hello\u0020World !'
u'Hello World !'
```

### The default encoding is normally set to ASCII
```python
>>> u"abc"
u'abc'
>>> str(u"abc")
'abc'
```

### To convert a Unicode string into an 8-bit string using a specific encoding
```python
>>> u"äöü".encode('utf-8')
'\xc3\xa4\xc3\xb6\xc3\xbc'
```

##  lists are a mutable type
```python
>>> cubes = [1, 8, 27, 65, 125]  # something's wrong here
>>> 4 ** 3  # the cube of 4 is 64, not 65!
64
>>> cubes[3] = 64  # replace the wrong value
>>> cubes
[1, 8, 27, 64, 125]
```

### You can also add new items at the end of the list, by using the `append()` method

### replace, remove, clear
```python
>>> # replace some values
>>> letters[2:5] = ['C', 'D', 'E']
>>> # now remove them
>>> letters[2:5] = []
>>> # clear the list by replacing all the elements with an empty list
>>> letters[:] = []
```

## More Control Flow Tools

### An if ... elif ... elif ... sequence is a substitute for the `switch` or `case` statements found in other languages.
```python
>>> x = int(raw_input("Please enter an integer: "))
Please enter an integer: 42
>>> if x < 0:
...     x = 0
...     print 'Negative changed to zero'
... elif x == 0:
...     print 'Zero'
... elif x == 1:
...     print 'Single'
... else:
...     print 'More'
...
More
```

### If you need to modify the sequence you are iterating over while inside the loop (for example to duplicate selected items), it is recommended that you first make a copy.
```python
>>> for w in words[:]:  # Loop over a slice copy of the entire list.
...     if len(w) > 6:
...         words.insert(0, w)
```

### To iterate over the indices of a sequence, you can combine `range()` and `len()` as follows:
```python
>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
...     print i, a[i]
```

### A function definition introduces the function name in the current symbol table. The value of the function name has a type that is recognized by the interpreter as a user-defined function. This value can be assigned to another name which can then also be used as a function. This serves as a general renaming mechanism:
```python
>>> fib
<function fib at 10042ed0>
>>> f = fib
>>> f(100)
0 1 1 2 3 5 8 13 21 34 55 89
```

### Default Argument Values
```python
def ask_ok(prompt, retries=4, complaint='Yes or no, please!'):
    while True:
        ok = raw_input(prompt)
        if ok in ('y', 'ye', 'yes'):
            return True
        if ok in ('n', 'no', 'nop', 'nope'):
            return False
        retries = retries - 1
        if retries < 0:
            raise IOError('refusenik user')
        print complaint
```

When a final formal parameter of the form `**name` is present, it receives a dictionary (see Mapping Types — dict) containing all keyword arguments except for those corresponding to a formal parameter. This may be combined with a formal parameter of the form `*name` (described in the next subsection) which receives a tuple containing the positional arguments beyond the formal parameter list. (`*name` must occur before `**name`.) For example, if we define a function like this:
```python
def cheeseshop(kind, *arguments, **keywords):
    print "-- Do you have any", kind, "?"
    print "-- I'm sorry, we're all out of", kind
    for arg in arguments:
        print arg
    print "-" * 40
    keys = sorted(keywords.keys())
    for kw in keys:
        print kw, ":", keywords[kw]
```

It could be called like this:

```python
cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper='Michael Palin',
           client="John Cleese",
           sketch="Cheese Shop Sketch")
```

### Unpacking Argument Lists

The reverse situation occurs when the arguments are already in a list or tuple but need to be unpacked for a function call requiring separate positional arguments. For instance, the built-in `range()` function expects separate start and stop arguments. If they are not available separately, write the function call with the *-operator to unpack the arguments out of a list or tuple:

```python
>>> range(3, 6)             # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> range(*args)            # call with arguments unpacked from a list
[3, 4, 5]
```

In the same fashion, dictionaries can deliver keyword arguments with the **-operator:
```python
>>> def parrot(voltage, state='a stiff', action='voom'):
...     print "-- This parrot wouldn't", action,
...     print "if you put", voltage, "volts through it.",
...     print "E's", state, "!"
...
>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> parrot(**d)
-- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !
```

### Small anonymous functions can be created with the `lambda` keyword.
```python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
```

### Coding Style

Name your classes and functions consistently; the convention is to use `CamelCase` for classes and `lower_case_with_underscores` for functions and methods. Always use `self` as the name for the first method argument

## Data Structures

You might have noticed that methods like `insert`, `remove` or `sort` that only modify the list have no return value printed – they return the default None. **This is a design principle for all mutable data structures in Python.**

### More on Lists

- `list.append(x)`
Add an item to the end of the list; equivalent to a[len(a):] = [x].

- `list.extend(L) `
Extend the list by appending all the items in the given list; equivalent to a[len(a):] = L.

- `list.insert(i, x)`
Insert an item at a given position. The first argument is the index of the element before which to insert, so a.insert(0, x) inserts at the front of the list, and a.insert(len(a), x) is equivalent to a.append(x).

- `list.remove(x)`
Remove the first item from the list whose value is x. It is an error if there is no such item.

- `list.pop([i])`
Remove the item at the given position in the list, and return it. If no index is specified, a.pop() removes and returns the last item in the list. (The square brackets around the i in the method signature denote that the parameter is optional, not that you should type square brackets at that position. You will see this notation frequently in the Python Library Reference.)

- `list.index(x)`
Return the index in the list of the first item whose value is x. It is an error if there is no such item.

- `list.count(x)`
Return the number of times x appears in the list.

- `list.sort(cmp=None, key=None, reverse=False)`
Sort the items of the list in place (the arguments can be used for sort customization, see sorted() for their explanation).

- `list.reverse()`
Reverse the elements of the list, in place.

#### Using Lists as Queues

To implement a queue, use `collections.deque` which was designed to have fast appends and pops from both ends. For example:

```python
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

### Functional Programming Tools

 - `filter(function, sequence)` returns a sequence consisting of those items from the sequence for which function(item) is true.
- `map(function, sequence)` calls function(item) for each of the sequence’s items and returns a list of the return values.
- `reduce(function, sequence)` returns a single value constructed by calling the binary function function on the first two items of the sequence, then on the result and the next item, and so on. 

```python
>>> def f(x): return x % 3 == 0 or x % 5 == 0
...
>>> filter(f, range(2, 25))
[3, 5, 6, 9, 10, 12, 15, 18, 20, 21, 24]

>>> def cube(x): return x*x*x
...
>>> map(cube, range(1, 11))
[1, 8, 27, 64, 125, 216, 343, 512, 729, 1000]

>>> def add(x,y): return x+y
...
>>> reduce(add, range(1, 11))
55
```

### List Comprehensions

List comprehensions provide a concise way to create lists.
```python
squares = [x**2 for x in range(10)]
>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
```

### The `del` statement

There is a way to remove an item from a list given its index instead of its value: the del statement. 
```python
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
>>> del a
```

###  Tuples and Sequences

```python
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
```

The statement `t = 12345, 54321, 'hello!'` is an example of tuple packing: the values 12345, 54321 and 'hello!' are packed together in a tuple. The reverse operation is also possible:
```python
>>> x, y, z = t
```

### Sets

Curly braces or the `set()` function can be used to create sets.

```python
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> fruit = set(basket)               # create a set without duplicates
>>> fruit
set(['orange', 'pear', 'apple', 'banana'])
>>> 'orange' in fruit                 # fast membership testing
True
>>> 'crabgrass' in fruit
False

>>> # Demonstrate set operations on unique letters from two words
...
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

#### set comprehensions
```python
>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
set(['r', 'd'])
```

### Dictionaries

dictionaries are indexed by keys, which can be any immutable type; strings and numbers can always be keys. Tuples can be used as keys if they contain only strings, numbers, or tuples;

```python
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'guido': 4127, 'irv': 4127, 'jack': 4098}
>>> tel.keys()
['guido', 'irv', 'jack']
>>> 'guido' in tel
True
```

The `dict()` constructor builds dictionaries directly from sequences of key-value pairs:
```python
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

### Looping Techniques

When looping through a sequence, the position index and corresponding value can be retrieved at the same time using the `enumerate()` function.

```python
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print i, v
...
0 tic
1 tac
2 toe
```

To loop over two or more sequences at the same time, the entries can be paired with the `zip()` function.
```python
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print 'What is your {0}?  It is {1}.'.format(q, a)
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

When looping through dictionaries, the key and corresponding value can be retrieved at the same time using the `iteritems()` method.
```python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.iteritems():
...     print k, v
...
gallahad the pure
robin the brave
```

## Modules

A module is a file containing Python definitions and statements. The file name is the module name with the suffix .py appended. Within a module, the module’s name (as a string) is available as the value of the global variable `__name__`. For instance, use your favorite text editor to create a file called fibo.py in the current directory with the following contents:

```python
>>> import fibo
>>> fibo.__name__
```

There is a variant of the `import` statement that imports names from a module directly into the importing module’s symbol table. 

```python
>>> from fibo import fib, fib2
# This imports all names except those beginning with an underscore (`_`).
>>> from fibo import *
```

### Executing modules as scripts

```python
python fibo.py <arguments>

if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
```

### Standard Modules

The built-in function `dir()` is used to find out which names a module defines. It returns a sorted list of strings:

```python
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
```

### Packages

The `__init__.py` files are required to make Python treat the directories as containing packages; 

```python
import sound.effects.echo
# This loads the submodule sound.effects.echo. It must be referenced with its full name.
sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

from sound.effects import echo
# This also loads the submodule echo, and makes it available without its package prefix, so it can be used as follows:
echo.echofilter(input, output, delay=0.7, atten=4)

from sound.effects.echo import echofilter
Again, this loads the submodule echo, but this makes its function echofilter() directly available:
echofilter(input, output, delay=0.7, atten=4)
```

Note that when using from package import item, the item can be either a submodule (or subpackage) of the package, or some other name defined in the package, like a function, class or variable.

```python
from . import echo
from .. import formats
from ..filters import equalizer
```

## Input and Outout

One question remains, of course: how do you convert values to strings? Luckily, Python has ways to convert any value to a string: pass it to the `repr()` or `str()` functions.
The `str()` function is meant to return representations of values which are fairly human-readable, while `repr()` is meant to generate representations which can be read by the interpreter (or will force a SyntaxError if there is no equivalent syntax).

```python
>>> print '{0} and {1}'.format('spam', 'eggs')
spam and eggs
>>> print '{1} and {0}'.format('spam', 'eggs')
eggs and spam
>>> print 'This {food} is {adjective}.'.format(
...       food='spam', adjective='absolutely horrible')
This spam is absolutely horrible.
```

'!s' (apply `str()`) and '!r' (apply `repr()`) can be used to convert the value before it is formatted.

### Old string formatting
The `%` operator can also be used for string formatting. It interprets the left argument much like a sprintf()-style format string to be applied to the right argument, and returns the string resulting from this formatting operation. For example:
```python
>>> import math
>>> print 'The value of PI is approximately %5.3f.' % math.pi
```

### Saving structured data with `json`

If you have an object x, you can view its JSON string representation with a simple line of code:
```python
>>> json.dumps([1, 'simple', 'list'])
'[1, "simple", "list"]'
```

## Errors and Exceptions

```python
import sys

try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except IOError as e:
    print "I/O error({0}): {1}".format(e.errno, e.strerror)
except ValueError:
    print "Could not convert data to an integer."
except:
    print "Unexpected error:", sys.exc_info()[0]
    raise
```

User-defined Exceptions

```python
>>> class MyError(Exception):
...     def __init__(self, value):
...         self.value = value
...     def __str__(self):
...         return repr(self.value)
```

## Classes

```python
class Dog:

    kind = 'canine'         # class variable shared by all instances

    def __init__(self, name):
        self.name = name    # instance variable unique to each instance
```

Python has two built-in functions that work with inheritance:

Use `isinstance()` to check an instance’s type: `isinstance(obj, int)` will be True only if obj.__class__ is int or some class derived from int.
Use `issubclass()` to check class inheritance: `issubclass(bool, int)` is True since bool is a subclass of int. However, `issubclass(unicode, str)` is False since unicode is not a subclass of str (they only share a common ancestor, basestring).

```python
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

### Iterators

```python
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
```

`Generators` are a simple and powerful tool for creating iterators. 

```python
def reverse(data):
    for index in range(len(data)-1, -1, -1):
        yield data[index]
>>> for char in reverse('golf'):
...     print char
```