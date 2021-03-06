---
title: "操作系统学习笔记-计算机系统概述"
date: 2019-09-15T22:19:48+08:00
description: ""
lastmod: 2019-09-26T0:53:47+08:00
subtitle:    ""  
author:      NoobCoder
image:       ""
tags:        ["操作系统"]
categories:  ["Tech" ]
keywords: [ ]
---

## 背景

回到学习方法论，学习任何知识点，都需要先搞清楚其背景知识，有了上下文，学习起来才可以更连贯，更好理解。

## 计算机系统概述

操作系统连接了计算机底层硬件与应用程序和用户，为了理解操作系统的功能和涉及的涉及问题，必须先对计算机组织与系统结构有一定的认识。

目前我对于计算机的组成认识也就CPU、内存、硬盘、io有个大概的了解，具体让我说，我可能还真说不出来。所以本节按照以下结构进行展开。

### 基本构成

基本构成的话，个人理解应该是处理器、存储器、IO模块？数据通过IO模块从存储器转移到处理器，然后产生的数据再通过IO模块回到存储器？

按照书本来说应该分为：

**处理器**(Process)：执行计算、处理数据等功能，当只有一个处理器时，被称为中央处理单元CPU(现在的手机中常说的多核处理器是指一个处理器中集成了多个计算核心，和多处理器还是有区别的)

**主存储器**(Main Memory)：即内存，用于存储数据于程序。与之对应的是磁盘存储器。主存储器易失，关闭计算机会导致主存内数据丢失。

**输入/输出模块(I/O)**：在计算机与外部环境之间移动数据，外部包括硬盘啊，键盘啊，网卡之类的。

**系统总线(System bus)**：为处理器、主存、IO模块间提供通信的设施。

那么这么说我之前对于IO模块和系统总线的理解有混淆了？经过搜索发现 **系统总线**才是我最开始理解的各个模块间通信的桥梁，而**IO模块**则像是窗口、或者是宫殿的各种门，每个门后边有不同的功能的人向宫殿内提供服务。鼠标IO模块负责光标移动，键盘IO模块负责输入用户的信息。

之前我对CPU有一个误解，那就是他只能进行计算，这一想法是和人脑进行对比时得出的，人脑可以思考+计算，而CPU只能计算，其实这会导致误解。其实CPU内部除了运算单元可以进行计算，还有很多寄存器可以进行数据交换。

处理器的一个功能是使用寄存器和存储器交换数据：

**存储器地址寄存器(Memory Address Register, MAR)**：确定下一次读写的存储器地址，即我这条指令运行完，要运行的下一条指令在存储器内的地址。为啥不直接把下一条指令取出来放到这里呢？而是存放一个地址，等到执行时再去取，浪费很多IO时间？

**存储器缓存寄存器(Memory Buffer Register,MBR)**:包含要写入存储器的数据或从存储器中读取的数据。也就是要送到运算单元的数据或者经过运算单元运算产生的数据。就像炒菜，得把原材料先切好放在固定的盘子里，炒完之后，再乘到一个大盘子里，由送菜的送走。厨师只管从材料那里拿且好的菜进行炒，然后乘到成品栏，由送菜员送走。

**输入/输出地址寄存器(I/O Address Register)**：确定一个特定的输入输出设备，即我产生的数据要发到哪一个IO设备

**输入/输出缓存寄存器(I/O Buffer Register)**：用于在IO模块和处理器之间交换数据。类似于MBR

总结，计算机的结构化部件为：处理器、存储器、系统总线、I/O模块。

而cpu包括 **执行单元、PC、IR(指令寄存器)、MAR、MBR、I/O AR、I/O BR**

### 处理器寄存器

这节其实看的我云里雾里，只知道介绍了2种寄存器类型:

**用户可见的寄存器**:这些寄存器可以被用户所调用分配，它包括：数据寄存器、地址寄存器

地址寄存器又有索引寄存器(基地址+索引值)、段指针(基地址+偏移)、栈指针(指向栈顶)

**控制和状态寄存器**：这些寄存器用于控制处理器的操作，所以只能被高权限的操作系统例程调用。它包括：	程序计数器PC：包含将取指令的地址

​	指令寄存器：包含最近取的指令内容

还有个**条件码**，是由处理器为操作结果设置的位，1 - 2 = -1，这个负号估计就是使用这个条件码

### 指令的执行

#### 啥是指令？

处理器执行的程序是由一组存储在主存种的指令组成的，既然可以被处理器直接执行，那么指令一定是高级语言经过一系列编译后的机器语言或者汇编语言了，就是不知道一条指令是一行还是一个函数的整个代码。

