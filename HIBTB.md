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

~~3. **hexo archive**(档案)界面滚动条拉到最下方会出现页面不断抖动的情况~~
>  CSS 控制Html页面高度导致抖动，这类由高度导致页面抖动的问题，其实究其根本原因是滚动条是否显示导致的。在主题下source\css\_common\scaffolding找到base.styl，添加以下代码:

```css
html,body{ overflow-y:scroll;}
html,body{ overflow:scroll; min-height:101%;}
html{ overflow:-moz-scrollbars-vertical;}
```
> ***成小丑了，被这点坑死了，导致返回顶部按钮和目录都失效了，现在看来页面抖动应该是网络的原因***

3. 返回顶部按钮
使用插件[hexo-cake-moon-menu](https://github.com/jiangtj-lab/hexo-cake-moon-menu)
在`G:\blog\themes\next\package.json`可以查看 hexo next 的版本
```json
"name": "hexo-theme-next",
"version": "7.8.0",
```
使用`npm i hexo-cake-moon-menu@2.1.2`安装version 2.1.2
在主题的_config.yml中添加：
```c
moon_menu:
  back2top:
    enable: true
    icon: fas fa-chevron-up
    order: -1
  back2bottom:
    enable: true
    icon: fas fa-chevron-down
    order: -2
```
这个返回顶部按钮并不会和 next 自带的返回顶部按钮和阅读百分比冲突，也就是可以同时开启，不过我还是关掉了。

4. 设置网页加载进度条
参考自[加载进度条](https://blog.csdn.net/TomAndersen/article/details/104693194?spm=1001.2014.3001.5506)。
这个进度条能够显示网页加载进度，非常好用，对我来说能调高网页浏览体验。
![记载进度条](https://i.postimg.cc/JnWZJsj6/2.jpg)

    - 进入NexT主题文件夹`themes/next/`
    - （如果使用CDN可跳过此步骤）克隆Github仓库至`themes/next/source/lib`路径下:
    `git clone https://github.com/theme-next/theme-next-pace source/lib/pace`
    - 配置NexT中的_config.xml，寻找pace选项并开启
      ```python
      # Progress bar in the top during page loading.
      # 设置页面加载时顶部进度条
      # Dependencies: https://github.com/theme-next/theme-next-pace
      # For more information: https://github.com/HubSpot/pace
      pace:
        # enable: false
        enable: true
        # Themes list:
        # big-counter | bounce | barber-shop | center-atom | center-circle | center-radar | center-simple
        # corner-indicator | fill-left | flat-top | flash | loading-bar | mac-osx | material | minimal
        theme: minimal
      ```
    - 在NexT主题的_config.xml文件中找到vendors选项，设置pace的cdn地址（本人设置的进度条为黑色主题，可以在[jsdelivr](https://www.jsdelivr.com/package/npm/pace-js?path=themes)中找到对应的样式最新版cdn地址）
      ```c
      vendors:
        ...
      pace: https://cdn.jsdelivr.net/npm/pace-js@1.2.4/pace.min.js
      pace_css: https://cdn.jsdelivr.net/npm/pace-js@1.2.4/themes/orange/pace-theme-loading-bar.css
      ```
    **CDN**的全称是Content Delivery Network，即内容分发网络。CDN是构建在现有网络基础之上的智能虚拟网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。由于CDN是为加快网络访问速度而被优化的网络覆盖层，因此被形象地称为“网络加速器”。

### 配置hexo next详细教程
[主要参考](https://minyuchengmin.github.io/2020/02/26/hexo-bo-ke-xin-ban-next-zhu-ti-da-jian/#valine-comments)
[含个人域名重定向和HTTPS](https://blog.shijy16.cn/2021/05/13/%E9%85%8D%E7%BD%AE/Hexo%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA/)

### something else
[unsplash](https://unsplash.com/)*里的图片非常清爽，并且提供api服务，从中挑选博客背景图非常合适。*