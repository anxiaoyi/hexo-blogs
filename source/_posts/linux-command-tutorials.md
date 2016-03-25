---
title: "Linux Tutorials"
layout: post
date: 2015-08-09
tags:
- linux
categories:
- linux
---

### show env variable
```
env
env | less
env | sort | less
```

### show shell variable
```
set
```

### show variable value
```
echo ${TERM}
echo "The terminal type is <$TERM>."
```

### export variable to env
```
export NAME[=value]...
```

### delete variable (unset)
```
unset NAME...
```

### shell option
```
set -o [option] #open
set +o [option] #off
```

check is a command built-in command, use `man shell` to check all built-in command, `apropos builtin`

```
type command...
help [built-in-command]
```

### set path
```
export PATH="$PATH:$HOME/bin"
```

### command replace
```
echo "The time and date are `date`".
```

### re-edit command, `^W` to delete the last word, `^X` or `^U` to delete the whole line, `stty` to show the key map

### command history
```
fc -l #fc means fix command
fc -s 24 #re-excute the 24 num command
fc -s #re-excute the last command
fc -s pattern=replacement number #`fc -s q=e`
```

### command history max number
```
echo $HISTSIZE
```

### alias
```
alias [name=commands]
unalias name
unalias -a #remove all
```

### do not use alias temporialy
```
\commands
```

```
create login file(system startup) and environment file(shell startup)
in bash: 
login file: .bash_profile .bash_login
env file: .bashrc #rc for run commands
logout file: .bash_logout 
```

### Unix design: Small is beautiful. Less is more

### redirect standanrd output
```
sort > file
sort >> file
```

### redirect standard input
```
sort < file
sort < file > outputfile
```

### redirect
```
calc 8> results
command 0< inputfile
command 1> outputfile
command 2> errorfile
sort > output 2>&1 #both output and error-output to the same-file
update 2> /dev/null
```

### sub-shell
```
(date)
```

### pipe turn stream
```
cat name1 name2 name3 | tee d1 d2 | grep Harley
cat name1 name2 name3 | tee -a backup | grep Harley # append
who | tee status
command | tee file # tee will send it's output to the standard output : screen
who | tee status | less

### combination commands
update || echo "The update program failed"
grep Harley people > /dev/null && sort people > contacts
```

### What's filter? 

it read standard input and write to the standard output, one line each time
### if you want to create your own filter, please first check `man` `whatis` `apropos` `info`

```
cat > data
cat >> data
cat < data # == cat data0
cat a b c > info 
cat a b c | sort
cat -n # add line number
cat -bn # use with -n, tell cat do not line number to the white line
cat -s #squeeze, squuze mutiple white lines to single line
```

### split
split multiple files to single ones

`split data` every file default is 1000 lines

`split -l 5000 data` every file is 5000 lines

default file name is : xaa, xab, xac, ..., the following commands will generate as x01 x02 x03 ...
`split -d -l 5000 data`

replace x with zk: zk01 zk02 zk03 ...
`split -d -l 5000 data zk`

more files: x000 x001 x002
`split -d -a 3 data`

### tac
tac will reverse all lines

`tac log > reverse-log`
`tac log | less`

reverse log1, reverse log2, reverse log3, and then combile them
`tac log1 log2 log3 | less`

### rev
reverse character order

`rev data`
input:<br>
12345<br>
abcde<br>

output:<br>
54321<br>
edcba<br>

`rev data | tac`

### head, tail
default will show 10 lines

`calculate | head`
`calculate | tail`

show 15 lines from head:
`calculate | head -n 15`

### colrm (column remove)
delete columns, the first character is 1, the second is 2...

remove [14,30], write the left to the standard output
`colrm 14 30 < students`
`colrm 14 < students > grades`

### cmp
compare arbitrary two files, compare by bytes, if same, you will see nothing
`cmp calculate-1.0 calculate-backup`

