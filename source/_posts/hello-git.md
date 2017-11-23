---
title: Hello Git
date: 2017-10-20 14:01:31
categories: Hello World
tags:
  - Hello World
  - Git
---

Git 入门

<!-- more -->

# 安装

Ubuntu ：通过执行 `sudo apt-get install git` 
Windows ：从 [https://git-for-windows.github.io](https://git-for-windows.github.io) 下载
安装完成后，设置名字和 Email：
```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

# 初始化

1. 创建一个版本库，在目录下执行：
    ```git
    $ git init
    ```

2. 新建一个 abc.txt ,然后把文件添加到版本库中：
    ```git
    $ git add abc.txt
    ```

3. 把文件提交到仓库：
    ```git
    $ git commit -m "提交说明"
    ```
    
# 基本操作

修改 abc.txt 文件，加入以下文字：
```text
aaaaaaaa
```
然后运行：
```git
$ git status
```
`git status` 命令可以查看仓库当前的状态,可查看到 abc.txt 已经被修改，但还没有准备提交。 

需要查看具体修改了什么内容，可运行：
```git
$ git diff adb.txt
```

最后提交修改：
```git
$ git add
```
```git
$ git commit -m "修改了abc.txt"
```

# 版本回退

使用 `git log` 命令查看历史记录，可以使用 `git log --pretty=oneline` 减少输出信息。

要把当前的版本回退到上一个版本： `git reset --hard HEAD^` ，上上一个版本只需要把 **HEAD^** 改成 **HEAD^^** 以此类推。
如果往上100个版本可以写成 **HEAD~100**

恢复到新版本 `git reset --hard 版本号` , 查看命令历史 `git reflog` 可查看到版本号。

# 工作区与暂存区

工作区就是我们初始化仓库的目录。

版本库就是初始化后生成的 **.git** 这个隐藏目录，里面包含：**暂存区(stage)**和 Git 自动创建的第一个分支 **master**，以及指向 **master** 的一个指针叫 **HEAD**。

当我们 `git add` 时，实际上就是把文件修改添加到暂存区，`git commit` 时，实际上就是把暂存区的所有内容提交到当前分支。

# 撤销修改和删除文件

使用 `git checkout -- abc.txt` 把 abc.txt 文件在工作区的修改全部撤销。
使用 `git reset HEAD abc.txt` 把暂存区的修改撤销掉，重新放回工作区。

使用 `git rm abc.txt` 删除一个文件。

# 远程仓库

使用 [GitHub][github] 作为远程仓库。

GitHub提供两种远程方式,如果要使用 SSH 认证，则需要：
1. 创建 SSH Key : 在用户主目录下查看是否存在 **.ssh** 目录，再查看这个目录下是否有 **id_rsa** 和 **id_rsa.pub** 这两个文件，  如果不存在，执行命令
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
2. 登录 GitHub 在 Settings 里面的 SSH and GPG keys 页面，点击 New SSH key 把 **id_rsa.pub** 的内容粘贴到 Key 文本框里。

把本地仓库与远程仓库关联,在本地仓库运行命令：
```
$ git remote add origin https://github.com/punkvv/yuqi.git
```
下一步，就可以把本地库的所有内容推送到远程库上：
```
$ git push -u origin master
```
由于远程库是空的，我们第一次推送 master 分支时，加上了 **-u** 参数，Git 不但会把本地的 master 分支内容推送的远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来，在以后的推送或者拉取时就可以简化命令。
```
$ git push origin master
```

从远程克隆库：
```
$ git clone https://github.com/punkvv/yuqi.git
```

# 分支管理

创建 **dev** 分支,并切换到 **dev** 分支：
```
$ git checkout -b dev
```
**-b** 参数表示创建并切换，相当于：
```
$ git branch dev
$ git checkout dev
```
用 `git branch` 查看当前分支：
```
$ git branch
```
合并指定分支到当前分支：
```
$ git merge dev
```
删除分支：
```
$ git branch -d dev
```

当合并分支有冲突时 Git 用 **<<<<<<<**，**=======**，**>>>>>>>** 标记出不同分支的内容：
```
<<<<<<< HEAD
aaa
=======
bbb
>>>>>>> dev
```
当 Git 无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用 `git log --graph` 命令可以看到分支合并图。

合并分支时，加上 **--no-ff** 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 fast forward 合并就看不出来曾经做过合并。
```
$ git merge --no-ff -m "merge with no-ff" dev
```

存储工作现场：
```
$ git stash
```
查看：
```
$ git stash list
```
恢复：
```
$ git stash apply
```
删除：
```
$ git stash drop
```
恢复并删除：
```
$ git stash pop
```
可以多次 stash ，恢复的时候，先用 `git stash list` 查看，然后恢复指定的 stash ：
```
$ git stash apply stash@{0}
```

开发一个新 feature ，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过 `git branch -D <name>` 强行删除。


查看远程库信息，使用 `git remote -v`。

本地新建的分支如果不推送到远程，对其他人就是不可见的。

从本地推送分支，使用 `git push origin branch-name` ，如果推送失败，先用 `git pull` 抓取远程的新提交。

在本地创建和远程分支对应的分支，使用 `git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用 `git branch --set-upstream branch-name origin/branch-name`。

从远程抓取分支，使用 `git pull`，如果有冲突，要先处理冲突。


# 标签管理

首先，切换到需要打标签的分支上：
```
$ git checkout master
```
然后，使用命令：
```
$git tag v1.0
```
就可以打一个新标签。
可以使用 `git tag` 查看所有标签。
默认标签是打在最新提交的commit上的。

如果需要对特定 commit 打标签，则需要找到历史提交的 commit id：
```
$ git log --pretty=oneline --abbrev-commit
```
然后：
```
$ git tag v0.9 <commit id>
```

使用：
```
$ git show <tagname>
```
查看标签信息。


还可以创建带有说明的标签，用 **-a** 指定标签名，**-m** 指定说明文字：
```
$ git tag -a v0.1 -m "version 0.1 released" <commit id>
```

删除标签：
```
$ git tag -d <tagname>
```
推送标签到远程：
```
$ git push origin <tagname>
```
把本地标签全部推送到远程：
```
$ git push origin --tags
```
如果标签已经推送到远程，需要删除远程标签，要先从本地删除：
```
$ git tag -d <tagname>
```
然后，从远程删除:
```
$ git push origin :refs/tags/<tagname>
```

# 自定义 Git
忽略特殊文件，通过添加 **.gitignore** 文件，然后配置忽略规则 [.gitignore][.gitignore]

可以使用：
```
$ git config --global alias.<别名> <命令>
```
来使命令简写。

配置个性化 log：
```
$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
``` 
每个仓库的配置文件在 **.git/config** 文件中。
当前用户的配置文件在用户主目录下的一个隐藏文件 **.gitconfig** 中。

参考 [Git教程][gitjc]、[GitHub][github]、[Git][git]

[gitjc]: https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000
[github]: https://github.com
[.gitignore]: https://github.com/github/gitignore
[git]: https://git-scm.com