#### 最简单的指令周期

把大象装进冰箱分2步，打开冰箱，把大象装进去。

那么指令的处理也只有2步：处理器从存储器中一次读一条指令；然后执行读取的指令。

**一个单一的指令需要的处理称位一个指令周期，指令周期可划分位取指阶段和执行阶段**。程序执行就是不断重复指令周期。

#### 取指令和执行指令

IR：指令寄存器，存放正在执行的指令

AC：累加器，临时存储体

PC：程序计数器，存放下一条指令地址，每次取完指令后增加1

处理器解释指令，并执行指令代表的操作。指令可由以下从的操作进行组合：

* 处理器与存储器：交换数据
* 处理器与IO模块：交换数据
* 数据出路：算术操作、逻辑操作
* 控制：改变执行顺序

指令(16位)格式为  操作码(4位)+地址(12位)

部分操作码：

* 0001：从存储器加载AC
* 0010：把AC的内容加载到存储器中
* 0101：从存储器中加到AC中

**要搞清这个流程必须结合指令周期来进行，即取指执指循环。**

#### IO函数

就是IO模块可以和处理器直接交换数据，当我们要把移动硬盘的电影拷贝到电脑上，还要经过处理器嘛？当然不，这样太耗费处理器了，所以出现了**直接存储器存取(direct memory access DMA),允许你IO设备直接和存储器交换读写数据，减轻了处理器的负担。**

### 中断

就是我吃饭，你给我打电话，我放下筷子，跟你谈电话，谈完电话，我继续拿起筷子吃饭。

中断是为了合理高效地利用处理器资源，中断类型为

| 类别         | 说明                                                   |
| ------------ | ------------------------------------------------------ |
| 程序中断     | 由指令的结果产生，入算术溢出                           |
| 时钟中断     | 处理器规定一条指令只能干10个单位时间，那么到期自动中断 |
| I/O中断      | IO控制器产生，告诉你某个IO操作成功/失败                |
| 硬件失效中断 | 硬件掉电等                                             |

#### 中断和指令周期

中断给指令周期新增了一个阶段，那就是**中断阶段**，该阶段检查中断是否产生，并初始化中断处理器去处理中断。

此处 引入 **中断处理器**。其实就是个程序，当IO模块给处理器发送一个中断请求信号时，处理器会暂停当亲程序处理，转而处理服务于特定IO设备的程序(中断处理器)。

并发的基础我觉得就是中断机制，如果不能中断，那么10000条请求都得串行执行，那么得等到猴年马月。并发就是，10000条请求，当有一个请求进入了等待IO的过程中，马上中断并切换，完成后再切回来继续，那样效率就高多了。

中断其实也有开销，首先得把当前执行的任务中断时的状态压栈，然后中断处理器也需要额外的指令去确定中断的性值，再决定采用适当的操作。

如果IO操作只需要5ms，而压栈加初始化中断处理器去确认中断性质都花了10+ms，那么其实反而是降低了效率。但是一般的IO都远远超过压栈及初始化中断处理器等所花的时间。

**中断的处理**

硬件方面：

1、设备控制器或其他系统硬件产生1个中断

2、处理器结束当前指令的执行

3、处理器发送中断应答信号

4、处理器将PSW(程序状态字)和PC压入控制栈

5、根据中断，处理器加载新的PC值，即要处理的新的程序的PC值

软件方面：

6、保存剩余 的处理状态信息

7、处理中断

8、恢复处理状态信息

9、恢复老PSW和PC值

4-6其实是为了给转移控制权到中断设备而做的准备

**多个中断**

处理方案1是当出现第一个中断时，不允许后续的中断，坏处是当有紧急情况时，无法处理

方案2是，引入**优先级** ，为中断设定优先级，只有高的先被处理。

### 存储器的层次结构

![存储器层次结构](/img/memoryStruct.png)

这一节面向设计者多一些，有个知识点是 **访问的局部性原理**，即再执行程序期间，处理器的指令存储访问和数据存储访问呈现“簇”状，通俗的说就是当我访问一个字节，那么我接下来可能会频繁去访问该字节附近的字节。

所以当处理器去磁盘拿数据时，一次不会只拿那部分的目标数据，而会把那附近的一块都拿出来放到内存里面去。

### 高速缓冲存储器

其目标是使访问速度接近现有速度最快，就是要往接近处理器的速度靠。位于处理器与主存储器之间，使平时说的1，2级缓存吧。

