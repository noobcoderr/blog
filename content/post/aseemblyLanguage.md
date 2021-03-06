---
title: "汇编语言的简单学习与入门"
date: 2019-09-07T23:39:43+08:00
lastmod: 2019-09-010
subtitle:    "汇编？超底层的！"
description: "什么是汇编？为什么要用汇编？优缺点？"
author:      Jzhang
image:       "img/theme/go1.png"
tags:        ["汇编"]
categories:  ["Tech"]
---

# 汇编语言的简单学习与入门

## 什么是汇编语言(什么是汇编)

### 我的最初理解

学一个新的知识，先别着急直接看，可以先说说自己的理解，疑问，然后带着自己的理解与疑问再去展开学习，那么学习的过程会更有意思。

在我的知识储备里，我对汇编语言的认识就只有计算机可以直接识别的底层语言，为什么可以被直接识别的原因好像是其标识符其实就是又固定的二进制含义，比如add就直接代表加操作的二进制形式(00000011)。在大学期间学习微机原理CortexM3时学到过，印象中是其语法变量或操作符是两-三个字母组成的单词 每行代码也只有3，4个词，好像是可以操作寄存器 (mov  xxx xxx之类)。由于其课程晦涩难懂，所以当时也没认真听讲，现在不得不来补当时落下的技术债务了。

### 教程介绍

直接 **描述/控制** CPU运行

CPU，只负责计算，不具备逻辑，它只识别二进制的操作码(add 00000011)、指令，运行完就等待

编译器，将用高级语言写好的逻辑代码翻译成CPU可以识别的操作码

程序员：二进制代码(不可读，难理解) -> 汇编代码 -> 高级语言

汇编语言：二进制指令的文本形式，一一对应，如 加法指令00000011 写成汇编就是ADD，只要还原成二进制，就可以被 CPU执行，所以汇编是最底层的语言

每种CPU的机器指令不一样。则对应的汇编语言也不一样。

### 知识储备 寄存器

可以说是最快的CPU缓存了吧

CPU不存数据，只进行运算，那么数据存哪儿呢？我们常听说的计算机存储设备有硬盘、闪存、内存、缓存等等其实就是ROM，RAM，Cache，Register。()

ROM是常说的硬盘，如笔记本的动辄1T机械硬盘，手机宣传的128G大存储空间，特点是一次写入，多次读取，但是读取速度慢读写速度更快，6+128的6就是内存，其特点是掉电失忆，存储速度快。

Cache，常说的一级和二级缓存，该缓存比内存。

Register，寄存器。缓存存储地址为随机，每次读取还得先寻址，而且速度也还达不到cpu的要求，所以寄存器出现了，它用来存储最常用的数据，如循环变量。CPU读取寄存器，在有寄存器跟内存交换数据。

寄存器不依赖地址区分数据，而是用名字区分，直接让cpu去某个寄存器取数据这样速度最快，因此有人将寄存器比作零级缓存。

![img](/img/assemblyLang/cpu_register.png)

#### 寄存器种类

通用寄存器和特殊寄存器

通用寄存器包括：EAX,EBX,ECX,EDX,EDI,ESI,EBP等

特殊寄存器：ESP。用于保存当前stack地址

目前寄存器已扩展到100多个，都是通用寄存器。

常说的32/64位CPU，是指寄存器大小。32位CPU寄存器大小位4字节



### 知识储备 内存模型

内存模型，就是研究内存如何存储数据。

#### Heap  堆

程序运行时，OS给其分配一段内存，用于存储程序代码以及产生的数据。这段内存有起始地址(0x1000)和结束地址(0x8000)。

![img](/img/assemblyLang/romModel.png)

程序运行过程种，对于动态的内存占用请求(新建对象，malloc)，系统会从预先分配好的那段内存中，从起始位置开始，分配给用户所需要的内存大小。

这种由用户主动请求而划分出来的内存区域，叫做Heap，也叫堆，从低位开始，向高位增长。

