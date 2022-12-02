---
title: hlsNote
date: 2022-12-02 16:24:20
categories:
  - FPGA
tags:
  - HLS
  - FPGA
hidden: false
---

# 参考资料
[pynq-z2资料下载](http://e-elements.com/product/show/id/60.shtml)
[TUL pynq-z2](https://www.tulembedded.com/FPGA/ProductsPYNQ-Z2.html)

<!--more-->

# 毕设题目
**基于HLS的手势识别系统设计**
Design of gesture recognition system based on High Level Synthesis
实验室建设，硬件，难

手势是人们常用的肢体语言之一，符合人类日常习惯的交互手段，是一种自然直观、有效简洁的沟通方式，在日常生活中人们之间的交流通常会辅以手势来传达一些信息或表达某种特定的意图。随着人工智能和计算机视觉技术的迅猛发展，智能化设备逐渐融入到人们生活的方方面面，因此对各类人机交互的需求也在不断增加，手势识别逐渐成为基于计算机视觉的智能人机交互的重要研究领域。 **本项目希望采用面向FPGA应用的高层逻辑综合（HLS）技术，通过学习比较成熟的手势识别相关算法，通过HLS技术将算法转化为FPGA可综合的硬件描述语言（HDL）知识产权（IP）核模块，在FPGA中采用硬件实现基本的手势识别算法，并对比算法的实现性能；通过优化，进一步提高手势识别算法的实时性。** 设计要求如下：
>（1）熟悉手势识别处理算法的基本流程，仿真算法的识别效果；
>（2）利用Xilinx的HLS方法实现算法的优化与性能仿真；
>（3）搭建FPGA平台，将手势识别算法在FPGA中进行验证，并可以演示；
>（4）扩展功能：可以进行手势识别与分类显示。

# HLS简介
## 高层综合简介

## HLS设计流程
Vivado HLS 的功能简单地来说就是 把 C、 C++ 或 SystemC 的设计转换成 RTL 实现，然后就可以在Xilinx FPGA 或 Zynq 芯片的可编程逻辑中综合并实现了 。需要注意的是，这里我们说的使用 C/C++完成的设计与运行在处理器（ ZYNQ中的 ARM处理器或 MicroBlaze软核处理器）中的软件代码是截然不同的。在 HLS中，所有的 C设计都是要在可编程逻辑中实现的，也就是说，我们仍然是在进行硬件设计，只不过使用的不再是硬件描述语言。
使用Vivado HLS进行设计的流程如下图所示：

![](https://i.postimg.cc/ncxXTjcb/5.jpg)

HLS设计的主要输入 是一个 C/C++/SystemC 设计 ，以及一个基于 C 的测试集 TestBench）。我们首先要知道 C语言的本质就是函数，那么 这个测试集 就是用于验证 C设计中的 函数，验证 过程需要一个 “黄金参考” 。这个 “黄金参考 类似于一个标准答案，用来 和 C设计中 函数所产生的输出做比对。

在对HLS设计进行综合之前，我们要先对其进行“ 功能性验证 ”，也就是 C仿真，其目的是验证 HLS 输入的 C代码的功能是否正确。验证的方式就是在 TestBench中调用 C设计的函数，然后将其输出与“黄金参考”进行比对，如果与黄金参考有差异就需要先对 C设计进行修改调试。

接下来就是对设计进行高层综合 ，即 HLS过程本身。该过程涉及到分析和处理基于 C 的代码，加上用户所给出的指令和约束，来创建 RTL描述。高层综合结束后会产生一组输出文件，包括以 Verilog或者VHDL语言编写的 RTL设计文件。

综合过程结束后得到的RTL模型，可以在 Vivado HLS 中进行 C/RTL 协同仿真 ，来进一步验证综合得到的RTL设计的正确性。在这个过程中 Vivado HLS会自动产生一个测试集为 RTL设计提供输入，然后拿它的输出与预期的值做比对。 C功能性验证和 C/RTL协同仿真 的区别如下图所示：

![](https://i.postimg.cc/cCyrp6R3/6.jpg)

左侧的功能性验证（ C仿真）中，原始测试集是用户输入的测试文件TestBench。而右侧的C/RTL协同仿真所需的 RTL测试集是由 Vivado HLS 自动产生的，这样就不再需要人工创建了，所产生的测试集包括了原始测试集和被测 RTL模块之间的数据传递。

除了对功能进行验证，我们还要评估 RTL设计的实现和性能 。比如，在 FPGA中所需的资源的数量，设计的延迟、所支持的最高时钟频率等是否 满足要求。如果不满足要求，那么就需要设计者通过修改指令和约束，然后再次进行高层综合，一个设计可能要做多次 HLS设计迭代，来找到“最佳 ”的解决方案。如果有必要，设计者也可以返回修改 C设计代码，然后从头开始重新对设计进行验证。

在设计被验证了之后，而且实现也满足了期望的设计目标，那么就可以集成进更大的系统里了。我们可以直接使用 HLS 过程所产生的 RTL文件（即 VHDL 或 Verilog 代码），更方便的做法是使用 Vivado HLS 的 IP 打包功能。对 Vivado HLS 所产生的输出打包意味着 HLS 设计能够以 IP核的形式引入其他 Xilinx 工具中，比如 Vivado中的 IP 集成器。这两种类型的输出如下图所示：

![](https://i.postimg.cc/65QyDsRY/7.jpg)

## 接口综合
设计者需要分析设计的两个主要方面：
- 设计的 接口 ，也就是它的顶层连接
- 设计的 功能 ，也就是它所实现的算法

给出一个HLS设计中接口和功能的概念图 

![](https://i.postimg.cc/tgb4gfvV/1.jpg)

在上图中，两端的绿色区域表示设计的输入和输出接口，其中展示了部分接口类型，如RAM接口、 FIFO接口，以及总线类型的接口等。这些接口可以是HLS工具从代码中通过 接口综合（ Interface Synthesis 得到的，也可以由设计者手动指定具体的接口类型。

图中间黄色的区域表示HLS设计具体能够实现的功能，对于不同的应用，其功能也各不相同。 在 Vivado HLS 设计中，功能是从输入的代码中，经过算法综合（ Algorithm Synthesis）的过程得到的 。

### 接口综合：
在这里我们先简单介绍一下接口综合。顾名思义，Interface Synthesis指的是 HLS 设计中对接口的综合，综合出来的接口能够与系统中的其他模块通信，还有可能需要与系统中的处理器进行通信。
这里接口的概念既包括端口（port），也包含所使用的协议。所有端口的细节（如类型、位宽和方向是从 C/C++ 文件中**顶层函数的参数和返回值**里推断出来的；而协议是从端口的表现（行为）推断出来的。比如，最简单的接口可以是一条 1 比特的线（ wire），而更复杂的接口，可能要用总线或 RAM 接口。接口综合能够推断出来的接口类型包括：线、寄存器、单向和双向握手信号 、 FIFO、存储器和总线等。

**举例：** 下面给出一个简单的C设计的顶层函数，函数名为 find_average_of_best_X()
```c
void find_average_of_best_X(int *average, int samples[8], int X) {
    // 主题函数（生命，子函数调用等）
}
```
函数内部工作的详细情况无关紧要，不过**每个参数的读 /写操作**将决定综合出来的端口的方向。这个函数定义包含三个参数，数组“sample”和整数 “X”是函数的输入，而 average作为函数的输出。因此，简单来说，这三个函数参数要被 HLS 转换成两个输入接口和一个输出接口，如下图所示：

![](https://i.postimg.cc/FsBQqqQQ/2.jpg)

需要注意的是，上图只是一个简化了的接口示意图。根据**所用的协议**，这些接口可能包括数据端口以外的控制输入或输出，如下图所示：

![](https://i.postimg.cc/43CR49dL/3.jpg)

上图是 函数 find_average_of_best_X()经 HLS综合出来的完整的 RTL模块的接口图。从图中可以看到由函数的三个参数所综合出来的接口**分别拥有了各自的协议**，如 ap_memory协议、 ap_none协议和 ap_vld协议。同时模块还多出来了一些端口，如 ap_clk和 ap_rst等，它们使用的是 ap_ctrl_hs 协议。这些协议决定了相应的接口是如何与**系统中其他模块**进行交互的， 至于**各协议具体的含义以及如何为接口选择其协议**，将是学习HLS设计的重点。

## 算法综合
算法综合关注的是设计的功能，即设计所期望的行为，它是由输入的
C设计所描述的。算法综合从代码中推出各种运算操作，然后转换成一组 RTL语句。
算法综合包括三个主要阶段，依次是：
- 解析出数据通路和控制电路；
- 调度和绑定；
- 优化；

### 解析出数据通路和控制电路
HLS 的第一个阶段是分析 C/C++/SystemC代码，并且解释所需的功能。 Vivado HLS从以下几个方面分析程序 逻辑和算法的运算、条件语句和分支、 数组运算和循环 等。
所产生的实现会具有一个数据通路元件，一般还会有一个控制元件。~~需要澄清的是，这里的“数据通路”处理指的是在数据样本上作的运算，而“控制”是协同数据流处理所需的电路。~~ 算法的本质定义出数据通路和控制元件，设计者可以在 HLS中采取专门的步骤来最小化控制元件的复杂度。

### 调度和绑定
HLS 是由两个主要过程组成的：调度（ Scheduling）和绑定 Binding）。它们是交替进行的，彼此互相影响，如下图所示：
![](https://i.postimg.cc/5tq57q5v/4.jpg)
- 调度 是把由 C 代码解释得到的 RTL 语句翻译成一组运算，每个运算都关联着一定的执行时间，以时钟周期为单位。这个阶段所作的决策，受时钟频率和不确定度、目标芯片的技术和用户所施加的指令所影响。
- 绑定 是调度好了的运算和目标芯片上的实际资源联系起来的过程。这些资源的功能和时序特征可能会影响调度，因此绑定信息会反馈给调度过程。

比如，如果综合出来的算法需要做一组算术运算，HLS过程就必须根据目标的时钟频率和不确定度来决定如何调度这些运算（要分配多少个时钟周期来完成），以及如何绑定这些运算（也就是如何把运算映射到 PL上的可计算资源里）。 C源码并不能表达或指定硬件架构，但是通过施加指令，源码确实可以产生不同的架构。

### 优化
有两种方法可以用来调整HLS过程的行为，让高层综合朝着设计者的实现目标而努力，从而影响结果：
- 约束: 设计者可以对设计的某些指标加以限制。比如，可以指定最低的时钟周期。这样就能确保实现结果能够满足要集成进去的系统的要求。类似的，设计者可以选择约束资源的利用情况 或其他的指标，从而优化应用的设计。
- 指令 设计者可以通过指令对 RTL的实现参数施加更具体的影响。有各种类型的指令，分别映射在代码的某些特征上，比如让设计者可以指定 HLS引擎如何处理 C代码中识别出来的循环或数组，或是某个特定运算的延迟。这能导致 RTL输出的巨大改变。因此，具有了指令的知识，设计者就可以根据应用的
需求来做优化了。

## HLS库
Vivado HLS中包含了一系列的 C库（包括 C和 C++），方便对一些常用的硬件结构或功能使用 C/C++进行建模，并且能够综合成 RTL。 在 Vivado HLS中提供的 C库有下面几种类型：
> 1. 任意精度数据类型库
> 2. HLS Stream库
> 3. HLS 数学库
> 4. HLS 视频库
> 5. HLS IP库
> 6. HLS 线性代数库

在HLS设计中调用库中的函数可以大大提高开发效率=。

# HLS 编程

## 循环入门
为了提升性能，循环常采用“流水打拍”或“展开”，以便充分利用FPGA架构的高度分布化和并行化。

### 循环流水打拍
流水打拍循环允许在前一次循环迭代完成前就启动后一次循环迭代，从而支持部分循环在执行时重叠。默认情况下，循环的每次迭代仅在前一次迭代完成后才会开始。

```c++
vadd: for(int i=0; i < len; i++) {
    c[i] = a[i] + b[i];
}
```

假定在硬件中，一次循环迭代需要三个周期，同时假定循环变量len为20，即vadd循环在内核内运行20次迭代。因此，总计需要60个时钟周期才能完成此循环的所有操作。
**tips:** 最好始终按照以上实力所示方式来标记循环（vadd:...），这样有助于在Vitis HLS中进行设计调试。

循环流水打拍支持该循环的后续迭代发生重叠且以并发方式运行，循环流水打拍可以通过在循环主体里添加 `pragma HLS PIPELINE` 来启用：

```c++
vadd: for(int i=0; i < len; i++) {
    # pragma HLS PIPELINE
    c[i] = a[i] + b[i];
}
```

启动下一次循环迭代所需要的周期数称为流水打拍循环的启动时间间隔（Initiation Interval，II）。II=2表示循环的下一次迭代会在当前迭代发生的2个周期后启动。II=1是理想情况，即每个周期都会启动一次循环迭代。使用 `pragma HLS PIPELINE` 时，可以指定编译器要实现的II，默认情况下编译器将尝试实现II=1。

下图展示了流水打拍循环和非流水打拍循环的执行差异。

![](https://i.postimg.cc/9QnLjBVr/10.jpg)

**tips：** 循环流水打拍会自动展开流水打拍循环内部嵌套的任意循环

如果循环内部存在任何数据依赖关系，则有可能无法实现II=1，并且可能导致启动时间间隔增大。在以下示例中，循环结果用作循环持续条件或退出条件，只有在前一次迭代结束后后一次循环迭代才能开始。
```c++
Minim_Loop: while(a != b) {
    if(a>b) a-= b; 
    else b -= a;
}
```

### 自动循环流水打拍
config_compile -pipeline_loops 命令会根据迭代次数对循环进行自动流水打拍循环，低于指定迭代次数限值的所有循环都将自动流水打拍，默认值是64。
如：
```c++
for (y=0; y<480; y++) {
    for (x=0; x<640; x++) {
        for (i=0; i<5; i++) {
            // do something
        }
    }
}
```
如果 pipeline_loops 选项设置为 6，那么以上代码片段中最内层的 for 循环将自动流水打拍。这等同于以下代码片段：
```c++
for (y=0; y<480; y++) {
    for (x=0; x<640; x++) {
        for (i=0; i<5; i++) {
            # pragma HLS PIPELINE II=1
            // do something
        }
    }
}
```

### 回绕已流水打拍的循环以保障性能











# HLS 库

## 任意精度数据类型库(arbitrary precision)
**tips:** Vitis HLS 中C语言不支持任意精度数据类型库，只能在C++中使用，参考 https://support.xilinx.com/s/article/75770?language=en_US。

Vitis HLS可以为C++提供整数数据类型和定点任意精度数据类型。

| 语言 | 整数数据类型 | 所需头文件 |
|  :----:  |  :----:  |  :----:  |       
| C++ | ap_[u]int\<W>，位宽为1024，可扩展到4K位 | #include "ap_int.h" |
| C++ | ap_[u]fixed\<W> | #include "ap_fixed.h" |

### 用于 C++ 的任意整数精度类型
头文件ap_int.h用于为C++定义任意精度整数数据类型，要在C++中使用任意精度整数数据类型，请执行以下操作：
1. 引入头文件 ap_int.h
2. 针对有符号的类型将位类型更改为ap_int\<N>，针对无符号的类型使用ap_uint\<N>，其中N为范围介于1~1024之间的位大小。

以下示例显示了如何添加头文件并实现 2 个变量来使用 9 位整数和 10 位无符号的整数类型：
#include "ap_int.h"

```c++
void foo_top() {
    ap_int<9> var1;
    ap_uint<10> var2;
}
```

**tips:** AP数据类型的劣势之一是阵列不会以0值进行自动初始化，如果需要初始化阵列，必须手动执行。

### 用于 C++ 的任意精度定点数据类型
使用定点数据类型执行的C++语言 仿真的行为与综合创建的硬件的行为 相匹配，从而能够使用C语言层次快速仿真来分析位精度、量化和上溢的影响，在Vitis HLS中使用定点数据非常重要。

# Vitis 使用

Vitis 软件平台支持嵌入式软件开发流程作为SDK的新一代技术，也支持Vitis应用加速开发流程，以满足使用基于Xilinx FPGA的最新软件加速功能的需求。

下图展示了Vitis 嵌入式软件应用开发工作流程：

## 嵌入式软件开发流程
![](https://i.postimg.cc/fRvT3b0g/14.jpg)

- 硬件工程师负责设计软件开发（从 Vivado® Design Suite 导出 XSA 存档文件）所需的逻辑和导出信息。
- 软件开发者负责通过创建平台来将 XSA（Xilinx Shell Archive） 导入 Vitis 软件平台，平台包含硬件规格和软件环境设置。
- 软件环境设置称为域，同样属于平台的一部分。
- 软件开发者基于平台和域来创建应用。
- 应用可在 Vitis IDE 中进行调试。
- 在复杂系统中，可能有多个应用同时运行并彼此通信。因此也需要执行系统级别验证。
- 全部就绪后，Vitis IDE 即可帮助创建启动镜像，用于初始化系统和启动应用。

## Vitis 软件平台中的工作空间结构
在 Vitis 工作空间内有两种类型的工程：

![](https://i.postimg.cc/3xQF8tgF/15.jpg)

- 工作空间：打开 Vitis 软件平台时，会创建工作空间。工作空间即供 Vitis 软件平台用于存储工程数据和元数据的目录位置。初始工作空间位置必须在启动 Vitis 软件平台时提供。
- XSA：XSA 是从 Vivado Design Suite 导出的。它包含各种硬件规格，例如，处理器配置属性、外设连接信息、地址映射和器件初始化代码等。创建平台工程时，必须提供 XSA。
- 平台：**目标平台**（或称平台）是由硬件组件 (XSA) 和软件组件（**域/BSP**、FSBL 之类的启动组件等）组合而成的。存储库(repository)内的平台不可编辑。 **工作空间内**的平台可编辑(custom 定制化)，称为**平台工程**。
- 平台工程：平台工程可以提供硬件信息和软件运行时（runtime）环境。它可定制，您可**添加域和修改域**设置。 平台工程可通过导入 XSA 或者通过导入现有平台的方式来创建。在同一个平台工程上可以创建多个**系统工程**，以便共享硬件和软件环境设置。
- 域：域即板级支持包 (BSP) 或操作系统 (OS)，其中包含软件驱动程序集合，您可在其中构建自己的应用。您可创建多个应用并在同一个域上运行。在平台中，每个域都绑定到单个处理器或者一个由同构处理器组成的集群（例如：A53_0 或 A53）。
- 系统工程：系统工程用于将任一器件上同时运行的应用组合在一起。在系统工程中，同一个处理器的两个独立应用不能组合在一起。在系统工程中，2 个 Linux 应用可以组合在一起。每个工作空间均可包含多个系统工程。
- 应用（软件工程）：每个应用包含一个或多个源文件以及必要的头文件，以允许编译和生成二进制输出 (ELF)文件。每个系统工程均可包含多个应用工程。每个软件工程都必须包含一个对应的域（应用对域是多对一的关系）。

## Vitis 软件平台与 SDK 之比较

![](https://i.postimg.cc/j216VbTR/16.jpg)














# 使用 pynq-z2 完成HLS流程
 
Overlay由 两个主要部分组成 bitstream文件 和 hwh Hardware Handoff 文件。可以说 **Overlay设计 其实就是一种 PL与PS的交互设计**。
PYNQ overlay具有 Python接口 ，从而允许软件程序员像使用其他任何Python软件包一样使用它，程序员可以在运行时将overlay下载到 Zynq PL中，以提供软件应用程序所需的功能。

因此，想要设计Overlay，需要先学习PS与PL的交互。

## PS与PL的交互

### ZYNQ PS与PL的接口

ZYNQ PS与PL之间有九路AXI接口。**在PL侧，有4路AXI Master HP(高性能)接口，2路AXI Master GP（通用）接口，2路AXI Slave GP接口和1路AXI Master ACP接口。** PS中还有连接到PL的GPIO控制器，如下图：

![](https://i.postimg.cc/pdZ7GT0n/11.jpg)

### linux内核 如何与 PL 交互

pynq是基于linux的，而linux运行在PS中，那么linux如何与PL进行交互：

![](https://i.postimg.cc/28ZJWmZh/12.jpg)

上图中看到了用于交互的linux驱动程序，linux内核与PL的FPGA之间通过**linux驱动fpga_manager, sysgpio, uio, devmem 和 xlnk** 进行交互，这些 **驱动** 和 **PS与PL之间的接口** 的对应关系是： 
- fpga_manager 用于下载bitstream（位流）文件到PL
- sysgpio 控制PS与PL之间的EMIO接口
- uio 实现PL到PS的中断管理
- devmem 用于PS侧的AXI **Master** GP接口
- xlnk 用于PS侧的AXI Slave GP接口 和 AXI Slave HP接口

### PYNQ 接口类 (Python 如何与 linux内核 交互)

![](https://i.postimg.cc/sXnCgRNq/13.jpg)

在 PYNQ 架构图中可以看到，上面所述 **linux驱动** 已经包装在Python库中。

在PYNQ中除了 **PL类** 用于通过 fpga_manager驱动下载 bitstream文件到 PL外，还有四个 PYNQ接口类 用于管理 ZYNQ PS（包括 PS DRAM）和 PL接口之间的数据移动。这四个类分别是：
- GPIO (General Purpose Input/Output 通用输入输出)：用于控制连接到PL侧的 GPIO外设，也可用作IP的终端或复位信号
- MMIO (Memory Mapped IO 内存映射IO)：可以使用Python代码访问与PS AXI Master GP接口相连的具有 PL AXI Slave GP 接口的IP核
- Xlnk (Memory allocation 内存分配)
- DMA (Direct Memory Access 直接内存访问)

**Xlnk类与DMA类：** 具有AXI Master接口的IP不受PS的直接控制，并且这样的IP允许直接访问DRAM。在访问DRAM之前，应先使用Xlnk类 **分配内存** 供IP使用。对于需要实现 PS DRAM 与 IP 之间更高性能的数据传输，可以使用 DMA。为此 PYNQ提供了 DMA类。

类的使用取决于IP的接口以及连接的 PS端的接口。
设计Overlay时应该考虑使用的接口类型以及使用哪些类来驱动IP。

## Overlay 设计GPIO 实验
ZYNQ有 54个MIO和 64个EMIO 其中EMIO是 PS与PL进行交互 的最简单直接的方式。在 PYNQ的 overlay设计中，自然少不了GPIO（EMIO）的使用。Overlay设计中涉及到的 **GPIO有两种，一种是 EMIO，另一种是 AXI GPIO**。

Zynq器件具有从 PS到 PL的多达 64个 GPIO。 这些 GPIO也被称为 EMIO 是一个非常简单的接口。

Zynq的 GPIO使用 Linux内核模块来控制。这意味着操作系统在运行时会为 GPIO分配一个数字。在PYNQ中使用 GPIO 之前，**须将Linux引脚号映射到 Python GPIO实例，GPIO类中的 get_gpio_pin() 函数用于将 Zynq的GPIO的引脚号 映射到 Linux的gpio引脚号 。**
from pynq import GPIO

先结束了，暂时用不上python了，电脑串口连接不上 pynq-2 是线的原因。

# 实战
使用 PYNQ-Z2 ，创建HLS工程时应该选择器件（part）

![](https://i.postimg.cc/hvDbQZ1B/9.jpg)

![](https://i.postimg.cc/zXN40q0z/8.jpg)

器件的选择参考[Xilinx FPGA 芯片命名规则](https://blog.csdn.net/Zenor_one/article/details/89453987)

## 按键控制LED实验

本实验使用 Vivado HLS 生成一个带有输入输出接口（ap_none）的IP核。学习使用C Simulation。

![](https://i.postimg.cc/ZKxp3r3Z/17.jpg)

设计的输入有 C/C++设计， Testbench测试集 以及 Constraints（约束）/directives（指令）。我们可以使用C Testbench对C程序进行仿真验证程序输出的正确性，也就是C Simulation， 此C Testbench也可以用来对综合后得到的RTL设计进行仿真，也即C/RTL协同仿真。

HLS规定了 **协议、端口类型和方向**之间的相关性，在HLS开发过程中，考虑C/C++函数参数的类型是十分重要的。Vitis HLS规定可以 传入/传出 C/C++ 函数的值有四种不同的数据类型，分别是**变量、指针、数组和引用**。一种特定的参数类型只对应有限的几种协议，如：传入一个数组作为形参，能使用的协议只有：ap_hs, ap_memory, bram, ap_fifo, ap_bus, axis 和 m_axi, 其中 ap_memory是默认的，参数类型对应的接口协议如下：

![](https://i.postimg.cc/TPT9pg95/18.jpg)

D：表示default，表示HLS工具默认综合出来的接口类型；
S：表示support，表示HLS工具支持综合出来的接口类型；

使用ap_none协议且**变量作为形参**时，接口只能被综合成输入（I）接口而不能被综合成输出接口。而指针变量作为形参则可以被综合成输入、输出和双向端口。


## 呼吸灯

本实验使用HLS生成一个带有AXI4-Lite总线接口的IP核。学习使用 C/RTL Simulation 协同仿真。在ZYNQ PS端通过AXI4-Lite总线来配置呼吸灯IP核的频率和开关。

AXI的英文全称是 Advanced eXtensible Interface，即高级可扩展接口，它是ARM公司所提出的 AMBA(Advanced Microcontroller Bus Architecture)协议的一部分。 AXI协议包含了 AXI4、 AXI4 Lite和 AXI Stream三种协议。

AXI4 协议支持突发传输，主要用于处理器访问存储器 等需要指定地址的高速数据传输场景。AXI4-Lite为外设提供单个数据传输，主要用于访问一些低速外设中的 寄存器。
AXI-Stream 接口则像 FIFO 一样，数据传输时 不需要地址，在主从设备之间直接**连续读写数据**，主要用于如视频、高速AD、PCIe、DMA接 口等需要高速数据传输的场合。

AXI4 -Lite 接口是简化版的 AXI4 接口，用于较少数据量的存储映射通信。本次实验只需要配置呼吸灯IP核的频率和 开关 ，因此接口类型选择 AXI4-Lite 接口。


```c
typedef struct {
    u16 DeviceId;
    u64 Control_BaseAddress;
} XBreath_led_Config;

typedef struct {
    u64 Control_BaseAddress;
    u32 IsReady;
} XBreath_led;

int XBreath_led_Initialize(XBreath_led *InstancePtr, u16 DeviceId);
XBreath_led_Config* XBreath_led_LookupConfig(u16 DeviceId);
int XBreath_led_CfgInitialize(XBreath_led *InstancePtr, XBreath_led_Config *ConfigPtr);

void XBreath_led_Set_sw_ctrl(XBreath_led *InstancePtr, u32 Data);
u32 XBreath_led_Get_sw_ctrl(XBreath_led *InstancePtr);
void XBreath_led_Set_freq_step(XBreath_led *InstancePtr, u32 Data);
u32 XBreath_led_Get_freq_step(XBreath_led *InstancePtr);


```






