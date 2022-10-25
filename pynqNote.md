---
title: pynqNote
date: 2022-10-25 11:15:10
categories:
tags:
---

PYNQ is an open-source project from Xilinx® that makes it easier to use Xilinx platforms.
Using the Python language and libraries, designers can exploit the benefits of programmable logic and microprocessors to build more capable and exciting electronic systems.

<!--more-->

## PYNQ-Z2 接口
![PYNQ-Z2 接口](https://miro.medium.com/max/720/1*UTsIblZqCsQd8BENvYPNDw.png)

## 启动 pynq
1. 在 MicroSD 卡中烧录 pynq 的镜像文件；
    在 http://www.pynq.io/board.html 下载镜像文件，用 WIN32 Disk Imager 将镜像刻录到 SD 卡中。
2. 设置电路板
    根据你的选择（板子供电方式，启动方式）来设置电路板，将跳线放置到相应的位置，连接 microUSB 电缆 和 以太网 电缆。板子上电一段时间后，正常情况下会看到这样的现象，DONE LED 应打开，随后 2 个蓝色 LED 和 4 个黄色用户 LED 闪烁。蓝色 LED 将闪烁一段时间，然后将关闭，而黄色 LED 将保持点亮状态，这表明该板已准备好使用。
3. 以太网连接
    - 联网状态：如果你的身边有路由器，则可以将 pynq 的以太网接口和路由器的LAN口相连，板子的IP地址将由DHCP服务器自动配置，pynq能从internet下载数据包。电脑浏览器地址栏中输入 http://pynq:9090/ 将能够访问运行在 pynq arm处理器上的 jupyter notepad 服务器。想要获得板子的IP地址以便和板子进行通信可以通过PuTTY串口通讯软件，也可以使用名为“Angry IP Scanner”的软件，可以从 https://angryip.org/dow nload 获得。它通常会自动检测您的网络配置并配置 IP 范围，只需要按开始按钮即可。例如我的主机在无线局域网中的IP地址为`192.168.31.29`，运行Angry IP Scanner如下：

    于是我在地址栏中输入FPGA的IP地址`192.168.31.166`，能看到和输入 http://pynq:9090/ 一样的效果，输入密码 `xilinx` 就能进入jupyter的网页。
    - 无法联网









