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

### Decimal to hex

```bash
echo 'obase=16; 122' | bc
```

### Curl

```bash
# -I, --head (HTTP/FTP/FILE)  Fetch  the  HTTP-header  only!
curl -I baidu.com
```

```bash
curl http://www.baidu.com > baidu.html
```

```bash
curl -o baidu.html http://www.baidu.com
```

```bash
# -v, --verbose Be  more  verbose/talkative  during  the  operation. Useful for debugging and seeing what's going on "under the hood".  A line starting with '>' means "header data" sent by curl, '<' means "header data" received by curl that is hidden in  normal  cases, and a line starting with '*' means additional info provided by curl.
curl -v http://www.baidu.com
```

```bash
# Enables  a full trace dump of all incoming and outgoing data, including descriptive information, to the given output file. Use "-" as filename to have the output sent to stdout.
curl --trace <file> http://www.baidu.com
```

### tcpdump

#### Capture Packets from Specific Interface

```bash
# ent0 is a "network adapter" which represents a physical device
# while en0 is a logical device (based on ent0) representing a standard "network Interface"
sudo tcpdump -i ent0(en0)
```

#### Display Available Interfaces

```bash
sudo tcpdump -D
```

#### Display Captured Packets in HEX and ASCII

```bash
tcpdump -XX -i en0
```

[More Sample](http://www.tecmint.com/12-tcpdump-commands-a-network-sniffer-tool/)

### host

The host command performs DNS lookups. Give it a domain name and you’ll see the associated IP address. Give it an IP address and you’ll see the associated domain name.

```bash
host baidu.com
host 220.181.57.217
```

### ls

```bash
# ls -l
-rw-r--r--   1 zk  staff  3045 Jul  8 11:19 README.md    
```

```bash
If the -l option is given, the following information is displayed for each file: file mode,
     number of links, owner name, group name, number of bytes in the file, abbreviated (简短的，仅可蔽体的，小型的) month,
     day-of-month file was last modified, hour file last modified, minute file last modified,
     and the pathname.

     The file mode printed under the -l option consists of the entry type, owner permissions,
     and group permissions.  The entry type character describes the type of file, as follows:

           b     Block special file.
           c     Character special file.
           d     Directory.
           l     Symbolic link.
           s     Socket link.
           p     FIFO.
           -     Regular file.

     The next three fields are three characters each: owner permissions, group permissions, and
     other permissions.  Each field has three character positions:

           1.   If r, the file is readable; if -, it is not readable.

           2.   If w, the file is writable; if -, it is not writable.

           3.   The first of the following that applies:

                      S     If in the owner permissions, the file is not executable and set-
                            user-ID mode is set.  If in the group permissions, the file is not
                            executable and set-group-ID mode is set.

                      s     If in the owner permissions, the file is executable and set-user-ID
                            mode is set.  If in the group permissions, the file is executable
                            and setgroup-ID mode is set.

                      x     The file is executable or the directory is searchable.

                      -     The file is neither readable, writable, executable, nor set-user-ID
                            nor set-group-ID mode, nor sticky.  (See below.)

                These next two apply only to the third character in the last group (other per-
                missions).

                      T     The sticky bit is set (mode 1000), but not execute or search permis-
                            sion.  (See chmod(1) or sticky(8).)

                      t     The sticky bit is set (mode 1000), and is searchable or executable.
                            (See chmod(1) or sticky(8).)
```