Heap的特点是不会自动消失，需要手动释放或者等待垃圾回收机制来处理。

![img](/img/assemblyLang/Heap.png)

#### Stack  栈

Stack是由于函数运行而临时占用的内存，除了Heap外，其余内存都是栈

![img](/img/assemblyLang/stack.png)

举例：

```c
int main() {
   int a = 2;
   int b = 3;
}
```

当执行main函数时，系统会位它在内存中建立以个帧(frame)，main函数内部的变量a，b都保存在内。main函数结束后，帧被收回，释放所有内部变量，不再占用空间。

![img](/img/assemblyLang/stack1main.png)

如果main调用了其他函数

```c
int main() {
   int a = 2;
   int b = 3;
   return add_a_and_b(a, b);
}
```

当执行到return语句时，系统会位该函数建立一个帧，该函数内部变量存在该帧内，当该函数结束，那么这帧被收回，函数会回到main被中止的地方，继续往下。可以说由多少层函数调用，就有多少帧，通过该机制实现函数层层调用，每一层都有自己的变量。

![img](/img/assemblyLang/stack2call.png)

所有的帧都存放在 Stack，由于帧是一层层叠加的，所以 Stack 叫做栈。生成新的帧，叫做"入栈"，英文是 push；栈的回收叫做"出栈"，英文是 pop。Stack 的特点就是，最晚入栈的帧最早出栈（因为最内层的函数调用，最先结束运行），这就叫做"后进先出"的数据结构。每一次函数执行结束，就自动释放一个帧，所有函数执行结束，整个 Stack 就都释放了。

![img](/img/assemblyLang/stack3push.jpg)

![img](/img/assemblyLang/stack3pop.jpg)

stack时由高位向低位分配内存。

### 一个示例

```clike
int add_a_and_b(int a, int b) {
   return a + b;
}

int main() {
   return add_a_and_b(2, 3);
}
```

`gcc -S example.c` 将其转为汇编得到 `example.s`，简化后得到

```assembly
_add_a_and_b:
   push   %ebx
   mov    %eax, [%esp+8] 
   mov    %ebx, [%esp+12]
   add    %eax, %ebx 
   pop    %ebx 
   ret  

_main:
   push   3
   push   2
   call   _add_a_and_b 
   add    %esp, 8
   ret
```

每条语句组成为： `CPU指令    运算子0,1,2,3...`，

push  入栈

call  调用函数

mov  将一个值写入寄存器，存入的时第一个运算子内

add  用于将2个运算子相加，并写入第一个运算子

pop  取出Stack最近一个写入的值，即最低位。

ret   终止当前函数执行，并将运行权交给上层函数，当前函数的帧被回收。



该文章基本复刻了阮老师的入门教程，因为平时没多少时间再去实践，所以想着自己手打一遍以加深印象。

## 为什么要引入汇编语言(来历)

初代程序员是手写二进制指令的，为了解决可读性问题，就给每个操作取了个对应的操作符名称。00000011用ADD表示，汇编语言就是这些操作符名称的集合，从此之后，程序员用汇编语言进行开发，开发完用assembler翻译(assembling)成二进制二进制指令。

## 如何使用汇编语言

## 何时使用汇编语言

## 何地使用汇编语言

经验收获：我们在介绍一个知识点A时，难免会依赖一些其他的知识点B，而通常我们喜欢顺便介绍一下依赖的知识点，这就造成了一篇介绍知识点A的文章里，介绍了BCDEF....，这其实非常不好，因为增加了知识量，让读者容易搞混或者根本不知道哪些才是A的重点(比如我在介绍寄存器时把ROM,RAM,CASHE都介绍了一遍)。所以以后我写文章或者说的时候，要搞清楚该次的重点。依赖的其他知识先略过，放最后再讲。





## 参考文章

* [汇编语言入门教程-阮一峰](http://www.ruanyifeng.com/blog/2018/01/assembly-language-primer.html)