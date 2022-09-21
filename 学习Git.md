---
title: 学习Git
date: 2022-09-19 16:47:52
categories: 
  - Git
tags:
  - git
---

## Git
Git 是一个开源的**分布式-版本控制系统**，用于敏捷高效地处理任何或小或大的项目。
Git 与常用的版本控制工具 CVS, Subversion（SVN）等不同，它采用了分布式版本库的方式，不必服务器端软件支持。
Git 不仅仅是个版本控制系统，它也是个内容管理系统(CMS)，工作管理系统等。
<!--more-->
你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 工作目录（工作目录下含有.git文件夹），它持有实际文件；第二个是 暂存区（Index，stage），它像个缓存区域，临时保存你的改动；最后是 HEAD，它指向你最后一次提交的结果。
![本地仓库组成](https://www.runoob.com/manual/git-guide/img/trees.png)

> **git 基本工作流程**
> 1. 提出更改（把它们添加到暂存区），使用如下命令：
>  `git add <filename>` 
>
> 2. 使用如下命令以实际提交改动：
>  `git commit -m "代码提交备注" `
>  现在，你的改动已经提交到了本地仓库的 HEAD 中，但是还没到你的远端仓库。
>
> 3. 执行如下命令以将这些改动推送到远端仓库：
>  `git push origin master`
>  可以把 master 换成你想要推送的任何分支。
>
> 4. 如果你想要将你的本地仓库连接到某个远程服务器（github），你可以使用如下命令添加：
>  `git remote add origin https://github.com/Iridescent-zhang/blog.git`
>  如此你就能够将你的改动推送到所添加的服务器上去了。
>
> 5. 在你创建仓库的时候，master 是“默认的”分支。在其他分支上进行开发，完成后再将它们合并到主分支上。
> ![分支](https://www.runoob.com/manual/git-guide/img/branches.png)
> 创建一个叫做“feature_x”的分支，并切换过去：
> `git checkout -b feature_x`
> 切换回主分支：
> `git checkout master`
> 再把新建的分支删掉：
> `git branch -d feature_x`
> 除非你将分支推送到远端仓库，不然该分支就是 不为他人所见的：
> `git push origin <branch>`
> 
> 6. 更新与合并
> 要更新你的本地仓库至最新改动，执行：
> `git pull`
> 以在你的工作目录中 获取（fetch） 并 合并（merge） 远端的改动。
> 要合并其他分支到你的当前分支（例如 master），执行：
> `git merge <branch>`
> 这两种情况下，git 都会尝试去自动合并改动。遗憾的是，这可能并非每次都成功，并可能出现冲突（conflicts）。 这时候就需要你修改这些文件来手动合并这些冲突（conflicts）。
> 在合并改动之前，你可以使用如下命令预览差异：
> `git diff <source_branch> <target_branch>`
>
> 7. 标签
> 你可以执行如下命令创建一个叫做 1.0.0 的标签：
> `git tag 1.0.0 1b2e1d63ff`
> b2e1d63ff 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：
> `git log`
> 
> 8. 替换本地改动
> 假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
> `git checkout -- <filename>`
> 此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。
> 假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
> `git fetch origin`
> `git reset --hard origin/master`
> 

## Git 工作流程
> 一般工作流程如下：
> - 克隆 Git 资源作为工作目录(git clone https://github.com/Iridescent-zhang/blog.git)。
> - 在克隆的资源上添加或修改文件。
> - 如果其他人修改了，你可以更新资源。
> - 在提交前查看修改。
> - 提交修改。
> - 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。
> ![Git工作流程](https://www.runoob.com/wp-content/uploads/2015/02/git-process.png)
> 

## Git 工作区、暂存区和版本库
- 工作区：就是你在电脑里能看到的目录。
- 暂存区：英文叫 stage 或 index。一般存放在 .git 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- 版本库：工作区中有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。

> ![工作区、版本库中的暂存区和版本库之间的关系](https://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)
> - 在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。
> - 图中我们可以看出此时 "HEAD" 实际是指向 master 分支的一个"游标"。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
> - 图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。
> - 当对工作区修改（或新增）的文件执行 git add 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
> - 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
> - 当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
> - 当执行 git rm --cached <file> 命令时，会直接从暂存区删除文件，工作区则不做出改变。
> - 当执行 git checkout . 或者 git checkout -- <file> 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区中的改动。
> - 当执行 git checkout HEAD . 或者 git checkout HEAD <file> 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未添加到暂存区中的改动，也会清除暂存区中未提交的改动。

## Git操作
Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。
本节将对有关创建与提交你的项目快照的命令作介绍。
Git 常用的是以下 6 个命令：git clone、git push、git add 、git commit、git checkout、git pull。
![Git常用命令](https://www.runoob.com/wp-content/uploads/2015/02/git-command.jpg)
> workspace：工作区
staging area：暂存区/缓存区
local repository： **版本库或本地仓库**
remote repository：远程仓库

> ```python
> git init //初始化仓库
> git clone //拷贝一份远程仓库，也就是下载一个项目。
> git remote add origin git@github.com:yourName/yourRepo.git //添加远程地址, 后面的yourName和yourRepo表示你在github的用户名和仓库，加完之后进入.git，打开 config，这里会多出一个remote "origin"内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。
> ```

## Git github


create a new repository on the command line
```python
echo "# git-test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Iridescent-zhang/git-test.git
git push -u origin main
```

push an existing repository from the command line
```python
git remote add origin git@github.com:Iridescent-zhang/git-test.git
git branch -M main
git push -u origin main
```



























































[Windows下如何解决git bash的默认home目录路径问题](https://www.cnblogs.com/songzhenhua/p/9312720.html)
[合并冲突](https://blog.csdn.net/nonfuxinyang/article/details/77206486)