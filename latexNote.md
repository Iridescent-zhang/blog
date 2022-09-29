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
所谓 TeX 发行，也叫 TeX 发行版、**TeX 系统**或者 TeX 套装，指的是包括 **TeX 系统的各种可执行程序，以及他们执行时需要的一些辅助程序和宏包文档** 的集合。

TeX Live 是 TUG (TeX User Group) 维护和发布的 TeX 系统，可说是「官方」的 TeX 系统。我们推荐任何阶段的 TeX 用户，都尽可能使用 TeXLive，以保持在跨操作系统平台、跨用户的一致性。
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
`\documentclass{article}`中的`documentclass`就是控制序列，它后面紧跟着的{article}代表这个控制序列的一个必要的参数，这个控制序列的作用，是调用名为 **article** 的**文档类**。
> 部分控制序列还有被方括号 [ ] 包括的可选参数。
> 所谓文档类，即是 TeX 系统预设的（或是用户自定的）一些格式的集合。不同的文档类在输出效果上会有差别。

`\begin{document}`中的控制序列是begin，`\end{document}` 中的控制序列是end，二者总是成对出现。这两个控制序列以及他们中间的内容被称为**环境**；他们之后的第一个必要参数总是一致的，被称为环境名，比如这里的`document`。
> 只有在 `document` 环境中的内容，才会被正常输出到文档中去或是作为控制序列对文档产生影响。因此，在`\end{document}`之后插入任何内容都是无效的。

\begin{document}与\documentclass{article}之间的部分被称为导言区。导言区中的控制序列，通常会影响到整个输出文档。比如`usepackage{lipsum}`中的控制序列是`usepackage`。你可以将导言区理解为是对整篇文档进行设置的区域——在导言区出现的控制序列，往往会影响整篇文档的格式。
> 比如，我们通常在导言区设置页面大小、页眉页脚样式、章节标题样式等等。
> ```c
> \documentclass{article}
> % 这里是导言区
> \begin{document}
> Hello, world!
> \end{document}
> ```

### 中英混排
在现在，一切教你使用 CJK 宏包的模板、人、网页、书，都是糟糕的、有害的、恼人的、邪恶的和应该摒弃的。

现在，XeTeX 原生支持 Unicode，并且可以方便地调用系统字体。可以说解决了困扰中国 TeX 使用者多年的大问题，实现了中文支持。
此外，除去中文支持，中文的版式处理和标点禁则也是不小的挑战。好在由吴凌云和江疆牵头，现在主要由刘海洋和李清维护的 **ctex宏包 / 文档类**一次性解决了这些问题。**ctex宏包和文档类**的优势在于，它适用于多种编译方式；在内部处理好了中文和中文版式的支持，隐藏了这些细节；并且，提供了不少中文用户需要的功能接口。

***CTeX 宏集*** 
虽然它的名字也是「CTeX」，但是 CTeX 宏集和 CTeX 套装是两个不同的东西(CTeX 宏集本质是 LaTeX 宏的集合，包含若干文档类（.cls 文件）和宏包（.sty 文件）。CTeX 套装是一个**过时的 TeX 系统**。不要安装和使用 CTeX 套装！)。CTeX 宏集是集成了中文支持、操作系统判定、字体选择、版式预设为一体的一组 ***宏包和文档类*** 的合集。我们推荐在任何情况下，优先使用 CTeX 宏集处理中文。

所谓宏包，就是一系列控制序列的合集。这些控制序列太常用，以至于人们会觉得每次将他们写在导言区太过繁琐，于是将他们打包放在同一个文件中，成为所谓的宏包（台湾方面称之为「巨集套件」）。`\usepackage{}` 可以用来调用宏包。

新版 CTeX 宏集的默认能够自动检测用户的操作系统，并为之配置合适的字库。对于 Windows 用户、Mac OS X 用户和 Linux 用户，都无需做任何配置，就能使用 CTeX 宏集来排版中文。
**请在任何情况下优先使用 CTeX 宏集在 LaTeX 中处理中文！**
```c
\documentclass[UTF8]{ctexart}
\begin{document}
你好，world!
\end{document}
```
差异：
1. 文档类从 article 变为 ctexart；
2. 增加了文档类选项 UTF8， 将文档以 UTF-8 编码保存；

你也可以直接使用 xeCJK 宏包来支持中英文混排，不过大多数情况是不推荐这样做的。
```c
\documentclass{article}
\usepackage{xeCJK} %调用 xeCJK 宏包
\setCJKmainfont{SimSun} % 定义在"xeCJK"宏包中的控制序列，设置CJK主字体为 SimSun （宋体）
\begin{document}
你好，world!
\end{document}
```


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
> - \title{·}   % 定义标题
> - \author{·}  % 定义作者
> - \date{·}  % 定义日期
> - 
> - 环境
> - \maketitle   % 这个控制序列能将在导言区中定义的标题、作者、日期按照预定的格式展现出来。
> **在文档类 article/ctexart 中，定义了五个控制序列来调整行文组织结构。**
> - \section{·}
> - \subsection{·}
> - \subsubsection{·}
> - \paragraph{·}
> - \subparagraph{·}
> **在report/ctexrep中，还有\chapter{·}；在文档类book/ctexbook中，还定义了\part{·}。**

