---
title: 学习HTML
date: 2022-09-06 20:24:20
categories:
tags:
---

## Learning Record
超文本标记语言HTML is short for "HyperText Markup Language"，是一种用于创建网页的标准标记语言，可以使用HTML来建立自己的Web站点，HTML文档运行在浏览器上，由浏览器解析。
<!--more-->
- HTML 不是一种编程语言，而是一种标记语言;
- 标记语言是一套标记标签 (markup tag);
- HTML 使用标记标签来描述网页;
- HTML文档包括HTML标签和文本内容;
- HTML文档也叫做Web页面;

HTML标签通常成对出现，如\<html>内容\</html>；
HTML元素指的是一对标签和开始、结束标签之间的内容，大多数HTML元素包含属性；
HTML元素可以相互嵌套，HTML文档就是由相互嵌套的元素构成的。

Web浏览器读取HTML文档，并将其作为网页显示，浏览器并不是直接显示的HTML标签，但可以使用标签来决定如何展现HTML页面的内容给用户：

\<a href="https://www.runoob.com">这是一个链接\</a>
\<img src="/images/logo.png" width="258" height="39" />
src、href都是属性

空元素是没有内容的元素，在开始标签中关闭，通过在开始标签中添加斜杠，例如\<br/>是关闭空元素的正确方法，包括img元素也是这样；

### 属性
HTML元素添设置属性来添加附加信息，属性一般描述在开始标签，通常以名称="值"的形式出现（name="value"）。
HTML链接由\<a>定义，链接的地址在href属性中指定：\<a href="http://wwww.runoob.com">这是一个链接\<a>；
属性值始终被包括在引号中。

<a href="https://www.runoob.com/tags/html-reference.html">HTML标签参考手册</a>
<a href="https://www.runoob.com/tags/ref-standardattributes.html">HTML标准属性参考手册</a>

### 标题
搜索引擎使用标题为网页的结构和内容编制索引，用户通过标题快速浏览你的网页，所以别乱用标题。

\<!--添加注释-->
\<br />元素定义换行；
\<hr />元素定义水平线

HTML文档中连续的空格，Tab，换行都识别为一个空白字符，段内换行得用\<br>；

### 头部
头部\<head>元素，在 <head>元素中你可以插入脚本（scripts）, 样式文件（stylesheet，CSS），及各种meta信息,可以添加在头部区域的元素标签为: \<title>, \<style>, \<meta>, \<link>, \<script>, \noscript> 和 \<base>。
- \<title> 标签定义了不同文档的标题。
- \<base> 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接;
- \<link> 标签定义了文档与外部资源之间的关系，通常是链接到样式表。
- \<style> 标签定义了HTML文档的样式文件引用地址，在\<style> 元素中你也可以直接添加样式来渲染HTML文档；
> \<link>和\<style>标签都可以通过链接/引用样式文件(text/css)来对HTML文档进行渲染；
- meta标签描述了一些基本的元数据。
- \<script>标签用于加载脚本文件，如： JavaScript。

### HTML样式CSS
/CSS (Cascading Style Sheets) 用于渲染HTML元素标签的样式。
> CSS 可以通过以下方式添加到HTML中:
> - 内联样式- 在HTML元素中使用"style" 属性
> - 内部样式表- 在HTML文档头部\<head>区域使用\<style>元素来包含CSS
> - 外部引用- 使用link调用外部 CSS 文件（最好的方式）。

>  内联样式：指定文本的样式即指定文本的显示方式，使用内联样式的方法是在相关的标签中使用style属性并赋值，比如背景颜色`style="background-color:yellow;"`，颜色`style="color:blue;"`，字体，字体颜色，字体大小，文本对齐方式;

>  内部样式表：当这个文件需要特别样式时，就可以使用内部样式表。你可以在\<head> 部分通过 \<style>标签定义内部样式表:
>    ```html
>    <head>
>    <style type="text/css">
>    body {background-color:yellow;}
>    p {color:blue;}
>    </style>
>    </head>
>    ```

>  外部样式表：当样式需要被应用到很多页面的时候，外部样式表将是理想的选择。使用外部样式表，你就可以通过更改一个文件来改变整个站点的外观。
>  ```html
>  <head>
>  <link rel="stylesheet" type="text/css" href="mystyle.css">
>  </head>
>  ```
>  rel是relation的意思

### 表格
\<table>	定义表格
\<th>	定义表格的表头，就是表格最左边和上边的一列/排。(table head)
\<tr>	定义表格的行(table row)
\<td>	定义表格单元(table data)

### 列表

### 区块
>HTML 可以通过 \<div> 和 \<span>将元素组合起来。
大多数 HTML 元素被定义为块级元素或内联元素。
块级元素在浏览器显示时，通常会以新行来开始（和结束）。
内联元素在显示时通常不会以新行开始。

HTML \<div> 元素是块级元素，它可用于组合其他 HTML 元素的容器。
\<div> 元素没有特定的含义。除此之外，由于它属于块级元素，浏览器会在其前后显示新行。
HTML \<span> 元素是内联元素，可用作文本的容器，用来组合文档中的内联元素(inline)。


### 表单
> HTML 表单用于收集用户的输入信息。HTML 表单表示文档中的一个区域，此区域包含交互控件，将用户输入的信息发送到 Web 服务器。

### 脚本
JavaScript 使 HTML 页面具有更强的动态和交互性。
看到脚本不看了










### 空元素
meta，img，br，hr，

### 非空元素
html，head，title，body，h123456，p，






















