---
title: cmd
date: 2022-09-20 14:56:32
categories:
  - Shell
tags: 
  - Cmd
---

## 环境变量
环境变量是操作系统用来指定运行环境的一些参数。当你运行某些程序时，除了在当前文件夹中寻找外，还会到这些环境变量中去查找，比如“path”就是一个变量，里面存储了一些常用的命令所存放的目录路径。
<!--more-->
当启动cmd命令行窗口调用某一命令的时候，经常会出现“xxx不是内部或外部命令，也不是可运行的程序或批处理文件”，如果你的拼写没有错误，同时计算机中确实存在这个程序，那么出现这个提示就是你的path变量没有设置正确，因为你的path路径，也就是默认路径里没有你的程序，同时你又没有给出你程序的绝对路径（因为你只是输入了命令或程序的名称而已），这时操作系统不知道去哪儿找你的程序，就会提示这个问题。
比如：当你在path中添加`C:\Program Files (x86)\Notepad++`时，在cmd或powershell终端输入**notepad++**，便会运行该路径下的notepad++.exe程序。

**我做了这样一件事，使我可以在终端输入自定义的命令打开指定的程序。** 首先，我创建了一个文件夹，就我而言：*D:\CmdFiles*，并且我将该文件夹的路径添加到path环境变量中，Windows 现在会在文件夹*D:\CmdFiles*中查找命令，您可以轻松地向该文件夹添加新命令或批处理文件！之后，在此文件夹中，我建立了批处理(.bat)文件，（举个栗子）文件np.bat里面有这样的命令：
```python
@echo off
start "" "C:\Program Files (x86)\Notepad++\notepad++.exe" %*
```
代码注释 [^代码注释]
[^代码注释]:  > @echo off阻止命令打印到命令提示符，引号之间的链接可以引用任何可执行文件；**%\*** 将确保您在np命令之后键入的任何内容 （例如`test.txt` ）都将放在引号中的原始命令之后。`np test.txt`将用notepad++打开test.txt。

也可以创建clash.cmd文件，文件内容如下：
```python
@echo off
"H:\ClashforWindows\Clash.for.Windows-0.15.2-win\Clash for Windows.exe" %*
```
在终端输入clash便可以运行Clash for Windows.exe。
在powershell和cmd中运行结果不同。

## cmd命令
> 1. 查看某个环境变量：输入 "set 变量名"即可，比如想查看path环境变量的值，即输入 set path
> 2. 修改环境变量 ：输入 "set 变量名=变量内容"即可，比如set path="c"，查看path路径的时候，其值为"c:\"，即覆盖以前的内容
> 3. 给变量追加内容：输入“set 变量名=%变量名%;变量内容”。如，为path添加一个新的路径，输入`set PATH=%PATH%;C:\Program Files (x86)\Notepad++`即可将`C:\Program Files (x86)\Notepad++`添加到path中
> **注意所有的在cmd命令行下对环境变量的修改只对当前窗口有效，不是永久性的修改。也就是说当关闭此cmd命令行窗口后，将不再起作用。**
> 在高级系统设置中的环境变量里添加了新路径之后，若你之前打开的应用程序没关掉重启（在你没重启电脑的情况下），那你这个应用程序也可能读取不到该系统变量，所以你关掉重启该应用就好了。如果还是不行，打开cmd，输入命令 `set PATH=c`，这个命令是使你写在path中的变量立即生效，然后重启cmd验证
> 

有几个关于右键打开cmd 终端的网页，以后再看吧。
首先`cmd.exe /s /k pushd "%V"`这是你右键打开cmd的命令。
[cmd.exe /s /k pushd "%V"是什么意思](https://zhidao.baidu.com/question/549082128.html)
[%Userprofile%\ 这个文件在哪里](https://zhidao.baidu.com/question/441785422.html)
[右键打开CMD/命令提示符/命令窗口](http://www.bathome.net/thread-26433-1-1.html)