### 插入目录
找到 \maketitle，在它的下面插入控制序列 \tableofcontents，保存并用 XeLaTeX 编译两次，观察效果：
![插入目录](https://i.postimg.cc/q7t747Ny/1.jpg)
请注意，LaTeX 将一个换行当做是一个简单的空格来处理，如果输出文档需要换行另起一段，则需要用两个换行（一个空行）来实现。

### 查看当前操作系统的字体
用notepad++打开 C 盘根目录下的 C:\font_zh-cn.txt（UTF-8编码下看到的才正常）。
文件中每行都是一个可用的字体。如：
`C:/WINDOWS/fonts/simsun.ttc: SimSun,宋体:style=Regular,常规`，此处表明该字体有两个表示名：宋体和SimSun

## 写作技巧
### 使用模板写作
**建议在使用中自己整理几个自己常用的模板**，从网上下载的模板只能提供一个大体的方向，有时也存在版本不同造成的兼容性问题，细节需要自己完善。

- [LaTeXtemplates.com](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.latextemplates.com%2F)网站是非常不错的模板分享网站，收集了包括书信，报告，论文，演示文稿，简历等等模板，整体收集模板质量很不错，非常推荐。
- 在 [LaTeX-examples ](https://github.com/MartinThoma/LaTeX-examples) 有作者收集的非常好的模板收集，也收集了大量的tikz等等例子，可下载，选择自己喜欢的模板使用。
- 在 [国外高校论文模板](http://uk.tug.org/training/thesis/) 有不少收集好的国外高校论文模板。
- [overleaf](https://www.overleaf.com/)也可以找到很多期刊的模板代码，还支持在线编辑，所见即所得。

### 文献引用
```c
\documentclass{ctexart}

\bibliographystyle{plain}   %引用的样式%
\begin{document}
    这是一个参考文献引用：\cite{graves1989practical} %大括号内为相应文献的引用标签
    \bibliography{text}     %导入参考文献库文件%
\end{document}
```
> 需要创建参考文献文件text.bib，获取参考文件信息可以自己手动整理，也可以在谷歌学术输入文献名，点击引用，并选择以BibTeX格式引用，如果出现 `403. That’s an error.
Your client does not have permission to get URL /scholar.bib?`，可能是由于IP被谷歌拉入了黑名单。可以尝试换ip地址或者使用IPv6地址（一般被拉黑的是IPv4地址），我在代理中使用IPv6地址便可获得引用信息。
```c
@article{ferrari2006raman,   %此处为引用标签。 这是注释，真正使用时不能写注释
  title={Raman spectrum of graphene and graphene layers},
  author={Ferrari, Andrea C and Meyer, JC and Scardaci, V and Casiraghi, C and Lazzeri, Michele and Mauri, Francesco and Piscanec, S and Jiang, Da and Novoselov, KS and Roth, S and others},
  journal={Physical review letters},
  volume={97},
  number={18},
  pages={187401},
  year={2006},
  publisher={APS}
}
```

### 数学公式
为了使用 AMS-LaTeX 提供的数学功能，我们需要在导言区加载amsmath宏包：
`\usepackage{amsmath}`
LaTeX 的数学模式有两种：行内模式 (inline) 和行间模式 (display)。前者在正文的行文中，插入数学公式；后者独立排列单独成行并且居中。
在行文中，使用\$ ... $可以插入行内公式，使用\\[ ... \\]可以插入行间公式，如果需要对行间公式进行编号，可以使用equation环境： 
```c
\begin{equation}
...
\end{equation}
```
>```c
>$E=mc^2$.
>\[ E=mc^2. \]
>\begin{equation}
>E=mc^2.
>\end{equation}
>```
>这里提一下关于公式标点使用的规范。行内公式和行间公式对标点的要求是不同的：行内公式的标点，应该放在数学模式的限定符之外，而行间公式则应该放在数学模式限定符之内。


建议 LaTeX 用户应当尽可能避免使用辅助工具输入数学公式。但对于急用的初学者而言，适当地使用辅助工具（而不形成依赖）也是有一些收益的。
- https://mathpix.com/ 能够通过热键呼出截屏，而后将截屏中的公式转换成 LaTeX 数学公式的代码。

- [在线LaTeX公式编辑器](https://www.codecogs.com/latex/eqneditor.php)，非常方便直观。

- http://detexify.kirelabs.org/classify.html 允许用户用鼠标在输入区**绘制单个数学符号的样式**，系统会根据样式返回对应的 LaTeX 代码（和所需的宏包）。这在**查询**不熟悉的数学符号时特别有用。

### 表格
表格也有类似的工具：[Creat LaTeX tables online](https://www.tablesgenerator.com/)

### 图片
在 LaTeX 中插入图片，有很多种方式。最好用的应当属利用graphicx宏包提供的\includegraphics命令。比如你在你的 TeX 源文件同目录下，有名为 a.jpg 的图片，你可以用这样的方式将它插入到输出文档中：

```c++
\documentclass{article}
\usepackage{graphicx}
\begin{document}
\includegraphics{a.jpg}
\end{document}
```
图片可能很大，超过了输出文件的纸张大小，或者干脆就是你自己觉得输出的效果不爽。这时候你可以用`\includegraphics`控制序列的可选参数来控制。比如
`\includegraphics[width = .8\textwidth]{a.jpg}`
这样图片的宽度会被缩放至页面宽度的百分之八十，图片的总高度会按比例缩放。

### 幻灯片
LaTeX的确还可以制作精美的幻灯片pdf，不过具体使用方法与论文写作大同小异，网上也有很多漂亮的模板。

*日常写作用轻量级的Markdown，想要获得更为复杂和严谨的论文排版作品，上LaTeX，这样基本就能涵盖所有的写作场景，告别臃肿难用的word软件，让我们更专注于内容，享受其中。*

**参考文章**
[LaTeX零基础入门教程](https://www.jianshu.com/p/3e842d67ada2)
[一份其实很短的 LaTeX 入门文档](一份其实很短的 LaTeX 入门文档)