### comm
compare ordered text files, explain the output: the first column means the first file only has, the second column means the second file only has, the third column means both the two files have.
`comm frick frack`

use `-1 -2 -3` to supress the column output
`comm -13 frick frack`

## diff
compare un-ordered text files. the output will be a group of instruction to help you to know how to let the first file to become the second file.

`diff old-names new-names`

3c3<br>
< Paig Turner<br>
---<br>
> Paige Turner<br>

`c`: for change
`d`: for delete
`a`: for append

`<`: for the first files
`>`: for the second files

`3c3`: for change the first file's the third line to the second file's the third line

`1a2`
`4d3`: normally, you should ignore the number after the character d
`1,2d0`

ignore case:
`diff -i a b`

ignore white space, it means `XX` will the same as `X     X`
`diff -w a b`

ignore white space in numbers, but at least one, `x x` will the same as `x   x`:
`diff -b a b`

ignore blank lines:
`diff -B a b`

ignore all the different details:
`diff -q a b`

tell me explicitly if the two files same:
`diff -s a b`

show the context in the different lines:
`diff -c a b`

unified output:
`diff -u a b`

output side by side
`diff -y a b`

`diff -y` == `sdiff`
`sdiff a b`

we can use diff to re-create all files in version, or called patch to minimize the download size to upgrade to the latest version in the program.
`diff foo-2.0.c foo-2.1.c > foo-diff-2.1`

## cut
oppisite to the command `colrm`

cut the [14,30] columns
`cut -c 14-30 info`

`cut -c 14-30,42-49 info`

`who | cut -c 1-8`

cut the files
`cut -f 1 -d ':' /etc/passwd`
`cut -f 1,3-5 -d ':' /etc/passwd`

## paste
combine the data columns, default separator is '\t'

`paste idnumber name birthday phone > info`

change default separator to ' '
`paste -d ' ' a b c d`

use separator givend in-turn
`paset -d '|%' a b c d`

## nl 
create line number

`nl books > best-books`

从100行开始编号:
`nl -v 100 data`

增量为5:
`nl -i 5 data`

默认不对空白行编号,强制对所有行编号
`nl -b a file`

## wc
word count

输出为`2 13 71 poem`,表示:2行,13个单词,71个字符
`wc poem`
`wc poem message story`

1个制表符可能表示1-8个空格,取决于它在行中的位置:1, 9,17, 25...

vi 查看不可见字符:
`:set list`
`:set nolist`

## expand
将制表符转换为空格

`expand animals > animals-expanded`

制表符每隔4个字符一个:1,5,9,13...
`expand -t 4 data > data-new`
`expand -t 7,15,21,56 data > data-new`

## 将空格转换为制表符
默认情况下,只替换开头的空格,用来缩进
`unexpand -t 4 rough-notes > mickey`

## fold
格式化行:长行分割为短行

80字符行

## 格式化段落
fmt

## pr
按照页/列来格式化文本,使其适合打印

## grep
抽取包含特定模式的行

syntax: pattern [option] pattern [file]

`grep anxiaoyi /etc/passwd`
`grep ': ' info`

g/re/p: global/regular expression/print

搜索包含pm的所有行
`w -h | grep pm`

统计/etc目录下所有子目录的数量, -c: 统计
`ls -F /etc | grep -c "/"`

忽略大小写字母
`grep -i pizza food-costs`

输出行号:
`grep -in pizza food-costs`

-l:列出文件名,而不是行
`grep -l Harley a-file b-file c-file`

-L:列出不包含匹配模式的文件

-w:只希望搜索完整的单词
`grep -w now memo`

-v:选取不包含指定模式的所有行
`grep -v DONE homework`

-x:完全由搜索模式构成的行
`grep -x Harley names`

-r:搜索整个目录树
-s:抑制出错信息
`grep -rs / 'shutdown now'`

## egrep
允许使用扩展正则表达式,也可以通过`grep -E`

## look
选取以特定模式开头的行
look必须从一个或者多个文件中获取数据,且是字母排序的
look pattern

