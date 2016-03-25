---
title: "Vim Skills"
layout: post
date: 2015-08-26
tags:
- vim skills
categories:
- tools
---

## Read the Forgotten Manual

- `:h vimtutor`: Vim tutor

## Do not repeat yourself

`A` command compounds two actions `$a` into a single keystroke. It is not alone in doing this. The table below shows an approximation of some examples:

![vim-skills-tip-2-1](/assets/image/vim/vim-skills-tip-2-1.png)

## Find and Replace by Hand

- `*`(see `:h *`) command executes a search for the word under the cursor at the moment

## Tip 12: Combine and Conquer

`Operator + Motion = Action`

- `d{motion}` command can operate on a single character `dl`, a complete word `daw`, or an entire paragraph `dap`. Its reach is defined by the motion. The same goes for `c{motion}`, `y{motion}`，你可以通过使用`:h operator`来查找完整的列表
- 当`g`被用来作前缀的时候，那么他就是为了改变命令第二个字符的行为的
- 注释：`\\{motion}`. `\\ap`:注释一段，`\\G`:从当前注释到文件尾，`\\\`注释当前行

Vim Operator Commands:

![vim-skills-table-2](/assets/image/vim/vim-skills-table-2.png)

## Tip 19: Overrite existing text with replace mode

- `r{char}`: overwrite a single character before switching straight back to Normal mode (see `:h r`)

## Tip 21: Define a Visual Selection

- `v`: Enable character-wise Visual mode.
- `V`: Enable line-wise Visual mode. 
- `<C-v>`: Enable block-wise Visual mode.

## Tip 27: Meet Vim Command Line

当你按下`:`的时候，你进入了Command-Line mode，然后按`<Esc>`就切换回来。

## Tip 28: Execute a command on one or more consecutive lines

Many Ex commands can be given a [range] of lines to act upon. We can specify the start and end of a range with either a line number, a mark, or a pattern.

- `:1`:jump to the top of the line
- `:$`:jump to the end of the file
- `:p`:shorthand for `:print`, 打印当前行
- `:3p`:move the cursor to line 3, and echoes the content of that line.
- `2,5p`:prints each line from 2 to 5.
- `:.,$p`: . symbol represent the current line
- `:%p`: % stands for all the lines in the current file

## Tip 35: Run Commands in the shell

- 使用`:!命令`来执行你想执行的命令
- 在Vim的command-line模式下，`%`是当前文件的简写
- `:shell`完全进入shell模式，使用`exit`返回

## Tip 47: Distinguish between real lines and display lines

请看下面这幅截图：光标在'n'上闪烁，如果你想移动到'vehicula'这个单词上，那么必须按`gk`才行，因为`k`是根据real line移动的，而`gk`是根据display line移动的

![vim-skills-tip-47-1](/assets/image/vim/vim-skills-tip-47-1.png)

下面这张表：汇总了display line和real line可以交互的一些命令：

![vim-skills-tip-47-2](/assets/image/vim/vim-skills-tip-47-2.png)

`g` 告诉vim你想要操控display lines

## Tip 48: Move Word-Wise

向前或者向后移动一个单词：

![vim-skills-tip-48-1](/assets/image/vim/vim-skills-tip-48-1.png)

## Tip 49: Find by Character

`f{char}` 在一行内搜索指定字符出现的第一个位置，然后调到那个字符所在的位置。使用`;`继续向后搜索，使用`,`向前搜索

Character searches can include or exclude the target:

![vim-skills-tip-49-1](/assets/image/vim/vim-skills-tip-49-1.png)

- `dt.` 删除当前光标所在字符到点号之前的所有字符

In genral, I tend to use `f{char}` and `F{char}` in Normal mode when I want to move the cursor quickly within the current line, whereas I tend to use the `t{char}` and `T{char}` character search commands in combination with `d{motion}` or `c{motion}`

## Tip 50: Search to Navigate

`/takes<CR>`:搜索，`n`:前进，`N`:后退

## Tip 51: Trace your selection with precision text objects

- `c{motion}`: delete the specified text and switches to Insert mode (`:h c)
- When we press `vi}`, Vim initiates Visual mode and then selects all of the characters containerd by the {} braces.
- vim text objects consist of two characters, the first of which is always either `i` or `a`. think of `i` as inside and `a` as around (or all).

## Tip 54: Jump Between Matching Parentheses

`%`可以在匹配的`(),{},[]`中跳来跳去

## Traverse the jump list

- [count]G: jump to line number

## Tip 59: Delete, Yank, and Put with Vim Unnamed Register

- `x` command cuts the character under the cursor, placing a copy of it in the unnamed register.

默认使用的寄存器就叫做匿名寄存器

## Tip 87: Meet the Substitute command

Syntax: `:[range]s[ubstitute]/{pattern}/{string}/[flags]`

flags:
- `g`: globally
- `c`: confirm
- complete reference flags: see `:h :s_flags`

## Tip 89: Eyeball Each Substitution

- `:%s/content/copy/gc`: Replace with copy? We can say `y` to perform the change or `n` to skip it. we are not limited to just two answers. this table shows what each answer means:(see `:h :s_c`)

![vim-skills-tip-89-1](/assets/image/vim/vim-skills-tip-89-1.png)

## Repeatable Actions and How to Reverse Them

可重复执行的命令以及如何反向操作

![vim-skills-table-1](/assets/image/vim/vim-skills-table-1.png)

## Vim Operator Commands


## Ex Commands That Operate on the Text in a Buffer

![vim-skills-table-9](/assets/image/vim/vim-skills-table-9.png)

## Delimited Text Objects

![vim-skills-table-12](/assets/image/vim/vim-skills-table-12.png)

## Cut and paste:

Cut and paste:
Position the cursor where you want to begin cutting.
Press v to select characters (or uppercase V to select whole lines).
Move the cursor to the end of what you want to cut.
Press d to cut (or y to copy).
Move to where you would like to paste.
Press P to paste before the cursor, or p to paste after.

## 配置ubuntu

打开 `/etc/vim/vimrc`
```bash
set nu                                                                      
set tabstop=4
set ruler #左下角显示当前状态
set autoindent
set cursorline #显示当前编辑的行线
取消行号  :set nonumber
```