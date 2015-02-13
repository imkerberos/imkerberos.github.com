---
layout: post
title: Haiku/BeOS 中的 Universal Binary
tags: [haiku, kernel]
published: true
catogory: Haiku
---

Haiku OS 中的 FatELF 实现

## 什么是 ELF

`ELF` 是 `Executable and Linkable Format` 的简写。是操作系统的一种可执行或者可连接的文件格式。
几乎所有的 Linux 都使用 ELF 文件格式来组织可执行文件，动态库文件或者开发过程中生成的链接文件。
与 `ELF` 类似的是 `Windows` 操作系统上使用的 `PE` 文件格式以及 `Mac OS X` 和 `iOS` 上使用的
`Match-O` 格式。

<!--more-->

## 什么是 Universal Binary

熟悉 iOS 或者 Mac 的同学应该都有这个概念。所谓 Universal Binary 就是多种 CPU 体系或者指令集的
可执行文件打包成一个统一的文件。这样做的好处在于对于最终的用户来说，下载程序不用再区分什么 32
位版本或者64位版本, 直接下载一个程序的 Universal 版本即可在自己的系统上运行。要知道，大部分的
最终用户都是小白的，根本不知道什么是 32 位或者 64 位，又或者不知道下载的 Mac 程序是 Intel 版本
的还是 PowerPC 版本的。当年 Apple 的机器都是使用 PowerPC 处理器的，后来切换到了 Intel 的处理器
上，但是这有个过渡的过程。无论最终用户使用的是 PowerPC 的系统还是 Intel 的系统，借助 Universal
Binary 技术，用户只需要下载一套程序就可以在自己的机器上运行，而不用关心自己的机器是 PowerPC 的
还是 Intel 的。

Universal Binary 的原理很简单：实际上的实现是多个程序文件打包成了一个。比如：PowerPC 和 Intel
机器的例子，开发者只需要分别制作能在 PowerPC 和 Intel 上运行的两个程序文件，最后打包成一个即可。

## 什么是 FatELF

基于上面的介绍，我们知道 ELF 是一种可执行文件格式。如果我们把多种 CPU 架构的 ELF 可执行文件打包
成一个文件，不就也成为 Universal Binary 文件了么？ 答案是肯定的，只需要我们设计一种文件格式包含
多种 `ELF` 文件，并且分别记录这些 `ELF` 文件可运行的条件即可。

已经有人设计出了这种文件，这就是 `FatELF`。[FatELF: Universal Binaries for Linux.](http://icculus.org/fatelf/)

## 实现 FatELF 需要哪些工作

本身设计这种文件格式是一件比较轻松的事情，复杂的事情在于：

- 如何让操作系统能够`认识`这种文件。
- 如何让程序员编译出这种可执行文件。

Landonf 通过对 `Haiku` 的内核部分打补丁可以支持这种文件。

- 对 bootloader 打补丁，使之能正确 `选择` 要启动的内核。
- 对内核打补丁，使之能支持混杂模式的内存管理。
- 对 image loader (*不是*图片加载器) 打补丁，使之能 `选择` 合适的可执行文件 或者动态库文件。
- 对 GCC/Binutils 打补丁，使之能产生 FatELF 的可执行文件或者库文件。

## 参考资料

- [FatELF](http://icculus.org/fatelf)
- [FatELF Toolchain Drivers](http://landonf.bikemonkey.org/2012/12/index.html)

