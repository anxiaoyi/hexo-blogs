---
title: "Linux shell scripting"
layout: post
date: 2015-08-23
tags:
- linux
categories:
- linux
---
# Linux shell scripting

## Shell something out

- Finding length of string
```sh
$ var=12345678901234567890
$ echo ${#var}
```

- Check for super user
```sh
if [ $UID -ne 0 ]; then
echo Non root user. Please run as root.
else
echo "Root user"
fi
```

- Doing math calculations with the shell

```sh
let result=no1+no2
echo $result

result=$[ no1 + no2 ]

result=$(( no1 + 50 ))

result=`expr 3 + 4`
result=$(expr $no1 + 5)
```

All of the above methods do not support floating point numbers, and operate on integers only.

```sh
result=`echo "$no * 1.5" | bc`
echo "scale=2;3/8" | bc
```

- Playing with file descriptors and redirection
- Arrays and associative arrays

```sh
array_var=(1 2 3 4 5 6)

array_var[0]="test1"
array_var[1]="test2"

index=5
$ echo ${array_var[$index]}

$ echo ${array_var[*]} # print all
$ echo ${array_var[@]} # print all
$ echo ${#array_var[*]} # length
```

Defining associative arrays

```sh
$ declare -A fruits_value
$ fruits_value=([apple]='100dollars' [orange]='150 dollars')
$ echo "Apple costs ${fruits_value[apple]}"
$ echo ${!array_var[*]}
$ echo ${!array_var[@]}
$ echo ${!fruits_value[*]}
```

- Visiting aliases
```sh
$ echo 'alias cmd="command seq"' >> ~/.bashrc
```

- Functions and arguments

A function can be defined as follows:
```sh
function fname()
{
        statements;
}
# Or alternately,
fname()
{
        statements;
}
```

A function can be invoked just by using its name:
```sh
$ fname ; # executes function
fname arg1 arg2 ; # passing args
```

Example: 
```sh
fname()
{
        echo $1, $2; #Accessing arg1 and arg2
        echo "$@"; # Printing all arguments as list at once
        # "$*" expands as "$1c$2c$3" , where c is the first character of IFS
        echo "$*"; # Similar to $@, but arguments taken as single entity
        return 0; # Return value
}
```

Reading command return value (status)
```sh
cmd;
echo $?;
```

- For loop:

```sh
for var in list;
do
commands; # use $var
done
list can be a string, or a sequence.

for i in {a..z}; do actions; done;

for((i=0;i<10;i++))
{
        commands; # Use $i
}
```

### Comparisons and tests

- Mathematical comparisons:
```sh
[ $var -eq 0 ] # It returns true when $var equal to 0.
[ $var -ne 0 ] # It returns true when $var not equals 0
[ $var1 -ne 0 -a $var2 -gt 2 ] # using AND -a
[ $var -ne 0 -o var2 -gt 2 ] # OR -o
`-gt` : Greater than
`-lt` : Less than
`-ge` : Greater than or equal to
`-le` : Less than or equal to

if condition;
then
commands;
elif condition;
then
commands
else
commands
fi
```

- String comparisons:
```sh
[[ $str1 = $str2 ]] : Returns true when str1 equals str2, that is, the text
contents of str1 and str2 are the same
[[ $str1 == $str2 ]] : It is alternative method for string equality check
We can check whether two strings are not the same as follows:
[[ $str1 != $str2 ]] : Returns true when str1 and str2 mismatches
We can find out the alphabetically smaller or larger string as follows:
[[ $str1 > $str2 ]] : Returns true when str1 is alphabetically greater than str2
[[ $str1 < $str2 ]] : Returns true when str1 is alphabetically lesser than str2
[[ -z $str1 ]] : Returns true if str1 holds an empty string
[[ -n $str1 ]] : Returns true if str1 holds a non-empty string

if [[ -n $str1 ]] && [[ -z $str2 ]] ;
then
commands;
fi
```

- Filesystem related tests:
```sh
[ -f $file_var ] : Returns true if the given variable holds a regular filepath or filename.
[ -x $var ] : Returns true if the given variable holds a file path or filename which is executable.
[ -d $var ] : Returns true if the given variable holds a directory path or directory name.
 [ -e $var ] : Returns true if the given variable holds an existing file.
 [ -c $var ] : Returns true if the given variable holds path of a character device file.
 [ -b $var ] : Returns true if the given variable holds path of a block device file.
 [ -w $var ] : Returns true if the given variable holds path of a file which is writable.
[ -r $var ] : Returns true if the given variable holds path of a file which is readable.
[ -L $var ] : Returns true if the given variable holds path of a symlink.
```

- Mac在命令行窗口中切换快捷键
`Win+Shift+左箭头(右箭头)`

- Kill Process By Name
```sh
killall python
pkill
```

- Shell Parameter
[Shell Parameter Expansion](http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html#Shell-Parameter-Expansion)

- Mac Keyboard shortcuts
[Mac Keyboard shortcuts](https://support.apple.com/en-us/HT201236)

- 数组循环
```bash
array=("abc" "def" "ghi") #注意，不能有逗号

for str in ${array[@]}; #循环
do
# do something with ${str}
done;
```
