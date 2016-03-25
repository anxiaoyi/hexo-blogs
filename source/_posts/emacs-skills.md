---
title: "The emacs skills"
layout: post
date: 2015-11-28
tags:
- emacs
categories:
- emacs
---

`Ubuntu` 跳到文件末尾：
```bash
M-> (Alt + Shift + >)
```

`Ubuntu` 跳到文件开头：
```bash
M-< (Alt + Shift + <)
```

`Ubuntu` 搜狗输入法不能切换的问题：

[在Emacs 24.3 Ubuntu14.04英文版中使用搜狗输入法](http://heartnheart.github.io/blog/2015/01/15/SogouIME_on_English_Ubuntu_14.04/)

- 方法一，从命令行启动：

```bash
LC_CTYPE='zh_CN.UTF-8' emacs
```

- 方法二，从Dash（搜索框）启动：
1. 重命名默认启动的emacs24-x
```bash
sudo mv /usr/bin/emacs24-x /usr/bin/emacs24-x_original
```
2. 创建新的名为emacs24-x的脚本：
```bash
oecho "LC_CTYPE='zh_CN.UTF-8' emacs24-x_original" | sudo tee /usr/bin/emacs24-x
sudo chmod a+x /usr/bin/emacs24-x
```

`ubuntu` 自动创建以`~`(tilde)结尾的文件：
```lisp
(setq make-backup-files nil)
```

Emacs 快捷键

```bash
M-< : Go to the start of file
M-> : Go to the end of file
```