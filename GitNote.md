---
title: 学习Git
date: 2022-09-19 16:47:52
categories: 
  - Language
tags:
  - Git
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

## Git 分支管理
几乎每一种版本控制系统都以某种形式支持分支，一个分支代表一条独立的开发线。
使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。
![分支](https://static.runoob.com/images/svg/git-brance.svg)

Git 分支实际上是指向更改快照的指针。

有人把 Git 的分支模型称为必杀技特性，而正是因为它，将 Git 从版本控制系统家族里区分出来。
当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

当你执行 git init 的时候，默认情况下 Git 就会为你创建 master 分支。
**当你以此方式在上次提交更新之后创建了新分支，如果后来又有更新提交， 然后又切换到了 testing 分支，Git 将还原你的工作目录到你创建分支时候的样子。**

**分支实在是太妙了，不管你在这个分支做了什么，`git checkout <branchname>`切换分支时（注意，如果你在这个分支做的改变没有commit时是无法切换到另一个已经存在的分支的，但是是可以切换到一个新建分支的(git branch \<branchname> )。），Git 将还原你的工作目录到该分支<branchbname>上一次commit时的状态(对于新分支，则是切换到原来分支创建新分支前一次提交更新时的样子) 也不大对，好像也可以切换到已存在的分支，再说吧**

使用分支将我们自己的工作切分开来，从而让我们能够在不同开发环境中做事，并来回切换。

> **分支命令**
>    ```python
>    git branch //查看已有分支
>    git branch <branchname> //新建分支
>    git checkout <branchname> //切换分支
>    git checkout -b <branchname> //创建新分支并立即切换到该分支下，从而在该分支中操作。
>    git branch -d <branchname> //删除分支
>    git rm <filename> //删除文件
>    git add . //添加当前项目的所有文件
>    echo 'runoob.com' > test.txt //向test.txt中写入runoob.com
>    git branch -M master //删去其他分支，主分支改名为master
>    git merge <branchname> //一旦某分支有了独立内容，你终究会希望将它合并回到你的主分支。 你可以使用该命令将任何分支合并到当前分支中去：
>    合并并不仅仅是简单的文件添加、移除的操作，Git 也会合并修改。
>    git commit -v //查看当前分支是否有change没有被stage和commit
>    git commit -am "备注" //相当于git add <filename>之后git commit -m "备注"，commit all changed files
>    git commit -help //该命令可以查看 commit 的命令选项。
>    git status -s //该命令用于查看项目的当前状态，git commit -v有类似功能。
>    ```
`git status`显示的状态：
??：新添加的文件未添加到index；
A：表示这个文件添加到了缓存区；
AM：该状态的意思是这个文件在我们将它添加到缓存之后又有了改动；
文件修改后，我们一般都需要进行 git add 操作，从而保存历史版本。

>    合并冲突出现时，需要手动去修改它，修改完之后用git add告诉Git文件冲突已经解决
>    用git status -s查看时，冲突文件前显示UU，git add后显示M(modify)
>    最后git commit提交。现在便成功解决了合并中的冲突，并提交了结果。

## Git 查看提交历史
Git 提交历史一般常用两个命令：
 - git log - 查看历史提交记录。
 - git blame <file> - 以列表形式查看指定文件的历史修改记录。

### git log
在使用 Git 提交了若干更新之后，又或者克隆了某个项目，想回顾下提交历史，我们可以使用 git log 命令查看。
可以用 --oneline 选项来查看历史记录的简洁的版本，即`git log --oneline`
还可以用 --graph 选项，查看历史中什么时候出现了分支、合并，即`git log --graph`
也可以用 --reverse 参数来逆向显示所有日志，即`git log --reverse --graph`，这样从上往下是按时间顺序，没有这个参数是逆序。
`git log --oneline --graph`很清楚
### git blame
如果要查看指定文件的修改记录可以使用 git blame 命令，格式如下：
`git blame <file>`
git blame 命令是以列表形式显示修改记录。

## git 标签
如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。
 我们可以用 `git tag -a v1.0` 命令给最新一次提交打上（HEAD）"v1.0"的标签。
-a 选项意为"创建一个带注解的标签"。 不用 -a 选项也可以执行的，但它不会记录这标签是啥时候打的，谁打的。 我推荐一直创建带注解的标签。
当你执行 git tag -a 命令时，Git 会打开你的编辑器，让你写一句标签注解，就像你给提交写注解一样。
现在，注意当我们执行 `git log --decorate` 时，我们可以看到我们的标签了。
`git log --oneline --graph --decorate`好用
如果我们忘了给某个提交打标签，又将它发布了，我们可以给它追加标签。
假设我们发布了提交 85fc7e7 (git log可以看到 ***commit*** 后面跟着很长的一段字符，取前七位)，但是那时候忘了给它打标签。 我们现在也可以：`git tag -a v0.9 85fc7e7`。
如果我们要查看所有标签可以使用以下命令：`git tag`。

将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。
所以，标签也是版本库的一个快照。
Git 的标签虽然是版本库的快照，但其实它就是指向某个 commit 的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的

"请把上周一的那个版本打包发布，版本号是v1.2"
"好的，按照tag v1.2查找commit就行！
所以，tag就是一个让人容易记住的有意义的名字，它跟某个commit(commit号前七位)绑在一起。

 删除标签 git tag -d v1.1
 
 ## Github
> `git init, git add, git commit` 之后
> 添加远程仓库:
> `git remote add origin git@github.com:Iridescent-zhang/blog.git`
> 查看当前的远程库:
> `git remote, git remote -v`
> 删除远程仓库:
> `git remote rm [别名]`
> 1. 从远程仓库下载新分支与数据到本地分支：
> `git fetch`
> 2. 从远端仓库提取数据并尝试合并到当前分支：
> `git merge`
>
> 从远程仓库获取更新也可以用:
> `git pull`
> ![pull equal to <fetch+merge>](https://www.runoob.com/wp-content/uploads/2015/03/main-qimg-00a6b5a8ec82400657444504c4d4d1a7.png)
> 假设你配置好了一个远程仓库[alias]，并且你想要提取更新的数据，你可以首先执行 git fetch [alias] 告诉 Git 去获取它有你没有的数据（假设有人这时候推送到服务器了），然后你可以执行 git merge [alias]/[branch] 以将服务器上的任何更新的分支合并到你的当前分支。
> 
> 推送你的新分支与数据到某个远端仓库命令:
> `git push [alias] [branch]`
> 以上命令将你的本地 [branch] 分支推送成为 [alias] 远程仓库上的 [branch] 分支，所以push时本地分支名和远程分支名得一样。
> 
> **执行 git fetch origin master 时，它的意思是从名为 origin 的远程上拉取名为 master 的分支到本地分支 origin/master 中。既然是拉取代码，当然需要同时指定远程名与分支名，所以分开写。**
> ***执行 git merge origin/master 时，它的意思是合并名为 origin/master 的分支到当前所在分支。既然是分支的合并，当然就与远程名没有直接的关系，所以没有出现远程名。需要指定的是被合并的分支。***
> **执行 git push origin master 时，它的意思是推送本地的 master 分支到远程 origin（成为远程仓库的同名master分支），涉及到远程以及分支，当然也得分开写了。**
> 
 ## END
GitHub 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。
git add -A 或者 git add –all 表示追踪所有操作，包括新增、修改和删除。
添加远程版本库命令格式：git remote add [shortname] [url]，一般shortname用origin，origin2...
git fetch 命令将提交、文件和引用从远程存储库下载到本地存储库中。
**git pull 其实就是 git fetch 和 git merge FETCH_HEAD 的简写。**

---

> 这是在github新建仓库时给出的代码
> create a new repository on the command line
> ```python
>echo "# git-test" >> README.md
>git init
>git add README.md
>git commit -m "first commit"
>git branch -M main
>git remote add origin git@github.com:Iridescent-zhang/git-test.git
>git push -u origin main
>```
>
>push an existing repository from the command line
>```python
>git remote add origin git@github.com:Iridescent-zhang/git-test.git
>git branch -M main
>git push -u origin main
>```

### git rebase（变基）详解
两个分支master和feature，其中feature是在提交点B处从master上拉出的分支
master上有一个新提交M，feature上有两个新提交C和D
![](https://img-blog.csdnimg.cn/36efc2704d174acab598c4b9addd3694.png?)

此时切换到feature分支上，执行如下命令，相当于是想要把master分支合并到feature分支（这一步的场景就可以类比为我们在自己的分支feature上开发了一段时间了，准备从主干master上拉一下最新改动）
```c
git checkout feature
git rebase master

//这两条命令等价于git rebase master feature
```
下图为变基后的提交节点图，解释一下其工作原理：
![](https://img-blog.csdnimg.cn/12b959efcc454da5a15b9fdec493d61b.png?)
> 官方解释（如果觉得看不懂可以直接看下一段）：当执行rebase操作时，git会从两个分支的共同祖先开始提取待变基分支上的修改，然后将待变基分支指向基分支的最新提交，最后将刚才提取的修改应用到基分支的最新提交的后面。
>
> 结合例子解释：当在feature分支上执行git rebase master时，git会从master和featuer的共同祖先B开始提取feature分支上的修改，也就是C和D两个提交，先提取到。然后将feature分支指向master分支的最新提交上，也就是M。最后把提取的C和D接到M后面，但这个过程是删除原来的C和D，生成新的C’和D’，他们的提交内容一样，但commit id不同。feature自然最后也是指向D’。
>
> 通俗解释（重要！！）：rebase，变基，可以直接理解为改变基底。feature分支是基于master分支的B拉出来的分支，feature的基底是B。而master在B之后有新的提交，就相当于此时要用master上新的提交来作为feature分支的新基底。实际操作为把B之后feature的提交存下来，然后删掉原来这些提交，再找到master的最新提交位置，把存下来的提交再接上去（新节点新commit id），如此feature分支的基底就相当于变成了M而不是原来的B了。（注意，如果master上在B以后没有新提交，那么就还是用原来的B作为基，rebase操作相当于无效，此时和git merge就基本没区别了，差异只在于git merge会多一条记录Merge操作的提交记录）
>
>上面的例子可抽象为如下实际工作场景：张三从B拉了代码进行开发，目前提交了两次，开发到D了；李四也从B拉出来开发了并且开发完毕，他提交到了M，然后合到主干上了。此时张三想拉下最新代码，于是他在feature分支上执行了git rebase master，即把master分支给rebase过来，由于李四更早开发完并合了主干，如此就相当于张三是基于李四的最新提交M进行的开发了。

**无论是个人开发，还是公司协作开发，只要没有特殊需求，用merge准没错！！**

---

出现`fatal: refusing to merge unrelated histories`
![](https://i.postimg.cc/nVwtY40H/1.jpg)

[遇到过类似的问题](https://stackoverflow.com/questions/55972713/fail-to-use-git-pull-couldnt-find-remote-ref-allow-unrelated-histories)
当执行git中的“git pull origin master –allow-unrelated-histories”命令时，会出现“ couldn’t find remote ref –allow-unrelated-histories”的错误:

[遇到过类似的问题](https://stackoverflow.com/questions/24357108/updates-were-rejected-because-the-remote-contains-work-that-you-do-not-have-loca)
git pull <remote> master:dev将获取remote/master分支并将其合并到您的local/dev分支中。
git pull <remote> dev将获取remote/dev分支，并将其合并到您当前的分支中。
也可以[rebase](https://blog.csdn.net/brownsville2/article/details/102246961)

--- 
设置GIT根目录，也就是为了设置git配置文件的路径，方便管理；
参考文章[如何修改git的默认路径](https://blog.51cto.com/u_15075507/3641247)

[Windows下如何解决git bash的默认home目录路径问题](https://www.cnblogs.com/songzhenhua/p/9312720.html)
[合并冲突](https://blog.csdn.net/nonfuxinyang/article/details/77206486)
[Runoob-Git](https://www.runoob.com/git/git-tutorial.html)
[pull request](https://chinese.freecodecamp.org/news/how-to-make-your-first-pull-request-on-github/)

[当前文件夹不安全](https://www.aspirantzhang.com/network/git-fatal-unsafe-repository.html)