## sort
`sort name > masterfile`

指定输出文件
`sort -o names names`

使用数字排序
`sort -n`

反向排序
`sort -r`

查找相同的行,只留下一行
`sort -u`

检查数据是否有序:
`sort -c`

`man ascii`
空格>数字>大写字母>小写字母

区域设置:
`locale`
`echo $LC_COLLATE`
en_US: 'aAbBcCdD'
C: 'ABC...abc...'

## uniq
查找重复行

消除重复行:
`uniq data`
输出到制定文件processed-data
`uniq data processed-data`
只查看重复行:
`uniq -d data`
只查看唯一行:
`uniq -u data`
统计每行重复出现的次数:
`uniq -c data`

## join
合并两个文件中的[有序]数据:

基于共同值组合文件:
`join name phone`

输出第1个文件中的所有行,即便与第2个文件没有匹配
`join -a1 birthday gifts`

只输出第1个文件中的不匹配行
`join -v1 birthday gifts`

使用第一个文件中的第3个字段,和第二个文件中的第4个字段
`join -1 3 -2 4 data statistics`

## tsort
由偏序创建全序

## whereis
`whereis sort`

## tr
转换字符

将所有a转换为A
`tr a A < old`
`tr abc ABC < old > new`

`tr abcde Ax < old > new` == `tr abcde Axxxx < old > new`

将所有冒号,分号和问号替换成点号
`tr ':;?' \. < old > new`

`tr A-Z a-z < old > new`
`tr [:upper:] [:lower:] < old > new`

`tr '\r' '\n' < macfile > unixfile`

挤压,替换成一个单独的X
`tr -s 0-9 X < old > new`
`tr -s ' ' ' ' < old > new`

删除一组字符,删除所有(和)
`tr -d '()' < old > new`

匹配所有不在第一组中的字符
`tr -c ' \n' X < old > new

## sed
`sed 's/harley/Harley/g' names > newnames`

替换原来文件:-i
`sed -i 's/a/A/g' names > newnames`

只改变数据流的第5行
`sed '5s/a/A/g' names`

只改变最后一行
`sed '$s/a/A/g' names`

改变5-10
`sed '5,10s/a/A/g' names`

改变包含指定模式的行,如,包含OK的
`sed '/OK/s/harley/Harley/g' names`

运行程序文件
`sed -f sed-file input`

-e指定任意多的sed命令
`sed -i -e 's/mon/Monday/g' -e 's/tue/Tuesday/g' calendar`

## Regex
![扩展正则表达式和基本正则表达式](/assets/image/regexr.png)

sed只支持基本正则表达式

![正则表达式基本匹配](/assets/image/basic-regex.png)

搜索完整单词
`grep '\<know\>' data`

在GNU实用工具系统上:Linux, FreeBSD可以使用`\b`替代

`grep -w 'cat' data`
`grep '\<cat\>' data`
`grep '\bcat\b' data`

## 预定义字符类:
![预定义字符类](/assets/image/regex-predefined.png)

最古老的Unix程序存放在/bin目录中
如果`grep`发现满足不了需求,那么就用`egrep`

## 文件

### editor
不允许编辑查看
`vi -R /etc/passwd`
`view /etc/passwd`

### less

```
q h... <br>
g: 跳到地一行 <br>
G: 最后一行 <br>
/pattern ?pattern n N !command <br>
=:显示当前行号和文件名 <br>
n<return>前进n行 <br>
40p: 跳转到40% <br>
40g: 40行 <br>
```

原始模式:raw mode: 键一按下,就立马执行
规范模式:缓冲...

man调用分页程序,来显示页面, `echo $PAGER`

!(less处理多个文件的能力)[/assets/image/less-command.png]

### head
`head -n 20 information`

### 观察不断增长的文件末尾:tail -f

到达末尾的时候不要停止,而要等待下去
`tail -f result`

同时监视多个文件
`tail -f file1 file2`

### od(octal dump)
八进制转储

显示二进制数据
`hexdump -C [file...]`
`od -Ax -txlz /bin/ls | less`

## vi
vi: visual editor
vim: vi improved

### 只读方式启动vi
`view`
`vi -R`

### 系统失败后数据的恢复
`vi -r test.c`

### 停止vi
`ZZ`

大多数vi命令:单字母或者双字母的形式,无需<return>
ex命令:以:开头

删除键入的最后一个单词(输入模式下): `^W`
hello

### move cursor
-: 光标移动到上一行的开头
+: 光标移动到下一行的开头
换行符: 下一行开头
0: 行开头
$: 行末尾
^: 行第一个非空格/制表符的字符上
w: 下一个单词词首
e: 下一个单词词尾
b: 上一个单词词首
W: 同w, 但是忽略标点符号
E: 同e, 但是忽略标点符号
B: 同b, 但是忽略标点符号
): 下一个句子
(: 上一个
 {: 下一个段落
} 向后移动到上一个段落
H: 屏幕顶部
M: 屏幕中间
L: 屏幕底部
10w, 50<Down>, 3{
``: 两个反引号,返回到前一位置
ma: 用名称'a'记住这一行,然后使用`a来返回
:set number
:set nonumber
100G: 跳转到100行
gg == 1G
:100<Return>
G: 末尾

