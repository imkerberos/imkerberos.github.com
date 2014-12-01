---
layout: post
title: 在老的 Macbook467 上安装 Windows8.1 单系统 
tags: [macbook, windows]
published: true
catogory: 教程
---

我的 Macbook467 (macbook5,1 late 2008) 已经用了好多年了。期间花了好多银子进行改装，所以到现在它还在发挥余热。

以前在上面装了个 Win7 ThinPC 给家人用，但是前两天老婆装几个软件的时候被装上了很多流氓软件。(题外话：现在国内的软件可能算是流氓软件的天下了。比如，装个软件会给你捆绑某数字的卫士，下载个某度的云管家要先下载某度的下载软件再去自动下载某云管家等。) 又因为 Win7 ThinPC 用着不爽了，索性一怒之下干脆安装 Win8.1 了。
<!--more-->

安装期间遇到了 N 多问题，不过最终都解决了。写下来分享一下过程。

我的 Macbook467 的配置:

- 俗称: Macbook 467
- 官方名称: MacBook (13-inch, Aluminum, Late 2008)
- 型号代码: macbook5,1
- CPU: Intel Core 2 Duo 2.4G
- 内存: 2GB, 后来升级到了 4GB
- 硬盘: 500GB, 后来升级到了 Intel SSD 80GB
- 光驱: 拆了，换成硬盘位，后来升级成了 Intel SSD 160GB
- 显卡: nVidia GeForce 9400M ? 记不清了，貌似是这个。

- 现有系统: Windows7 Thin PC 32 位 （单系统）
- 目标系统: Windows 8.1 Update 64位 （单系统）

为什么选择 Windows8.1 呢，据说 Windows 8.1 比 Windows7 快，而且硬盘占用少。另外觉得 Windows7 长得丑..
macbook467 是 64 位的机器。另外根据调查资料说：BootCamp (驱动) 部分对于 Windows8 只支持 64 位。
所以干脆就用 64 位的 Windows8 了，虽然我个人不喜欢 64 位系统，但是为了能使用 4GB 的内存和 Windows8，还是不得不用 64 位的 Windows 了。

安装过程真的是好麻烦.. 先列一下遇到的问题:

1. 启动安装，由于光驱被 SSD 占用了，所以需要 U 盘启动。这个失败了无数次。
2. 安装驱动。BootCamp 4.0.xxx 版本。
3. 第一次用 Windows 8，很多不习惯的地方，进行修改和优化。
4. LCD 背光问题。
5. 其他系统优化，顺便推荐科学上网的工具。
6. 折腾 ....... 存储备份资料的 iPod Video 坏了，悲剧，九牛二虎之力才把资料找回来。

安装完未解决的问题:

1. 开机长时间白屏问题。
2. 有个协处理的驱动没有安装。

