---
title: "Linux commands"
layout: post
date: 2016-02-01
tags:
- linux
categories:
- linux
---

### Android Studio

- `Ctrl + N`: 打开某个类
- `Ctrl + Shift + N`: 打开某个文件
- `连按两下shift`: Search EveryWhere
- `Ctrl + F4`: 关闭某个类
- `Alt + 左/右箭头`: 在已经打开的文件中切换
- `Ctrl + 左/右箭头`: 一个单词一个单词的左右跳

### 解压

- `unzip package.zip -d 目录路径`: 解压package.zip到某个目录
- `tar -cvf foo.tar /something`: 创建一个tar ball，即把something打包
- `tar -tvf foo.tar`: 显示tar里面的所有文件
- `tar --delete -f foo.tar /something/bar`: 从tar里边删除`/something/bar`
- `tar --wildcards --delete -f foo.tar /something/bar*`: 根据正则表达式删除所有匹配的
- `tar -xzvf foo.tar.gz` 解压`tar.gz`结尾的字符

### Sqlite数据库

- `.schema` 显示某个表的结构

### Shell

- kill process on batch （批量杀死进程)

`ps -fA | grep python | cut -c8-11 | xargs kill -9 {}`

- 文件提取某几列

`cut -d' ' -f4-5 temp-debug > temp-debug.cut`

### nginx:[error] open()

nginx: [error] open() “/usr/local/var/run/nginx.pid” failed

解决:

```shell
sudo nginx -c /usr/local/etc/nginx/nginx.conf
sudo nginx -s reload
```

### Linux Terminal 在 tab 中切换快捷键

```bash
Ctrl + [PageUp/PageDown]
```

### Finding the pid listening on a specific port on Mac OS X

```bash
lsof - list open files

lsof -n -i4TCP:8080 | grep LISTEN
```

### Install hexo

```bash
npm install hexo --no-optional

# config git deploy
npm install hexo-deployer-git --save

# how to deploy
hexo clean
hexo generate
hexo deploy

# how to add CNAME file
touch `CNAME` file in `source` folder
```

### Some importan thing about `grep`

```bash
-E, --extended-regexp
             Interpret pattern as an extended regular expression (i.e. force grep to behave as egrep).

-o, --only-matching
             Prints only the matching part of the lines.

-n, --line-number
             Each output line is preceded by its relative line number in the file, starting at line 1.  The line
             number counter is reset for each file processed.  This option is ignored if -c, -L, -l, or -q is spec-
             ified.

-R, -r, --recursive
             Recursively search subdirectories listed.
```

Basic usage:

```bash
grep -Rn 'something' ./
```