### 插入模式
i a I(行开头) A(行末尾) o Ox
^U: 删除刚才键入的一系列字符?

### 修改文本
ra: 将光标出的字符修改为a
s: 多个字符替换一个单个字符
cc: 插入方式替换当前整行
:s/UNIX/Linux/cg: c: confirm
:%s/UNIX/Linux/g == :1,$/UNIX/Linux/g

### 删除文本
x: 删除光标处的字符
X: 删除光标左边的字符
D: 删除当前光标到本行末尾的字符
dd: 删除整行
:lined 删除指定行
:line, lined: 删除指定范围的行
dw: w是指光标移动命令所指示的所有字符
d10w, d10W, db, d2) d5} dG dgg

### 撤销或者重复改变
u: 撤销上一命令对编辑缓冲区的修改
U: 恢复当前行
.: 重复上一命令对编辑缓冲区的修改

### 移动文本
p:插入到当前行的下面
P: 当前行的上面

### 复制
yx, y10w, y10w, yb, y2), y5} yy, 10yy

### 改变大小写
将光标移动到字符上,然后按下~,将改变大小写

###
:set showmode nonumber tagstop=4
:set smd nonu ts=4
:set all

### 复制与移动行
:5co10 : 复制第5行,插入倒第10行下面
:4,8co20: 复制4-8
:5m10 移动第5行
:.,$m0

### 输入shell命令
:!date
:!! 上次命令

### 将文件中的数据插入倒编辑缓冲区中
:10r info--->将文件info的内容插入到第10行之后
:r info--->插入到当前行之后
:r !ls---->将程序执行结果插入到

### 保存
:w
:w file
:w! file
:w >> file
:10,20w >> save

### 切换到一个新的文件
:e document

### 初始化文件:.exrc .vimrc

## 文件
### 特殊文件
用来表示屋里设备的伪文件:
/ls /dev | less

### tty
显示你当前正在使用哪一个伪终端工作
`cp /etc/passwd /dev/tty`
dd if=/dev/zero of=temp bs=100 count=1 100字节的数据

### 命名管道
```
mkfifo fifotest
grep bash /etc/passwd > fifotest
wc -l < fifotest
rm fifotest
```
### proc文件
proc文件直接从内核获取信息,`/proc`
`cat /proc/cpuinfo`

### 漫游根目录
/ 根目录
/dev 设备文件
/etc 配置文件
/usr 静态数据使用的辅助文件系统
/opt 安装第三方应用程序的位置

## 目录
### 使用-p 创建全部
`mkdir -p ~/essays/history/roman/unix/research`
`rmdir essays/history`

