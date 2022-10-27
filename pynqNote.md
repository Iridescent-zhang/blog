---
title: pynqNote
date: 2022-10-25 11:15:10
categories:
tags:
---

PYNQ is an open-source project from Xilinx® that makes it easier to use Xilinx platforms.
Using the Python language and libraries, designers can exploit the benefits of programmable logic and microprocessors to build more capable and exciting electronic systems.

<!--more-->

# 启动 PYNQ
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

## PYNQ-Z2 接口
![PYNQ-Z2 接口](https://miro.medium.com/max/720/1*UTsIblZqCsQd8BENvYPNDw.png)

## 关于 Jupyter-Notebook 
从SD中启动实际上是运行了一个利用 ARM 处理器功能的操作系统 (Linux)，Jupyter-Notebook 是在 Linux 中运行的服务器。通过使用PC访问 Jupyter-Notebook 服务器连接到设备，现在可以在浏览器网页上编写代码，代码将在 FPGA Arm Cortex 处理器内部执行，所以基本上你可以在处理器上远程运行命令。

## 本节参考：
[Umer Farooq : A PYNQ-Z2 Guide for Absolute Dummies ](https://blog.umer-farooq.com/a-pynq-z2-guide-for-absolute-dummies-part-i-fun-with-leds-and-switches-47dd76abf9a9)
[csdn PYNQ-Z2快速上手](https://blog.csdn.net/qq_34341423/article/details/102507665)
[video ](https://www.youtube.com/watch?v=RiFbRf6gaK4&t=6s)

# 学习 PYNQ
## 什么是PYNQ
PYNQ是Python On Zynq的缩写，它是一个软件开发框架，指导硬件层、驱动层和应用层之间的接口设计，不是ISE、Vivado、SDSoC这样的IDE工具，更不是Zynq芯片的下一代芯片产品。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk4J6DptOU7hJqfmFm%2FFramework.png?generation=1574912214909447&alt=media)

PYNQ框架的设计初衷是通过高层次的封装，将底层硬件FPGA实现细节与上层应用层的使用脱耦，**对软件开发者来说，PYNQ框架已经提供了完整的访问FPGA资源的library，让上层应用开发者通过Python编程就可以调用FPGA模块**，不需要懂Verilog/VHDL硬件编程就可以享受FPGA可并行计算、接口可方便扩展和可灵活配置带来的诸多好处。

在PYNQ框架下，ARM A9 CPU上运行的软件包括：
- 载有Jupyter Notebooks设计环境的网络服务器
- IPython内核和程序包
- Linux
- FPGA的基本硬件库和API

**PYNQ框架是为软件开发者提供了访问FPGA资源的python接口，Python开发者可以忽略这些实现细节**，通过python即可轻松访问FPGA，动态加载各种预编译好的FPGA应用，像调用函数一样去调用各种通过FPGA加速的应用或者访问连接到FPFA的外设。让软件工程师能轻松享受FPGA并行计算和可灵活配置的诸多好处，并不是说用Python对FPGA进行编程来取代传统的RTL编程方式，注意区别。总而言之，PYNQ只是一个工具而已，一个让软件工程师不需要学习太多硬件知识就能发挥FPGA功能的工具。

## Jupyter Notebook必知必会
**Jupyter是基于网页的用于交互计算的应用程序**，可被应用于全过程计算：开发、文档编写、运行代码和展示结果。Jupyter可以很方便地部署在嵌入式Linux中，作为嵌入式Linux的Web IDE。

Notebook主要由两部分组成，**网页应用和文档部分**。网页应用即基于网页形式的、结合了编写说明文档、数学公式、交互计算和其他富媒体形式的工具。简言之，网页应用是可以实现各种功能的工具。 至于文档，Notebook中所有交互计算、编写说明文档、数学公式、图片以及其他富媒体形式的输入和输出，都是以文档的形式体现的。这些文档是保存为后缀名为.ipynb的JSON格式文件，不仅便于版本控制，也方便与他人共享。此外，文档还可以导出为：HTML、LaTeX、PDF等格式。

Notebook具有下列特点：
- 编程时具有语法高亮、缩进、tab补全的功能。
- 可直接通过浏览器运行代码，同时在代码块下方展示运行结果。
- 以富媒体格式展示计算结果。富媒体格式包括：HTML，LaTeX，PNG，SVG等。
- 对代码编写说明文档或语句时，支持Markdown语法。
- 支持使用LaTeX编写数学性说明。

具体关于 Jupyter Notebook 用户UI界面的介绍可以看[这里](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao/03jupyter-notebook-bi-zhi-bi-hui)。

base文件夹和logictools文件夹与具体的Overlay应用相关（Overlay的概念后面会介绍），common文件夹中的是与具体Overlay不绑定的较为通用的例子。对新手来说，getting_started文件夹提供了了解PYNQ必知必会的几个例子，建议先从这里入手。

## PYNQ Overlay介绍
Overlays，或者硬件库，都是可编程FPGA的设计理念。
通过它们，用户可以把Zynq处理系统（Processing System of the Zynq）上的应用扩展到可编程逻辑层面上。Overlays可以用来加速软件应用或者为特定的应用自定义其硬件平台。

PYNQ提供了一个Python交互界面，允许我们通过处理系统上的Python来控制可编程逻辑里的overlays。FPGA设计是一个非常专业化的任务，这需要专业的硬件工程知识。PYNQ的overlays就是由硬件设计师创建，并且包装成了PYNQ PYTHON API。软件开发者就可以无需重新自己设计overlay，而是直接使用这些写好的overlay来操作特定的硬件模块。这其实和专业软件工程师设计软件库并把它们包装成用户可用的API的道理一样。

一般来讲，我们把那些比较基层的、更像是用来**做参考设计**的overlay称为**base overlay**。它们是已经烧写在SD卡上的。当然了，我们也可以自己安装新的Overlay并在运行中使用他们。

每一个Overlay都会有其对应的.bit文件，比如base_overlay对应的就是base.bit。我们可以用下面的方法加载Overlay。
```python
from pynq import overlay
overlay = Overlay("base.bit")
```

对于我们提到的已有的base overlay，PYNQ很贴心的为我们准备了专门的类，方便我们使用它们。调用代码如下所示：
```python
from pynq.overlays.base import BaseOverlay
base_overlay = BaseOverlay("base.bit")
```
**base_overlay**就是把bit文件转化后的python类。

对于一个python类，我们自然可以使用辅助函数help来查看这个类的具体介绍
```python
help(base_overlay)
```
部分介绍如下：

![](https://i.postimg.cc/XYwpfXpc/1.jpg)

## BaseOverlay介绍
- PYNQ-Z2上的base overlay所涉及的硬件如下：
- HDMI接口（输入输出）
- Audio codec
- 4个绿色LED，2个彩色LED，两个开关，四个按钮
- 两个Pmod PYNQ Microblaze
- Arduino PYNQ Microblaze
- RPI (Raspberry Pi) PYNQ MicroBlaze
- 4个跟踪分析器（PMODA,  PMODB, ARDUINO, RASPBERRYPI）

### HDMI
PYNQ-Z2板上的 HDMI接口 是直接连接到可编程逻辑引脚上的。也就是说在板上没有外用的HDMI电路，HDMI接口是由 HDMI IP 控制生成的。

HDMI IP是链接到处理系统DRAM上的，视频可以以**流**的方式从HDMI输入端进入内存，然后再从HDMI输出端流出。这就允许我们通过python来处理视频数据，或者干脆通过python写一段视频流然后再由HDMI输出。

虽然Jupyter notebook支持内嵌的video形式，但是我们从HDMI捕捉到的视频数据会是原生的，如果不进行适当的编码处理，将无法在notebook上直接播放。

**HDMI IN**：HDMI输入端IP可以捕捉标准的HDMI分辨率。在HDMI资源被连接后，HDMI控制器就会启动，并自动检测输入的数据。分辨率情况可以从Python 的HDMI类里读取，图像数据也可以流入处理系统DRAM。

**HDMI OUT**：数据可以从处理系统DRAM流到HDMI输出端。HDMI输出控制器能通过帧缓冲器来达到视频数据的流畅播放。
HDMI输出端IP支持以下分辨率：
- 640x480
- 800x600
- 1280x720 (720p)
- 1280x1024
- 1920x1080 (1080p)

***HDMI实例：***
源码在板上的getting_started文件夹下base_overlay_video里。
首先，我们得准备一个满足上述条件的输出显示屏，两根HDMI-HDMI线。将HDMI IN端与电脑相连，再将HDMI OUT端与准备的显示屏相连。打开我们的Jupyter Notebook。

可以结合[源码](http://pynq:9090/notebooks/getting_started/5_base_overlay_video.ipynb)和[教程](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao/05baseoverlay-jie-shao#hdmi-out)来学习使用 HDMI。

### IOP-Pmod
Pmod包是一个使用Pmod端口的外部设备的驱动集合。一个Pmod端口是一个12引脚接口，传统的Pmod外接设备包括了各式传感器（光、温度）、交互设备（网口、WIFI、蓝牙）以及输入输出设备（按钮、开关、LED）。
![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45oYcmYScZaIkqVF%2F16.png?generation=1574912160835169&alt=media)
每一个Pmod连接器是由2排6引脚共计12引脚构成的。每一排由3.3V(VCC),接地(GND)以及4个数据引脚构成。如果使用两排的话那就是8个数据引脚。
![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45o_00CI6NFq0CO8%2F17.png?generation=1574912160926567&alt=media)

所有的引脚都在3.3V时正常工作。根据不同设备的不同pull-up/pull-down I/O要求，Pmod数据引脚会有不同的IO标准。（例如IIC需要pull-up，SPI需要pull-down）

引脚0,1和4,5是连接到pull-down电阻器。这个可以支持SPI接口以及大部分外接设备。引脚2,3和6,7是连接到pull-up电阻器。这个可以支持IIC接口。

***略~~***

## Logictools Overlay



































































































































































































































# 本文参考
[官方文档](https://pynq.readthedocs.io/en/latest/)
[PYNQ-Z2 中文资料](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao)