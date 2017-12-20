---
title: git 使用记录
date: 2017-11-23 11:12:08
categories: Git
tags: Git
---

使用 git 遇到的一些问题

<!-- more -->

## 撤销全部修改
```
git checkout .
```

## 同步到远程仓库
`error:failed to push some refs to git`
先把远程仓库和本地仓库合并：
```
git pull --rebase origin master
```
再执行：
```
git push -u origin master
```

## warning: LF will be replaced by CRLF
问题出在不同操作系统所使用的换行符是不一样的。
在 Git 中，可以通过以下命令来显示当前你的 Git 中采取哪种对待换行符的方式。
```
git config --global core.autocrlf false
```
false 表示 line endings 不做任何改变，文本文件保持原来的样子， 还可以为 true 或者 input。