### 移动或重命名目录
`mv data extra`

### 目录栈 directory stack
dirs, pushd, popd

### ls
```
ls -1 数字1:每行一项
ls -r: 逆序
ls -F: 文件类型
ls --color
```

### file
检查文件类型
`file /etc/* | less`

### 掌握磁盘空间适用情况
```
ls -hs (human-readable, size)
du (disk usage: KB)
df 
quota
```

### ls 通配符
```
Ha*
?: 一个字符
d?
??
?*y
```

## 文件
### cp

-i: interactive 交互的
`cp -i xx xx` 

文件权限
![file permissions](/assets/image/file-permission.png)

`id`
`chmod 644 essay1`
`umask 022` 为新文件指定权限的的方式

方便的查看某个特定的文件的i节点的内容
`stat filename`

创建新连接:不能为目录创建链接,不能为不同文件系统中的文件创建链接
`ln file newname`

`ln -s`

查找一个特定的文件:whereis, locate, find
`where is`

搜索数据库查看文件: 自动维护,定期更新
`locate -r '.jpg$' > photos`

搜索目录树:find
syntax: find path...test...action...
可以指定不止一个路径
find测试
![find-test](/assets/image/find-test.png)

`find /usr -type d -name bin -print`
`find ~ -typ f -size 1K -print`
`find ~ -type f \! -name '*.jpg' -print`

![find-action](/assets/image/find-action.png)
`find ~ -name '*.backup' -exec ls -dils {} \`

处理查找到的文件: -exec: 为每个文件生成一个单独的命令, xargs:输出到管道
`find ~ -type f -mtime +365 | xargs grep "zk"`
`find ~ -type f | xargs echo | wc -w`
添加参数-i:insert, {}占位符
`find . -type f | xargs -i mv {} ~/backups/{}.old`

## 进程
### 进程分叉
echo $$: 显示shell pid

### 后台进程
`sor < bigfile > result &`

创建延迟
```
sleep 5
sleep 5s
sleep 5m
sleep 5h
```

~[job-control](/assets/image/job-control.png)

显示作业控制列表:
`jobs [-l]`

![fg](/assets/image/fg.png)
![zk]({{ site.baseurl }})

bg将作业移至后台
`fg [%job...]`
![bg-tip](/assets/image/fg.png)

### ps
![ps-option]/assets/image/ps.png)

使用top每隔几秒钟显示系统整体的信息
`top-d 1`

显示进程树
`pstree`

### kill
`kill 5505`
`kill %2`
`kill -9 pid`
![kill-9](/assets/image/kill-9.png)

发送信号:
kill [-signal] pid...|jobid...
![signal](/assets/image/signal.png)

查看自己系统支持哪些信号
```
kill -l
![kill-9-tip](/assets/image/kill-9-tip.png)

locate signal.h
find / -name 'signal.h' =print 2> /dev/null
```

`kill -STOP %2`
`kill 3662` #TERM

### nice
默认为10
`nice gcc myprogram.c &` 以一个较低的优先级运行

-20 ~ -1: 较高优先级,root权限
`nice -n 19 xxx`

`renice 19 -p 4089`

## apt-remove

1. find software that you are insteresting:
```
sudo dpkg -l | grep sogou*
```

2. remove it:
```
sudo apt-get purge sogoupinyin
```

## x-window to tty

```
Ctrl+Alt+F(1,2,3,4,5,6)
Alt+F7 #to x-window
```
## Ubuntu Terminal 多标签切换

```
alt + 1
alt + 2
alt + 3
...

ctrl + PageUp
ctrl + PageDown
```

## 查看当前内存使用情况

```bash
free -h
```

## 查看某个pid开了几个线程

```bash
cat /proc/${PID_NUMBER}/status
```

## [Mac 下面批量替换](http://unix.stackexchange.com/questions/112023/how-can-i-replace-a-string-in-a-files)
```bash
find . -type f -exec sed -i '' 's/foo/bar/g' {} +
```
