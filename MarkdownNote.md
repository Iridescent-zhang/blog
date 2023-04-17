---
title: 学习Markdown
date: 2022-09-06 16:37:25
categories:
  - Language
tags:
  - Markdown
---

## 基本语法
这些是 John Gruber 的原始设计文档中列出的元素，所有 Markdown 应用程序都支持。

<!--more-->

![basic](https://img-blog.csdnimg.cn/2021080722171071.png)

### 基本语法细节
1. 细节1
    ![](https://i.postimg.cc/hvnhM3k2/1.jpg)
    这样用的时候在`Get-History`和`>`中间需要有个空格，才能达到这样的效果：
    >```python
    > Get-History
    >```

2. 细节2
    如果整个代码块想要作为列表元素的一部分，则需要四个空格。如：
    ![](https://i.postimg.cc/yxTxGyBs/2.jpg)
    > 1. 举个栗子
    >    ```python
    >    Get-History
    >    ```


## 扩展语法
并非所有 Markdown 应用程序都支持这些元素。
![extend](https://img-blog.csdnimg.cn/20210807221747578.png)



### 扩展语法细节
1. 脚注第二行的[^1]后面一定要跟英文冒号: 
2. 列表嵌套只需在子列表中的选项前面添加两个或四个空格即可

`<!--more-->`: 隐藏博客首页文章大部分内容。



### VSCode Markdown ALL in One使用

Markdown ALL in One插件支持很多快捷键，在vscode中可以使用CTRL+SHIFT+P之后输出markdown选择要使用的命令。


### VS Code主题

[18个VS Code主题推荐](https://cloud.tencent.com/developer/article/2122340)



### VS Code 配置Markdown

[将 VS Code 打造成一个体验舒适的 Markdown 编辑器](https://blog.cxplay.org/works/vscode-to-markdown-editor/)

注意：在setting.json中设置markdown的字体是影响VS Code代码编辑区部分，也就是Markdown源码。要修改渲染后的Markdown字体要在Markdown Preview Enhanced插件的style.less中进行修改，打开命令面板，输入Customize CSS，便打开 style.less 文件

## 文档参考
[Markdown指南](https://www.markdown.xyz/)
[RUNOOB](https://www.runoob.com/markdown/md-tutorial.html)