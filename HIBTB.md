---
title: 搭建hexo博客
date: 2022-09-06 17:18:48
categories:
  - blog
tags:
  - hexo
  - next
---
## How I bulid this blog
**hexo**是一个应用非常广泛的，快速且高效的博客框架，我使用的主题是[next](https://github.com/theme-next/hexo-theme-next)，网上配置教程非常多，过程繁琐但不难，需要注意的一点是大多数教程是旧版*next*（仍作参考），新版*next*有一些不同的地方，修改除_config.yml以外的文件时做好备份。

本文不是详细的hexo next配置教程，仅仅记录配置过程遇到的问题，详细配置教程见文末链接。
<!--more-->

### some problems
1. **hexo d**失败，*fatal：Failed to connect to github.com port 443: Timed out*

> Git设置全局代理
1080是代理端口，看代理软件可查，根据自己的实际修改。

```python
git config --global http.proxy 127.0.0.1:1080
git config --global https.proxy 127.0.0.1:1080
```
2. **hexo s**在本地服务器显示正常，**hexo d**发布结果与本地服务器不同
> 解决方法
> - 使用hexo clean清除缓存，之后hexo g, hexo d重新部署；
> - 浏览器按F12，看哪些css, js文件没有加载上，进行调试修改；
> - 清除浏览器缓存

3. **hexo archive**(档案)界面滚动条拉到最下方会出现页面不断抖动的情况
>  CSS 控制Html页面高度导致抖动，这类由高度导致页面抖动的问题，其实究其根本原因是滚动条是否显示导致的。在主题下source\css\_common\scaffolding找到base.styl，添加以下代码:

```css
html,body{ overflow-y:scroll;}
html,body{ overflow:scroll; min-height:101%;}
html{ overflow:-moz-scrollbars-vertical;}
```

### 配置hexo next详细教程
[主要参考](https://minyuchengmin.github.io/2020/02/26/hexo-bo-ke-xin-ban-next-zhu-ti-da-jian/#valine-comments)
[含个人域名重定向和HTTPS](https://blog.shijy16.cn/2021/05/13/%E9%85%8D%E7%BD%AE/Hexo%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA/)

### something else
[unsplash](https://unsplash.com/)*里的图片非常清爽，并且提供api服务，从中挑选博客背景图非常合适。*