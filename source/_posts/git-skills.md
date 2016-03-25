---
title: "Git skills"
layout: post
date: 2015-04-12
tags:
- git
categories:
- software
---

## Git 初始化

```bash
# 查看版本号
git --version

# 设置name和email
git config --global user.name "kun.zhao"
git config --global user.email igozhaokun@163.com

# 用下面命令检查
git config user.name
git config user.email

# 删除name和email
git config --unset --global user.name
git config --unset --global user.email

# 设置命令别名，以后就可以:git st执行
git config --global alias.st status

# 开启颜色显示
git config --global color.ui true

# 创建版本库
git init
```

## Git 暂存区(stage)

```bash
# 查看提交日志，--stat查看每次提交的文件变更统计
git log --stat

# 查看精简版的提交日至
git log --pretty=oneline [文件名]

# 查看文件某次的改动
git show 356f6def9d3fb7f3b9032ff5aa4b9110d4cca87e

# ASCII 字符串表示的简单图形，形象地展示了每个提交所在的分支及其分化衍合情况
git log --graph

# 显示精简格式的状态输出
git status -s
```

### 理解stage

### Git diff

```bash
# 工作区和暂存区比较
git diff

# 暂存区和HEAD比较
git diff --cached

# 工作区和HEAD比较
git diff HEAD
```

## Git 重置

```bash
# 用HEAD指向的目录树重置暂存区，工作区不受影响。相当于用git add命令更新到暂存区的内容撤出暂存区
git reset

# 同上
git reset HEAD

# 仅将文件filename的改动撤出暂存区，暂存区中其它文件不改变。相当于对git add filename的反向操作
git reset -- filename

# 同上
git reset HEAD filename

# 工作区和暂存区不改变，但是引用向前回退一次。当对最新提交的提交说明或者提交的更改不满意时，撤销最新的提交以便重新提交
git reset --soft HEAD^

# 工作区不改变，但是暂存区回退到上一次提交之前，引用也回退一次
git reset HEAD^

# 同上
git reset --mixed HEAD^

# 彻底撤销最近的提交，引用，工作区和暂存区都会回退到上一次提交的状态
git reset --hard HEAD^
```

## Git 检出

```bash
# 检出branch分支。更新HEAD以指向branch分支，以及用branch指向的树更新暂存区和工作区
git checkout branch

# 汇总显示工作区、暂存区和HEAD的差异
git checkout

# 同上
git checkout HEAD

# 用暂存区中的filename来覆盖工作区的filename文件。相当于取消自上次执行git add filename以来的本地修改。这个命令很危险，本地修改会悄无声息的覆盖，毫不留情
git checkout -- filename

# 维持HEAD的指向不变，用branch所指向的提交中的filename替换暂存区和工作区中相应的文件
git checkout branch -- filename

# 取消所有本地修改，用暂存区的所有文件直接覆盖本地文件
git checkout -- .
git checkout .
```

## 恢复进度

```bash
# 保存当前工作进度。会分别对暂存区和工作区的状态进行保存
git stash

# 查看保存的进度
git stash list

# 恢复进度，并将恢复的工作进度从存储的工作进度中清除
git stash pop

# 不删除恢复的进度，其余和git stash pop一样
git stash apply

# 删除一个存储的进度，默认删除最新的进度
git stash drop

# 删除所有存储的进度
# git stash clear
```

## git 基本操作

### git 忽略语法

```bash
# 这是注释行
*.a # 忽略所有以.a为扩展名的文件
!lib.a #不忽略
/TODO #只忽略此目录下的TODO文件，子目录的TODO文件不忽略
build/ #忽略所有build/目录下的文件
doc/*.txt #忽略doc/nots.txt，但是不忽略doc/server/arch.txt
```

### 文件归档

```bash
# 基于最新提交建立归档文件latest.zip
git archive -o latest.zip HEAD
```

## 改变历史

## Git 克隆

## Git协议与工作协同

## 对于已经修改提交过的注释，如果需要修改，可以借助 git commit --amend 来进行。

### View the change history of a file

```bash
git log --follow -p -- file
```

### Getting a fatal error in git for multiple stage entries

```bash
rm .git/index

# after deleting index you need to recreate it by git reset
git reset
```

### Undo the last commit

```bash
git reset --soft HEAD~  
```

### To stop tracking a file that is currently tracked, use `git rm --cached`.

```bash
git rm --cached ProjectFolder.xcodeproj/project.xcworkspace/xcuserdata/myUserName.xcuserdatad/UserInterfaceState.xcuserstate
```