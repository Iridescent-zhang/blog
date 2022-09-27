---
title: latexNote
date: 2022-09-23 18:24:56
categories: 
  - LaTeX
tags:
  - LaTeX
---

## 配置vscode texlive SumatraPDF环境
<!--more-->
### 关于 TeX Live
TeX Live 是 TUG (TeX User Group) 维护和发布的 TeX 系统，可说是「官方」的 TeX 系统。我们推荐任何阶段的 TeX 用户，都尽可能使用 TeX Live，以保持在跨操作系统平台、跨用户的一致性。
Texlive 是 LaTex 的编译环境，提供了大量的脚本和宏包供我们使用，并且有很方便的宏包管理器可以下载更新宏包，十分方便。

正向同步: 即从代码定位到编译出来的 pdf 文件相应位置
反向同步: 即从编译出的 pdf 文件指定位置跳转到 tex 文件中相应代码所在位置

> - PDFTeX程序：Tex语言的一个实现，也就是把Tex语言转换为排版的一个程序。它会把TeX 语言写的代码直接编译成 PDF文件。
> - PDFLaTeX命令：PDFTeX程序中的命令，用来编译用LaTeX格式写的tex文件。
> - XeTeX程序：TeX语言的新的实现，即把Tex语言转换为排版的一个新程序。支持Unicode编码和直接访问操作系统字体。
> - XeLaTeX命令：XeTeX程序中的命令，用来编译用LaTeX格式写的tex文件。
> 两者最大的区别是：XeLaTeX对应的XeTeX对字体的支持更好，允许用户使用操作系统字体来代替TeX的标准字体，而且对非拉丁字体的支持更好。

### vscode user setting.json设置latex的代码
```json
    "latex-workshop.latex.autoBuild.run": "never",
    "latex-workshop.showContextMenu": true,
    "latex-workshop.intellisense.package.enabled": true,
    "latex-workshop.message.error.show": false,
    "latex-workshop.message.warning.show": false,
    "latex-workshop.latex.tools": [
        {
        
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
        
            "name": "pdflatex",
            "command": "pdflatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "%DOCFILE%"
            ]
        },
        {
        
            "name": "bibtex",
            "command": "bibtex",
            "args": [
                "%DOCFILE%"
            ]
        }
    ],
    
    "latex-workshop.latex.recipes": [
        {
        
            "name": "xelatex",
            "tools": [
                "xelatex"
            ],
        },
        {
        
            "name": "pdflatex",
            "tools": [
                "pdflatex"
            ]
        },
        {
        
            "name": "xe->bib->xe->xe",
            "tools": [
                "xelatex",
                "bibtex",
                "xelatex",
                "xelatex"
            ]
        },
        {
        
            "name": "pdf->bib->pdf->pdf",
            "tools": [
                "pdflatex",
                "bibtex",
                "pdflatex",
                "pdflatex"
            ]
        }
    ],
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk"
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.latex.recipe.default": "lastUsed",
    "latex-workshop.view.pdf.internal.synctex.keybinding": "double-click",

    //使用 SumatraPDF 预览编译好的PDF文件
    // 设置VScode内部查看生成的pdf文件
    "latex-workshop.view.pdf.viewer": "external",
    // PDF查看器用于在\ref上的[View on PDF]链接
    "latex-workshop.view.pdf.ref.viewer":"auto",
    // 使用外部查看器时要执行的命令。此功能不受官方支持。
    "latex-workshop.view.pdf.external.viewer.command": "C:/Users/ASUS/AppData/Local/SumatraPDF/SumatraPDF.exe", // 注意修改路径
    // 使用外部查看器时，latex-workshop.view.pdf.external.view .command的参数。此功能不受官方支持。%PDF%是用于生成PDF文件的绝对路径的占位符。
    "latex-workshop.view.pdf.external.viewer.args": [
        "%PDF%"
    ],
    // 将synctex转发到外部查看器时要执行的命令。此功能不受官方支持。
    "latex-workshop.view.pdf.external.synctex.command": "C:/Users/ASUS/AppData/Local/SumatraPDF/SumatraPDF.exe", // 注意修改路径
    // latex-workshop.view.pdf.external.synctex的参数。当同步到外部查看器时。%LINE%是行号，%PDF%是生成PDF文件的绝对路径的占位符，%TEX%是触发syncTeX的扩展名为.tex的LaTeX文件路径。
    "latex-workshop.view.pdf.external.synctex.args": [
        "-forward-search",
        "%TEX%",
        "%LINE%",
    //    "-reuse-instance",
    //    "\"H:/Microsoft VS Code/Code.exe\" \"H:/Microsoft VS Code/resources/app/out/cli.js\" -r -g \"%f:%l\"", // 注意修改路径
        "%PDF%"
    ]
```

