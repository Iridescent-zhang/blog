---
title: pynqNote
date: 2022-10-25 11:15:10
categories:
  - FPGA
tags:
  - PYNQ
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
![PYNQ-Z2 接口](https://i.postimg.cc/25FxDQYH/PYNQ-Z2-PA-v2-pp-20201209-STD.jpg)

## 关于 Jupyter-Notebook 
从SD中启动实际上是运行了一个利用 ARM 处理器功能的操作系统 (Linux)，Jupyter-Notebook 是在 Linux 中运行的服务器。通过使用PC访问 Jupyter-Notebook 服务器连接到设备，现在可以在浏览器网页上编写代码，代码将在 FPGA Arm Cortex 处理器内部执行，所以基本上你可以在处理器上远程运行命令。

## 本节参考：
[Umer Farooq : A PYNQ-Z2 Guide for Absolute Dummies ](https://blog.umer-farooq.com/a-pynq-z2-guide-for-absolute-dummies-part-i-fun-with-leds-and-switches-47dd76abf9a9)
[csdn PYNQ-Z2快速上手](https://blog.csdn.net/qq_34341423/article/details/102507665)
[video ](https://www.youtube.com/watch?v=RiFbRf6gaK4&t=6s)

# 学习 PYNQ
**写在前面：** 最好的老师就是 `/home/xilinx/jupyter_notebooks` 里的**官方例程**
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
Logictools overlay 包含了可编程逻辑硬件区块来与外部数字逻辑电路连接。Python可以做出有限状态机（Finite State Machine）、布尔型逻辑函数和数字模式。一个可编程 switch 连接了硬件区和外部IO引脚之间的输入和输出。Logictools overlay也可以通过追踪分析器（trace analyzer）来捕捉IO接口传来的数据，方便我们分析调试。
![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45k0Rmf1MxjzmVqs%2F01.png?generation=1574912184317900&alt=media)
Logictools IP包含了4个主要硬件区块
- 布尔型生成器
- 跟踪分析器
- 模式生成器
- FSM生成器（有限状态机生成器）

每一个区块不需要汇编配置文件，这意味着一个配置可以直接加载到生成器里并立即执行。

PYNQ-Z2 logictools overlay有两个logictools逻辑控制处理器（LCP），一个与Arduino header连接，一个与RPI header连接。
Arduino header有20个引脚，RPI有26个引脚，他们可以用作为LCP的GPIO。

板上的4个LED和4个按钮可以连接到任意一个LCP上，使得扩展输入成为了可能。注意！LED和按钮是共享的，在一个时刻只能被一个LCP使用。
![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45k2w-Kfk__l6Lk7%2F02.png?generation=1574912184236329&alt=media)

**以下三个例程都在** [/home/xilinx/jupyter_notebooks/logictools](http://pynq:9090/tree/logictools)

### 布尔型生成器
与BaseOverlay不一样，我们要用LogicToolsOverlay来导入对应的logictools.bit。所谓布尔型生成器，就是用与、或、异或、非来构成最终的布尔型输出。LD是板上的LED，PB是板上的按钮，进行布尔运算之后将结果1/0赋值给LD进行输出。从bit文件转换出的logictools_olay类里，我们可以找到布尔型生成器（来自 pynq.lib.logictools 的 boolean_generator），用其初始化一个布尔型生成器出来，并用对应的表达式（存储在列表或字典中）配置该生成器，随后用run来运行。
```python
from pynq.overlays.logictools import LogicToolsOverlay
from pynq.lib.logictools import BooleanGenerator

logictools_olay = LogicToolsOverlay('logictools.bit')
boolean_generator = logictools_olay.boolean_generator

function = {'XOR_gate':'LD2 = PB3^PB0', 'AND':'LD1 = PB1&PB0'}

boolean_generator.setup(function)
boolean_generator.run()

boolean_generator.stop() #调用stop函数即可停止生成器运作。
```

## 模式生成器 
这里我们用python代码来模拟电路，并用追踪生成器捕捉到的波形来验证我们的结果，展示如何操作模式生成器的单步运行模式。

首先，我们导入logictools overlay，并通过代码形式模拟波形。波形的构造满足一定格式。用{‘signal’:[]}来表明输入的信号波形。在[]内，我们逐一添加波形信息。格式为：{‘name’:’’, ’pin’:’’, ‘wave’:’lh.’}其中l代表low波，h表示high波，‘.’表示重复前面波形。

接下来，我们按照上图代码为我们的模拟内容增加一点东西。外层的{‘signal’:[]}框架不变，在[]里，我们将之前的4个模拟波形信号用列表的方式打包，并命名为‘stimulus’（列表的第一个元素为名称），以同样的格式增加一栏‘analysis’。

```python
from pynq.overlays.logictools import LogicToolsOverlay
from pynq.lib.logictools import Waveform

logictools_olay = LogicToolsOverlay('logictools.bit')

up_counter = {'signal' : [
    ['stimulus',
        {'name':'bit0','pin':'D0','wave':'lh'*8 },
        {'name':'bit1','pin':'D1','wave':'lh.'*4 },
        {'name':'bit2','pin':'D2','wave':'l...h...'*2 },
        {'name':'bit3','pin':'D3','wave':'l.......h.......' }
        ],
    ['analysis',
        {'name':'bit0_output','pin':'D0'},
        {'name':'bit1_output','pin':'D1'},
        {'name':'bit2_output','pin':'D2'},
        {'name':'bit3_output','pin':'D3'}
        ]
] }

#显示激励和跟踪信号的波形
waveform = Waveform(up_counter)
waveform.display() #Waveform函数只是一个把代码转换成模拟波形并输出的函数而已，不具备追踪功能。

# 配置模式生成器
pattern_generator = logictools_olay.pattern_generator
pattern_generator.trace(num_analyzer_samples = 16)
pattern_generator.setup(up_counter, 
                        stimulus_group_name = 'stimulus', 
                        analysis_group_name = 'analysis')

pattern_generator.step()                        
pattern_generator.show_waveform()                        
```
按照上面代码，我们生成一个模式生成器，其创建方式与布尔型生成器一模一样。在配置setup的时候，我们传入之前我们自己写的波形数据，并把模拟信号和分析内容指示给他。随后，我们调用模式生成器的step函数，即可跟踪模拟信号。重复运行step函数，我们可以看到，analysis栏波形按照上面stimulus栏的波形进行输出。

![](https://i.postimg.cc/qqrmC2VY/1.jpg)

## FSM生成器
用FSM生成器来生成一个FSM（有限状态机）。这个例子中，我们做出来的FSM是一个格雷码计数器，它有三个状态位并可以通过8个状态来计数。计数器的输出是用格雷码编写的，这意味状态之间的转换只有一个2进制位会被改动（这是格雷码的特性）。

```python
from pynq.overlays.logictools import LogicToolsOverlay

logictools_olay = LogicToolsOverlay('logictools.bit')

fsm_spec = {'inputs': [('reset','D0'), ('direction','D1')],
            'outputs': [('bit2','D3'), ('bit1','D4'), ('bit0','D5')],
            'states': ['S0', 'S1', 'S2', 'S3', 'S4', 'S5', 'S6', 'S7'],
            'transitions': [['01', 'S0', 'S1', '000'],
                            ['00', 'S0', 'S7', '000'],
                            ['01', 'S1', 'S2', '001'],
                            ['00', 'S1', 'S0', '001'],
                            ['01', 'S2', 'S3', '011'],
                            ['00', 'S2', 'S1', '011'],
                            ['01', 'S3', 'S4', '010'],
                            ['00', 'S3', 'S2', '010'],
                            ['01', 'S4', 'S5', '110'],
                            ['00', 'S4', 'S3', '110'],
                            ['01', 'S5', 'S6', '111'],
                            ['00', 'S5', 'S4', '111'],
                            ['01', 'S6', 'S7', '101'],
                            ['00', 'S6', 'S5', '101'],
                            ['01', 'S7', 'S0', '100'],
                            ['00', 'S7', 'S6', '100'],                            
                            ['1-', '*',  'S0', '']]}  

# 生成实例
fsm_generator = logictools_olay.fsm_generator

# The FSM generator will work at the default frequency of 10MHz. This can be modified using a frequency argument in the setup() method.
fsm_generator.setup(fsm_spec)

# 显示状态转移图
fsm_generator.show_state_diagram()

fsm_generator.run()
fsm_generator.show_waveform()

fsm_generator.stop()
```
![](https://i.postimg.cc/CKhSbrkC/2.jpg)
![](https://i.postimg.cc/ncqXk5jN/1.jpg)

## PYNQ Library详解 - PS与PL接口
USB端口和其他的标准接口可以连接现成的USB和其他外部设备到Zynq PS上，并可以通过Python/Linux进行操控。
PYNQ提供overlay的一些底层控制，包括内存映射IO读写，内存分配等。

### interrupt 中断
**Interrupt类**代表了Vivado Block Design中的单一中断引脚。它通过使用一个wait函数去阻塞程序直到一个中断被抛出来模仿一个Python事件。

中断只有在一个线程或者协程在等待对应事件的时候才能使用，推荐使用方法为在一个循环里进行等待（wait）操作，检查并清除IP里的中断注册器，然后再结束等待。比如说，AxiGPIO类就用这个方法等待所需要的值。
```python
class AxiGPIO(DefaultIP):
    # Rest of class definition

    def wait_for_level(self, value):
        while self.read() != value:
            self._interrupt.wait()
            # Clear interrupt
            self._mmio.write(IP_ISR, 0x1)
```

### MMIO
**MMIO类**允许Python对象获取**映射的系统内存地址**。特别的，在PL里的外部设备的注册与地址空间都是可以访问的。

在一个overlay里，连接到 **AXI通用端口（GP）** 的外设将会把他们的注册器或地址空间映射到系统内存里。通过PYNQ，一个IP的注册器或者地址空间就可以通过MMIO类从Python获取。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45jVUe_Ugw9vd5Jg%2F53.png?generation=1574912159895092&alt=media)

MMIO提供了一个简单但方便的方法来访问控制外设。对于只有少量数据交互或者性能要求不高的外设，MMIO通常都是够用了的。如果对性能要求很高，或者大量数据要在PS和PL间传输，那么使用**带有DMA IP和PYNQ DMA类的Zynq HP接口**会更合适。

### PS GPIO
Zynq设备有最多从PS到PL的64个GPIO。它们可以用来做简单的操控。比如base overlay中通过PS GPIO来对IOP进行复位/重置。PS GPIO是一个非常简单的接口，并不需要PL中的IP就可以使用。
**GPIO类**就是用来控制PS GPIO的。(注意: AXI GPIO是由AxiGPIO类控制的)

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45jXG0ULRUQ7jW4o%2F54.png?generation=1574912159738464&alt=media)

 PS GPIO使用Linux核来控制GPIO。这意味着操作系统会在运行时给GPIO分配一个标号。在使用PS GPIO之前，我们必须把Linux引脚标号映射到Python GPIO实例上。

Get_gpio_pin()函数就是用来映射PS引脚标号到Linux引脚标号上的。

**Linux引脚标号?**

```python
from pynq import GPIO

output = GPIO(GPIO.get_gpio_pin(0), 'out')
input = GPIO(GPIO.get_gpio_pin(1), 'in')

output.write(0)
input.read()
```

### Xlnk
**Xlnk类**用来分配连续内存。

连接到**AXI Master（HP或ACP端口）的IP**能访问PS DRAM。

连接到AXI Master（HP或ACP端口）的IP能访问PS DRAM。在PL里的IP访问DRAM前，必须给IP分配内存。Python里的一个数组将会被分配到虚拟内存的某个地方。被分配的物理内存地址必须提供给PL里的IP。

Xlnk可以分配内存，并提供物理指针。它可以分配连续地址，PL IP可以用的更有效。通过Numpy包，Xlnk可以分配数组，这允许专门制定数组的数据类型。Xlnk也被DMA隐式使用来分配内存。

## PYNQ Library详解 - IP访问
Vivado工具为各种接口标准和协议的外设提供了IP，PYNQ给常用的外接设备- Video（HDMI IN/OUT）、 GPIO设备（Buttons, Switches, LEDs）、传感器和执行器等提供了Python API。这些PYNQ API也可以被扩展用以支持其他的IP。

### Audio
Audio模块提供了从输入麦克风读取音频、从播放器播放音频以及读写音频文件的方法。Audio模块是连接到Audio IP子系统上来捕获播放内容。该模块可以支持不同的IP子系统。目前支持的有line-in, PYNQ-Z2上使用ADAU1761编码的HP/Mic。
 
实例在 `<Jupyter Home>/base/audio/audio_playback.ipynb` 。
*It uses the audio jack HP+MIC to play back recordings; it can take inputs from the microphone on HP+MIC or LINE_IN. Pre-recorded audio sample can also be taken as input. Moreover, visualization with matplotlib is shown.*

### AxiGPIO
AxiGPIO类提供了读写通用设备如LED、按钮、开关等（需要通过AXI GPIO控制IP连接到PL上）并接受来自外部的中断。
每一个AXIGPIO能有至多两个通道，每一个通道至多有32个引脚。
![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk4C5en0SfvtAreqaR%2F01.png?generation=1574912187256632&alt=media)

### AxiIIC
AxiIIC类提供了对AXI IIC控制器IP的读与写。send()和receive()方法可以用来读与写。
```python
send(address, data, length, option=0)
```
- Address是外部设备IIC的地址 
- Data是一个被发送到IP的字节数组 
- length是发送的字节数 
- option允许一个IIC重复运行

### DMA（直接内存访问）
PYNQ支持AX central DMA IP。DMA可以在PS DRAM与PL的快速交互中发挥出色的效果。DMA类只支持简单模式。

DMA有一个AXI Lite控制接口，一个读写通道以及连接到一个IP的流（stream）端口。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk4C5jp2rnhULxrG04%2F03.png?generation=1574912187122306&alt=media)

读通道会从PS DRAM中读取数据，并写入流中。写通道会从流中读取数据并写回PS DRAM里。

**需要注意**的是DMA在完成数据传输的时候，需要连接到DMA（写通道）的IP来设置AXI TLAST信号。若该信号未被设置，DMA将会永远无法完成该次传输。在使用HLS来产生IP的时候，这一点非常重要——TLAST信号必须在C代码中设置。

### Logictools
Logictools包含了Trace Analyzer以及三个PYNQ硬件生成器（布尔型生成器、FSM生成器、模式生成器）的驱动。

Logictools overlay里的主要硬件模块的基础操作都是一致的，它们是setup(), run(), step(), stop(), reset()。每一个模块可能有额外的独有**方法**来提供特定的功能。下图为运行的基本图解：

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk4C5na0bSI_8GlGdE%2F05.png?generation=1574912187117149&alt=media)

- 重置态 这个状态是一个模块在overlay加载后准备开始的状态。这个“重置”状态会一直维持直到使用setup()方法进行配置。 在“重置”状态下，所有的逻辑工具、overlay可用的IO都处于未连接状态。这可以预防一些由于粗心大意的驱动而造成的意外。  **模式生成器**有一个BRAM来存储将要生成的模式。在这个状态下，BRAM会被配置为0。类似的还有FSM生成器。

- 准备态 在这个状态下，生成器/分析器已经配置完毕。将要连接的输入输出引脚已经准备完毕，但是把这些引脚连接到内部硬件的接口开关还未配置。

 - 运行态 一旦生成器在准备状态，调用run()或step()即可使它们进入运行态。在这个状态下，接口开关已经配置为连接外部IO。硬件模块在这个状态下开始运行。 运行时默认是以single-shot模式。这个模式下，生成器将会在追踪分析器采集到足够多的数据或者模式结束后才会停止，这之后生成器和分析器会一起回到准备态。布尔型生成器是一个特例，它总是处于连续模式下运行。 在连续模式下，模式生成器会连续的生成它的模式，并在达到模式的末尾后循环运行。FSM生成器则会持续运行直到被停止。（使用stop()函数） 
 
 **通用方法：** 
 - **Setup()** 每一模块必须在使用前使用setup()进行配置。 注意，当配置完成后，IO接口并未连接。 
 - **Run()** 该方法启动模块，使之进入运行态。 
 - **Step()** 与run()方法类似，区别在于运行的时候是一步一步运行的。 在对模式生成器使用该函数时，碰到结尾即停止，不会循环。 FSM生成器只有在获得了足够多的样本后才能进行step操作。 
 - **Stop()** 若一个模块正在运行，则在其重新运行前必须先停止。 一个模块的运行被停止后，它的输出就会与外部IO断开，只有当重新回到运行态后才会连接。 
 - **Trace()** 追踪功能默认开启。当启用时，追踪分析器会捕获所有已连模块的信息。使用trace方法启用或者停用。 
 - **Reset()** 重置生成器到初始状态，**在更改硬件配置**时我们需要调用该方法。

**模式生成器：**
模式生成器支持至多64K的模式。根据sample clock的频率，会不断生产data word。
Sample clock也是可编程的。最慢的速度是252KHz，最快可以达到10MHz。

**FSM生成器（有限状态机）：**
FSM生成器有一个内置模块内存来执行有限状态机。在Arduino shield header上的20个引脚都是可用的。FSM必须有最少1个输入，最多可以有19个输出。最大的输入个数为8个。比如说，基于输入的个数，如下的配置都是可行的。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk4C6A3nRKlhiPBgEI%2F17.png?generation=1574912187140929&alt=media)

追踪分析器是由MicroBlaze子系统控制，它连在一个DMA上，DMA也受到加载配置信息的MicroBlaze子系统的控制，包括配置模块内存来执行FSM。

**Trace Analyzer（跟踪分析器）：** 
**片上调试**使得FPGA资源得以被用来监测调试时的内部外部信号。调试电路利用一些设计来保存系统运行时的信号数据。调试数据保存在芯片上的内存，并可以之后被读取分析。传统片上调试的常常会受限于本地内存的大小。这意味着只能得到有限的调试数据。 片上调试的概念已经被扩展到允许调试数据被保存在DDR内存。这使得我们可以获得更多的调试数据，并用python进行分析。 追踪分析器会监测PMOD和Arduino上的外部PL**输入输出模块**（IOB）。这些IOB是三态的。这意味着每一个引脚都会有三个内部信号：input（I），output（O），tri-state（T）。T信号被用来控制一个引脚是用作输入还是输出。Trace Analyzer会连接到IOP上的所有3个信号。 

### Video
Video子包里有一整套驱动集合——从HDMI-IN端口读取数据、写数据到HDMI-OUT端口、传输数据、设定中断、操作视频帧。

 视频硬件子系统含有 HDMI-IN模块、HDMI-OUT模块和一个视频DMA。HDMI-IN和HDMI-OUT模块都支持彩色空间转换。例如从YCrCb到RGB或者反过来。 PYNQ-Z2板上有HDMI输入与HDMI输出端口。HDMI接口是直接连接到 可编程逻辑引脚 上的。在板上没有外用的HDMI电路。HDMI接口是由 可编程逻辑模块 的 HDMI IP 控制的。 HDMI IP是链接到处理系统DRAM上的，视频可以以流的方式从HDMI输入端进入内存，然后再从HDMI输出端流出。这就允许我们通过python来处理视频数据，或者干脆通过python写一段视频流然后再由HDMI输出。 虽然Jupyter notebook支持内嵌的video形式，但是我们从HDMI捕捉到的视频数据会是原生的，如果不进行适当的编码处理，将无法在notebook上直接播放。 - **HDMI IN**

**YCrCb:** YCrCb即YUV，主要用于优化彩色视频信号的传输，与RGB视频信号传输相比，它最大的优点在于只需占用极少的频宽（RGB要求三个独立的视频信号同时传输）。其中“Y”表示明亮度（Luminance或Luma），也就是灰阶值；而“U”和“V” 表示的则是色度（Chrominance或Chroma），作用是描述影像色彩及饱和度，用于指定像素的颜色。“亮度”是透过RGB输入信号来建立的，方法是将RGB信号的特定部分叠加到一起。“色度”则定义了颜色的两个方面─色调与饱和度，分别用Cr和Cb来表示。其中，Cr反映了RGB输入信号红色部分与RGB信号亮度值之间的差异。而Cb反映的是RGB输入信号蓝色部分与RGB信号亮度值之间的差异。

HDMI输入端IP可以捕捉标准的HDMI分辨率。在HDMI资源被连接后，HDMI控制器就会启动，并自动检测输入的数据。分辨率情况可以从Python 的HDMI类里读取，图像数据也可以流入处理系统DRAM。

## PYNQ Library详解 - PS and PL control


## PYNQ Library详解 - IOP
Zynq平台通常有多个Headers和接口，它们用来连接外部设备或者直接连接Zynq PL引脚。许多现成的外部设备都可以连接到Pmod和Arduino接口上。其他的外部设备可以通过转换器（Adapter）或者面包板（Breadboard）连接到这些端口。**需要注意的是**当我们要使用一个外部设备的时候，我们必须先**在overlay中构建一个控制器**，并提供相应的软件驱动，然后我们才能使用这个设备。
- Arduino
- Grove
- Pmod
- RPi

### Arduino
Arduino子包包含了所有**用来控制** 连接到Arduino端口的外部设备 **的驱动装置**。一个Arduino连接器可以把Arduino compatible shields连接到PL引脚上。 不过要记得**在一个overlay里必须有相应的控制器来执行对应的驱动**，这之后shield才能被使用。 Arduino引脚也可以用作通用引脚来连接传统的硬件设施。

如果有的话，一个Arduino PYNQ MicroBlaze可以控制Arduino接口。这个MicroBlaze就和Pmod的MicroBlaze一样，只是有更多的AXI控制器。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45Tm2OOYZ-C5fpav%2F38.png?generation=1574912159325564&alt=media)

正如在图标里所示，Arduino PYNQ MicroBlaze有一个PYNQ MicroBlaze子系统，一个配置开关，以及许多AXI控制器 [Arduino](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao/0704pynq-library-xiang-jie-iop#arduino)。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45To804LALQA5LIM%2F39.png?generation=1574912159104658&alt=media)

在pynq.lib.arduino包里可以找到更多有关Arduino的信息。
https://pynq.readthedocs.io/en/latest/pynq_libraries/arduino.html

### Pmod
Pmod包是一个 使用Pmod端的外部设备 的驱动集合。

### Rpi
Rpi子包是控制 连接RPi（Raspberry Pi）接口外设 的驱动集合。
同样的，在使用具体设备之前，我们需要加载相应的overlay上的控制器。RPi引脚也可用作通用引脚来连接传统的硬件设备。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45U9Z-38B3Iv8Vwl%2F50.png?generation=1574912159389267&alt=media)

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45UBlbtSd7VpQGoz%2F51.png?generation=1574912159169195&alt=media)

在pynq.lib.rpi包里可以找到更多信息。

## PYNQ Library详解 - Pynq MicroBlaze
PYNQ库提供了对子系统Pynq MicroBlaze的支持。
它允许我们加载 预编译好的应用，并且可以在Jupyter中 创建编译 新的应用。

### MicroBlaze Subsystem
PYNQ MicroBlaze子系统 可以由 PynqMicroblaze类 进行管控，这允许我们从Python下载程序，通过执行处理器的重置信号、共享数据内存读写 和 管理中断 来进行操控。

每一个 PYNQ MicroBlaze子系统 都含有一个 IOP（IO处理器），一个IOP定义了一些能被Python控制的交互与动作控制器。现在一共有三个IOP：Arduino，PMOD，Logictools。

该子系统含有一个MicroBlaze处理器，AXI交互、中断控制器，一个中断请求器和外部系统接口以及BRAM、内存控制器。

![](https://2192837526-files.gitbook.io/~/files/v0/b/gitbook-legacy-files/o/assets%2F-Luk3T3QoKyS_b-1joBV%2F-Luk3tukeoHpawWbVABz%2F-Luk45lutSSL0eZk-Bfp%2F52.png?generation=1574912162153238&alt=media)

AXI交互控制器把MicroBlaze连接到中断控制器、中断请求器和外部接口上。
- 中断控制器是其他连接到MicroBlaze处理器的交互/动作控制器的接口。
- 中断请求器发送中断请求至Zynq处理系统。
- 外部接口允许MIcroBlaze子系统与其他控制器或DDR内存进行交互。
- BRAM保存了MicroBlaze的指令和数据。

BRAM是双端口的：一个端口连接到MicroBlaze指令与数据端口，另一个连接到ARM® Cortex®-A9 通讯处理器。

如果外部接口连接到了DDR内存，则DDR可以用来在子系统和PS之间传输大量数据分段。

**创建一个新的PYNQ MicroBlaze**
学到这不学了，等到觉得学这个对以后有帮助再继续吧。[创建一个新的PYNQ MicroBlaze](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao/0705pynq-library-xiang-jie-pynq-microblaze#chuang-jian-yi-ge-xin-de-pynq-microblaze)

# 本文参考
[官方文档](https://pynq.readthedocs.io/en/latest/)
[PYNQ-Z2 中文资料](https://pynqdocs.gitbook.io/pynq-tutorial/pynq-zhong-wen-zi-liao)
来自公众号“PYNQ开源社区”的 [PYNQ入门资料集锦](https://mp.weixin.qq.com/s/GW_Ke2wEbcr3iXlML8O12w)
[Umer Farooq的文章](https://blog.umer-farooq.com/a-pynq-z2-guide-for-absolute-dummies-part-ii-using-verilog-and-vivado-to-burn-code-on-pynq-d856f79948b1)