上一节说的访问的局部性原理在这里传数据使也应用到了。

### I/O通信技术

包括三个知识点。可编程IO、中断驱动IO、直接存储器访问(DMA)

可编程IO：这玩意处理器通知他后，他完成了IO操作后并不主动通知处理器，还得处理器定期检查，所以花费了很多不必要的时间。

中断驱动IO:就是上一个的改进，就是他完成了IO操作后，主动发出中断信息告诉处理器，我活儿干完了，来验收吧。

直接存储器访问：就是处理器直接把整个任务打包交给DMA模块，之后就不管了，当整个任务完成之后，DMA才会通知处理器。即只有在开始和结束时处理器才参与。

DMA与中断驱动IO的区别就是，后者时，处理器会参与每一次IO指令。即一次任务，处理器参与了很多次，而DMA时，处理器只参与了2次。

所以会有DMA时直接硬盘与磁盘直接交互的错觉(外接硬盘给内置磁盘穿数据，通过USB)。

###  过程调用

就是函数调用其他函数的实现原理

#### 栈的实现

栈顶、栈底、栈指针

####  过程调用和返回

就是函数调用的具体实现就是压栈。

####  可重入过程

就是一段代码可以被多个用户共享使用

### 习题总结

#### 关键术语

| 术语            | 描述                                                         | 术语       | 描述                                                     |
| --------------- | ------------------------------------------------------------ | ---------- | -------------------------------------------------------- |
| 地址寄存器      | 存储下一条要运行的指令或者IO设备的地址。                     | 指令寄存器 | 存储最近取的一条指令                                     |
| 辅存            | 相对于主存，是磁盘？                                         | 主存       | 即掉电易失，RAM                                          |
| 栈              | 程序运行时被分配的一块内存区域，除去堆之外的内存即栈，栈的增加由高位到低位。 | 命中率     | 常用于主存？当去主存中取数据时，可以一次获取到的概率？   |
| 处理器          | 负责计算处理数据。                                           | 寄存器     | 速度较高的缓存，负责CPU与主存间的数据交换                |
| 时间局部性      |                                                              | 空间局部性 | 就是访问一个数据，通常后续会访问到其周围的数据           |
| 指令周期        | 取指令、执行指令、检查中断                                   | IO模块     | 负责计算机于对外进行数据通信的部件，如鼠标、键盘、显示器 |
| 中央处理单元CPU | 当只有一个处理器时，就叫CPU                                  |            |                                                          |
| 栈指针          | 指向栈顶                                                     | 条件码     | 处理器位计算结果设置的位                                 |
| 程序计数器PC    | 存储下一个指令的地址                                         | 系统总线   | 连接IO、存储器、处理器的数据通信桥梁                     |

索引寄存器、栈帧、中断、高速缓存存储器、可编程IO、IO、栈指针、主存、高速缓冲槽、可重入过程、多道程序设计、

#### 复习题

1. 列出并简要的定义计算机的四个主要组成部分。

   处理器、存储器、系统总线、IO模块

2. 定义处理器寄存器的两种主要类别？

   地址寄存器、缓冲寄存器。错

   其实是用户可见寄存器、控制和状态寄存器，其实我不是很懂，前者是用户可调用？后者只有高权限才可以调用？

3. 一般而言，一条机器指令能指定的四种不同操作是什么？

   1是在处理器与存储器中传送数据，2是处理器与IO模块间传送数据，3是数据处理，即运算，4是控制，即控制指令执行顺序。

4. 什么是中断

   就是使正在执行指令的处理器停止下来转而去处理造成停止的程序的情况。

5. 多中断的处理方式

   一是当接收一个中断后，禁止再允许中断；二是给所有中断排优先级

6. 内存层次的各个元素间的特征是什么

7. 什么是高速缓冲存储器

   处于主存与处理器之间，通常称之为一级缓存与二级缓存，速度比寄存器慢，但是比主存快。

8. 简述IO操作三种技术

   可编程IO、中断驱动IO、直接存储器访问DMA

9. 空间和时间局部性的区别

   空间局部性是指当访问储存中的某个数据时，接下来一段时间的访问很大概率是访问其周围空间的数据

   时间局部性是指当访问某个数据时，接下来访问该数据的时间或次数占比比较高

10. 开发空间和时间局部性的策略

    空间局部性策略：取数据时不止取目标数据，把它周围的那一块都取到缓存中。

    时间局部性策略：既然接下来该数据要被频繁使用，那么就将之副本放在处理器可以最快速取到的缓存中，比如寄存器或一级缓存。

#### 习题