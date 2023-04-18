---
title: UbuntuNote
categories:
  - System
tags:
  - Ubuntu
hidden: false
date: 2023-04-18 19:01:57
---

学习Ubuntu

<!--more-->

# 重装双系统(Ubuntu)

## 彻底卸载Ubuntu

原因：重装windows系统，Ubuntu引导不见了，Ubuntu无法使用了。或者想重装Ubuntu，或者不再使用，腾出空间。


### 删除Ubuntu系统磁盘空间

如果在之前安装Ubuntu时选择的是下图选项：

![](UbuntuNote/1.png)

则一般有三种创建分区的方式：
1. 分为两个区('/'、'/home')、
2. 分为四个区('swap'、'/'、'/home'、'efi')
3. 分为五个区('swap'、'/'、home'、'efi'、'/usr')

我之前装的时侯分成了四个区即('swap'、'/'、'/home'、'efi')，因此在电脑的磁盘管理可以看到这四个区，其中'efi'也即**系统分区**，很重要，之前的话里面有Ubuntu和Windows的启动项，不放在一起的话Ubuntu的启动项会自己放在另一个**efi系统分区**，后面再说。其它三个分区都可以直接删除，在磁盘管理处直接删除卷即可。


### 删除Ubuntu的系统启动项

键盘按住 Win + R，输入 diskpart 进入相应命令行，输入 list disk 查看所有磁盘，并输入 `select disk 盘编号` 选择的盘应该是Ubuntu的启动项所在的那个盘，我的情况是Windows和Ubuntu的启动项所在的efi系统分区，和系统盘(C盘)并不在同一个磁盘，很奇怪。输入 list partition 查看该磁盘所有分区，并`select partition 分区编号`选择“系统”分区（即该efi） ，输入 `assign letter=字母` 将其分配成一个独立磁盘（磁盘字母随意只要不和现有磁盘冲突即可，如assign letter T）。磁盘T不可访问，可以通过软件 Total Commander来删除Ubuntu系统启动项，也可以`以管理员身份打开“记事本”，点击菜单栏“文件 -> 打开”，选择刚刚创建的磁盘T，打开其中文件夹“EFI”，并选择该目录下的“Ubuntu”文件夹右键删除`，最后`remove letter=磁盘字母`删除创建的磁盘。

把引导菜单中原Ubuntu系统涉及的启动项删除，可以在软件easybcd里编辑引导菜单：

![](UbuntuNote/2.png)

删除Ubuntu项，这样在电脑开机进入bios的时候就看不到Ubuntu了。

**至此彻底删除Ubuntu。**

可参考：
[双系统如何删除一个系统](https://www.laomaotao.net/more/2021/0330/9229.html)
[彻底卸载Ubuntu双系统](https://blog.csdn.net/qq_42257666/article/details/120721561)
[双系统下重装Ubuntu系统](https://beingjay.com/2021/10/08/%E5%8F%8C%E7%B3%BB%E7%BB%9F%E4%B8%8B%E9%87%8D%E8%A3%85Ubuntu%E7%B3%BB%E7%BB%9F/)


## 安装Ubuntu

### 准备过程

下载Ubuntu镜像文件，我下载了最新的LTS版本，即`Ubuntu 22.04.2 LTS`，可以选择桌面版(Desktop)或服务器版(Server)。
[下载Ubuntu桌面系统 | Ubuntu](https://cn.Ubuntu.com/download/desktop)

之后使用Rufus制作启动盘，至少大于4G，注意：制作启动盘会格式化硬盘。注意不要选错U盘，分区类型务必选择GPT分区表，目标系统类型务必选择UEFI(非CSM)，若你的BIOS只支持NTFS，要改成NTFS，不过一般都是支持FAT32的，先用FAT32，如果不行再回来改。

![](UbuntuNote/3.png)

选择以iso镜像模式写入，进度条满之后即可。

压缩磁盘空间得到一个未分配的空间，推荐大小为：
1. 只是玩一玩linux系统，则分配30GB
2. 学习ROS，80GB以上
3. 深度学习、机器学习，100GB以上
4. 软件开发，50GB

 联想电脑Y9000P进入BIOS是F2，不行的话就Fn+2(数字键盘的2也可以)，F12可以选择以什么方式启动。

 在BIOS中关闭安全启动(secure boot)和快速启动(fast boot)，我只关了安全启动。


### 安装过程

插入U盘，进入BIOS设置从U盘启动或者直接F12选择从U盘启动，启动后进入Grub界面，类似这样：

![](UbuntuNote/4.png)

我的是：
1. Ubuntu
2. Ubuntu高级选项
3. Windows Boot manager
4. something else

安装时都选择中文，**不联网(没换源，下载的话会很慢)**，我选择了最小安装(简洁无敌)。**一定要选择其他选项：**

![](UbuntuNote/5.png)

#### 分区

**这个过程十分关键**

找到你刚刚压缩出来的空闲空间，根据大小判断，我的是260G左右。

**选中空闲的空间**，点击底下的➕建立分区，这次总共建立三个分区。
1. EFI系统分区
   也即启动分区，系统将从这里加载启动，内核文件也放在这里，大小选择512MB。**分区类型选择逻辑分区，空间起始位置，"用于"选择EFI系统分区**，如下：

![](UbuntuNote/6.png)

2. 根目录
   这个越大也好，因为很多程序默认安装在根目录的opt文件夹下，新手不建议乱改安装位置。实际上剩余的全给根目录('/')也没关系，因为Linux文件系统采用的是"使用时分配"的策略。也就是说，你用了多少，就会给那个分区分配多少分区，没用的时候，剩下的空间就处于"待分配"的状态。**这里分区类型选择主分区，"用于"选择Ext4日志文件系统**，如下：

![](UbuntuNote/7.png)

3. /home目录
   这个是用户自己的空间，和Windows下的C盘以外的空间差不多，保存用户自己的数据，重装系统时这部分不会丢失，其余的空间都给/home。**这里分区类型选择逻辑分区，空间起始位置，"用于"选择Ext4日志文件系统**。

这次安装Ubuntu没有分配**Swap区**(虚拟内存)，因为现在大家内存都很大了，没有这个必要了，很多教程都比较老了，对现在的环境来说就是坑了。

![](UbuntuNote/1.jpg)

![](UbuntuNote/8.png)

可以发现存在两个EFI系统分区，我知道448MB里面存放了Windows系统的启动项，488MB应该就是刚刚为Ubuntu创建的EFI系统分区，里面有启动项。


#### ELSE

分区完后注意，在**安装启动引导器的设备**处，一定要选择刚刚为Ubuntu系统分配的**EFI系统分区**，还是根据大小判断(我们分配的大小会是511MB)，别选成了Windows的EFI系统分区(因为我为Ubuntu重新分配了EFI系统分区，这次两个系统的启动项就不在同一个EFI系统分区里了)

![](UbuntuNote/1.jpg)

确定选择正确后便可以安装。

设置密码图方便可以设置为空格(我就是这么做的)，root密码我设为了123，进入root的命令是`su root`。

拔掉U盘之后就可以重启了，重启后进入Grub界面，选择Ubuntu则启动Ubuntu，选择Windows Boot Manager则启动Windows。如果嫌每次都进Grub太麻烦可以在BIOS中调整顺序把Windows放到Ubuntu前面，则每次默认直接进Windows。

可参考：
[Ubuntu/Windows双系统安装巨详细](https://blog.csdn.net/NeoZng/article/details/122779035)



# 学习Ubuntu

待续~~

