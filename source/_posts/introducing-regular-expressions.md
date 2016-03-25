---
title: Introducing Regular Expressions
layout: post
date: 2015-11-21
tags:
- regular
categories:
- regular
---

## 简单的模式匹配

在`RegExr`中，去掉`global`选项的话，那么任何正则表达式只匹配目标文本中的第一个匹配项

### 数字
```bash
\d
[0-9]
[01234566789]
```

### 非数字
```bash
\D
[^0-9]
[^\d]
```

### 单词

`\w`:字母、数字和下划线

```bash
\w
[_a-zA-Z0-9]
```

### 非单词

`\W`:空格、标点、其它非字母、非数字字符

```bash
\W
[^_a-zA-Z0-9]
[^\w]
```

### 空白符

`\t`:制表符，`\n`:换行符，`\r`:回车符

```bash
\s
[ \t\n\r]
```

### 非空白字符

```bash
\S
[^ \t\n\r]
[^ \s]
```

### 任意字符

```bash
.
```

`.*` == `[^\n]` 或者 `[^\n\r]`

### 用`sed`在一行开头和末尾添加标签

`-n`: suppress automatic printing of pattern space

`s/^/<h1>`: 在行的开头添加`<h1>`

`;`: 分割命令

`/$/<\/h1>/`: 在行的末尾添加`</h1>`

`p`: 打印受影响的那一行

`q`: 结束程序，只处理第一行

```bash
sed -n 's/^/<h1>/;s/$/<\/h1>/p;q' rime.txt
```

另外一种写法：

`-e`:引导命令

```bash
sed -ne 's/^/<h1>/' -e 's/$/<\/h1>/p' -e 'q' rime.txt
```

还可以全部写在文件里：

使用`sed -f h1.sed rime.txt`运行

```bash
#!/usr/bin/sed

s/^/<h1>/
s/$/<\/h1>
q
```

`Perl`写法：
```bash
#/usr/bin/perl -n

if ($. == 1) {
	s/^/<h1>/;
	s/$/<\/h1>/m;
	print;
}
```

## 边界

### 行的开始和结束
```bash
^ $
```

### 单词边界与非单词边界

`\<\>`是较旧的写法

```bash
\bTHE\b
\Be\B
\<THE\>
```

`grep -Eoc '\<(THE|The|the)\>' rime.txt`:

`-E`: 扩展的正则表达式

`-o`: 只显示一行中与模式匹配的那部分

`-c`: 返回结果的数量

### 使用元字符的字面值

15个在正则表达式中有特殊含义的元字符：

```bash
.^$+?|(){}[]\-
```

## 选择、分组、向后引用

### 捕获分组和向后引用
```bash
\1
$1
```

### 非捕获分组
```bash
(?:the|The|THE)
```

## 字符组

### 并集与差集

```bash
[0-3[6-9]]
[a-z&&[^m-r]]
```

### POSIX字符组

```bash
[[:alnum:]]
[[:^alnum:]]
```

![posix-character-group](/assets/image/regex/posix-character-group.png)

## 量词

### 匹配特定次数
```bash
{n}
{n,}
{m,n}
{0,1} == ?
{1,} == +
{0,} == *
```

### 懒惰量词
```bash
??
+?
*?
{n}?
{n,}?
{m,n}?
```