[vscode SumatraPDF 设置反向搜索](https://zhuanlan.zhihu.com/p/434142338)  
打开SumatraPDF，进入设置->选项对话框，在“设置反向搜索命令行”处（即双击PDF文件时，应运行的命令）填入如下内容：
```python
"H:\Microsoft VS Code\Code.exe" "H:\Microsoft VS Code\resources\app\out\cli.js"  --ms-enable-electron-run-as-node -r -g "%f:%l"
```

### 快捷键
> Ctrl + Alt + B：编译（编译选项与上一次编译相同）
> Ctrl + Alt + R：编译（可选编译链）
> Ctrl + Alt + V：打开 PDF 预览
> Ctrl + Alt + J：定位跳转到光标所在位置对应的 PDF 文件位置

**参考文章**
[Visual Studio Code (vscode)配置LaTeX](https://zhuanlan.zhihu.com/p/166523064)
[使用VSCode编写LaTeX](https://zhuanlan.zhihu.com/p/38178015)
[Visual Studio Code 折腾记：LaTeX 集成编辑环境](https://blog.ceba.tech/2018/11/Visual-Studio-Code-LaTeX/index.html)
 
## LaTex教程
**控制序列**是以反斜杠\开头，以第一个空格或非字母 的字符结束的一串字符，他们并不被输出，但是他们会影响输出文档的效果。比如：
> - `\documentclass{article}`中的`documentclass`就是控制序列，它后面紧跟着的{article}代表这个控制序列的一个必要的参数，这个控制序列的作用，是调用名为 **article** 的**文档类**。
> - `\begin{document}`中的控制序列是begin，`\end{document}` 中的控制序列是end，二者总是成对出现。这两个控制序列以及他们中间的内容被称为**环境**；他们之后的第一个必要参数总是一致的，被称为环境名，比如这里的`document`。
> - 只有在 `document` 环境中的内容，才会被正常输出到文档中去或是作为控制序列对文档产生影响。因此，在`\end{document}`之后插入任何内容都是无效的。
> - \begin{document}与\documentclass{article}之间的部分被称为导言区。导言区中的控制序列，通常会影响到整个输出文档。比如`usepackage{lipsum}`中的控制序列是`usepackage`。
> ```c
> \documentclass{article}
> % 这里是导言区
> \begin{document}
> Hello, world!
> \end{document}
> ```

### 中英混排
现在，XeTeX 原生支持 Unicode，并且可以方便地调用系统字体。可以说解决了困扰中国 TeX 使用者多年的大问题，实现了中文支持。
此外，除去中文支持，中文的版式处理和标点禁则也是不小的挑战。好在由吴凌云和江疆牵头，现在主要由刘海洋和李清维护的 **ctex宏包 / 文档类**一次性解决了这些问题。**ctex宏包和文档类**的优势在于，它适用于多种编译方式；在内部处理好了中文和中文版式的支持，隐藏了这些细节；并且，提供了不少中文用户需要的功能接口。
***CTeX 宏集*** 
虽然它的名字也是「CTeX」，但是 CTeX 宏集和 CTeX 套装(不要安装和使用 CTeX 套装！)是两个不同的东西。CTeX 宏集是集成了中文支持、操作系统判定、字体选择、版式预设为一体的一组宏包和文档类的合集。我们推荐在任何情况下，优先使用 CTeX 宏集处理中文。
**请在任何情况下优先使用 CTeX 宏集在 LaTeX 中处理中文！**
```c
\documentclass[UTF8]{ctexart}
\begin{document}
你好，world!
\end{document}
```
差异：
1. 文档类从 article 变为 ctexart
2. 增加了文档类选项 UTF8

### 组织文章
```c
\documentclass[UTF8]{ctexart}
% 导言区
\title{你好，world!}
\author{Liam}
\date{\today}
\begin{document}
\maketitle
\section{你好中国}
中国在 East Asia.
\subsection{Hello Beijing}
北京是 capital of China.
\subsubsection{Hello Dongcheng District}
\paragraph{Tian'anmen Square}
is in the center of Beijing
\subparagraph{Chairman Mao}
is in the center of 天安门广场。
\subsection{Hello 北京}
\paragraph{北京} is an international city。
\end{document}
```
![对应的结果](https://i.postimg.cc/rsJSnsPW/1.jpg)

**出现的控制序列**
> - 导言区
> - \title{·}
> - \author{·}
> - \date{·}
> - 
> - 环境
> - \maketitle
> - \section{·}
> - \subsection{·}
> - \subsubsection{·}
> - \paragraph{·}
> - \subparagraph{·}
> - 

### 查看当前操作系统的字体
用notepad++打开 C 盘根目录下的 C:\font_zh-cn.txt（UTF-8编码下看到的才正常）。
文件中每行都是一个可用的字体。如：
`C:/WINDOWS/fonts/simsun.ttc: SimSun,宋体:style=Regular,常规`，此处表明该字体有两个表示名：宋体和SimSun

## 使用模板写作
**建议在使用中自己整理几个自己常用的模板**，从网上下载的模板只能提供一个大体的方向，有时也存在版本不同造成的兼容性问题，细节需要自己完善。

- [LaTeXtemplates.com](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.latextemplates.com%2F)网站是非常不错的模板分享网站，收集了包括书信，报告，论文，演示文稿，简历等等模板，整体收集模板质量很不错，非常推荐。
- 在 [LaTeX-examples ](https://github.com/MartinThoma/LaTeX-examples) 有作者收集的非常好的模板收集，也收集了大量的tikz等等例子，可下载，选择自己喜欢的模板使用。
- 在 [国外高校论文模板](http://uk.tug.org/training/thesis/) 有不少收集好的国外高校论文模板。
- [overleaf](https://www.overleaf.com/)也可以找到很多期刊的模板代码，还支持在线编辑，所见即所得。











