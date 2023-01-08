---
title: 学习Markdown
date: 2022-09-06 16:37:25
categories:
  - Programing
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

## 文档参考
[Markdown指南](https://www.markdown.xyz/)
[RUNOOB](https://www.runoob.com/markdown/md-tutorial.html)