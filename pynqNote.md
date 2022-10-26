---
title: pynqNote
date: 2022-10-25 11:15:10
categories:
tags:
---

PYNQ is an open-source project from Xilinx® that makes it easier to use Xilinx platforms.
Using the Python language and libraries, designers can exploit the benefits of programmable logic and microprocessors to build more capable and exciting electronic systems.

<!--more-->

## 启动 PYNQ
1. 在 MicroSD 卡中烧录 pynq 的镜像文件；
    在 http://www.pynq.io/board.html 下载镜像文件，用 WIN32 Disk Imager 将镜像刻录到 SD 卡中。
2. 设置电路板
    根据你的选择（板子供电方式，启动方式）来设置电路板，将跳线放置到相应的位置，连接 microUSB 电缆 和 以太网 电缆。板子上电一段时间后，正常情况下会看到这样的现象，DONE LED 应打开，随后 2 个蓝色 LED 和 4 个黄色用户 LED 闪烁。蓝色 LED 将闪烁一段时间，然后将关闭，而黄色 LED 将保持点亮状态，这表明该板已准备好使用。
3. 以太网连接
    - 有联网的路由器：如果你的身边有路由器，则可以将 pynq 的以太网接口和路由器的LAN口相连，板子的IP地址将由DHCP服务器自动配置，pynq能从internet下载数据包。电脑浏览器地址栏中输入 http://pynq:9090/ 将能够访问板载Linux上运行的 Jupyter Notebook Web服务器的主页。想要获得板子的IP地址以便和板子进行通信可以通过PuTTY串口通讯软件，也可以使用名为“Angry IP Scanner”的软件，可以从 https://angryip.org/download 获得。它通常会自动检你的网络配置并配置 IP 范围，只需要按开始按钮即可。例如我的主机在无线局域网中的IP地址为`192.168.31.29`，运行Angry IP Scanner如下：
    ![](https://i.postimg.cc/1t92ZjMm/5.jpg)
    ![](https://i.postimg.cc/7Z0WJkWP/dfs.jpg)
    于是我在地址栏中输入 pynq 的IP地址`192.168.31.166`，能看到和输入 http://pynq:9090/ 一样的效果，输入密码 `xilinx` 就能进入jupyter的网页。
    - 无联网的路由器：你可以将 pynq 的以太网接口和 PC 的以太网接口相连，这种情况下，你需要设置 PC 的以太网静态 IP 地址，开发板的默认静态 IP 地址为 192.168.2.99，你需要将 PC 的 IP 地址配置在相同的网段内，这样二者就可以进行通信了，例如：IP地址配置为 192.168.2.1，子网掩码配置为 255.255.255.0。（打开网络与共享中心–>更改适配器设置–>选择以太网右击属性–>选中Internet协议版本4–>选中属性–>选中‘使用下面的IP地址’选项–>输入对应数），你可以使用命令`ping 192.168.2.99`查看是否连接成功。连接成功后在浏览器地址栏输入 http://192.168.2.99:9090/ 就可以打开嵌入式Web服务器Jupyter Notebook的页面。这里 pynq 是无法从internet进行下载更新的，但是，如果你的电脑是通过 WIFI 连接互联网的，可以将网络共享给PYNQ-Z2板卡，可以参考[这篇文章](https://blog.csdn.net/qq_43588553/article/details/113699594)。
4. 通过samba传输文件
    在开发过程中，如果需要在PC机与板卡之间传输一些较大的文件，可以通过PYNQ支持的samba协议将PYNQ的文件系统当作一个网络硬盘直接读取。在Windows中只需要打开资源管理器，输入`\\pynq\xilinx`即可成功连接。
    ![](https://i.postimg.cc/MTHF3jdt/3.jpg)
    其中文件夹`jupyter_notebooks`内容与 Jupyter Notebook 服务器主页显示的一致。

### PYNQ-Z2 接口
![PYNQ-Z2 接口](https://miro.medium.com/max/720/1*UTsIblZqCsQd8BENvYPNDw.png)

### 关于 Jupyter-Notebook 
从SD中启动实际上是运行了一个利用 ARM 处理器功能的操作系统 (Linux)，Jupyter-Notebook 是在 Linux 中运行的服务器。通过使用PC访问 Jupyter-Notebook 服务器连接到设备，现在可以在浏览器网页上编写代码，代码将在 FPGA Arm Cortex 处理器内部执行，所以基本上你可以在处理器上远程运行命令。

### 本节参考：
[Umer Farooq : A PYNQ-Z2 Guide for Absolute Dummies ](https://blog.umer-farooq.com/a-pynq-z2-guide-for-absolute-dummies-part-i-fun-with-leds-and-switches-47dd76abf9a9)
[csdn PYNQ-Z2快速上手](https://blog.csdn.net/qq_34341423/article/details/102507665)
[video ](https://www.youtube.com/watch?v=RiFbRf6gaK4&t=6s)

# 学习 PYNQ












## 参考
[官方文档](https://pynq.readthedocs.io/en/latest/)
[PYNQ-Z2 中文资料](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao)