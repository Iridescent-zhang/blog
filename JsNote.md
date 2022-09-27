---
title: 学习JS
date: 2022-09-18 17:03:19
categories: 
  - JS
tags:
  - Web前端
  - JS
---

JavaScript 是脚本语言
JavaScript 是可插入 HTML 页面的编程代码。
JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。
<!--more-->
在HTML 输出流中使用 document.write，相当于在原有html代码中添加一串html代码。而如果在文档加载后使用（如使用函数，即函数中含有document.write，并且调用这个函数），会覆盖整个文档。

JavaScript 能够对事件作出反应。比如对按钮的点击：onclick事件
在标签中填写 onclick 事件调用函数时，不是 onclick="函数名"， 而是 onclick="函数名+()"

JavaScript：改变 HTML 内容
您会经常看到 document.getElementById("some id")。这个方法是 HTML DOM 中定义的。
DOM (Document Object Model)（文档对象模型）是用于访问 HTML 元素的正式 W3C 标准。
JavaScript 能够改变任意 HTML 元素的大多数属性，而不仅仅是图片元素的src属性。

JavaScript：改变 HTML 元素的样式

HTML 中的 Javascript 脚本代码必须位于 \<script> 与 \</script> 标签之间。
Javascript 脚本代码可被放置在 HTML 页面的 \<body> 和 \<head> 部分中。
浏览器会解释并执行位于 \<script> 和 \</script>之间的 JavaScript 代码 


通常，我们需要在某个事件发生时执行代码，比如当用户点击按钮时。
如果我们把 JavaScript 代码放入函数中，就可以在事件发生时调用该函数。
其他 JavaScript 语句，会在页面加载时执行。

您可以在 HTML 文档中放入不限数量的脚本。
通常的做法是把函数放入 \<head> 部分中，或者放在页面底部。这样就可以把它们安置到同一处位置，不会干扰页面的内容。

也可以把脚本保存到外部文件中。外部文件通常包含被多个网页使用的代码。
外部 JavaScript 文件的文件扩展名是 .js。
如需使用外部文件，请在 \<script> 标签的 "src" 属性中设置该 .js 文件：
` <script src="myScript.js"></script> `
外部 javascript 文件不使用 \<script> 标签，直接写 javascript 代码。

Chrome snippets（代码段） 小脚本

### JavaScript 输出
JavaScript 没有任何打印或者输出的函数。
> JavaScript 显示数据
> - 使用 window.alert() 弹出警告框。
> - 使用 document.write() 方法将内容写到 HTML 文档中。
> - 使用 innerHTML 写入到 HTML 元素。
> - 使用 console.log() 写入到浏览器的控制台。

innerHTML则是DOM页面元素的一个属性，代表该元素的html内容。

如果您的浏览器支持调试，你可以使用 console.log() 方法在浏览器中显示 JavaScript 值。
浏览器中使用 F12 来启用调试模式， 在调试窗口中点击 "Console" 菜单。

当用引号时会认为引号中是字符串，从而直接将引号中的内容打印出来。

JSON(JavaScript Object Notation)是一种轻量级的数据交换格式。
JSON 它其实是来自JavaScript对对象(Object)的定义。但是它作为数据格式来使用的时候，和JavaScript没有任何关系，它只是参照了JavaScript对对象定义的数据格式。
json是一种与语言无关的数据交换的格式，它很易于操作，没有什么复杂的程序，也可以服务于任何语言

### JavaScript 语法
JavaScript 是一个程序语言。语法规则定义了语言结构。
数组（Array）字面量 定义一个数组：
[40, 100, 1, 5, 25, 10]
对象（Object）字面量 定义一个对象：
{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}
函数（Function）字面量 定义一个函数：
function myFunction(a, b) { return a * b;}

JavaScript 使用关键字 var 来定义变量， 使用等号来为变量赋值：
JavaScript是弱类型编程语言,定义变量都使用 var 定义,与 Java 这种强类型语言有区别.
在定义后可以通过 typeOf() 来获取JavaScript中变量的数据类型（但无法区分数组和对象）。

在 HTML 中，JavaScript 语句用于向浏览器发出命令。
语句是用分号分隔：

不是所有的 JavaScript 语句都是"命令"。双斜杠 // 后的内容将会被浏览器忽略：

JavaScript 对大小写是敏感的。

JavaScript 使用 Unicode 字符集。

JavaScript 中，常见的是驼峰法的命名规则，如 lastName (而不是lastname)，getElementById。

var firstName='king';//小驼峰
var FirstName='queen';//大驼峰
var first_name='maizi';//下划线法

### JavaScript 语句






















































































