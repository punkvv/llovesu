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