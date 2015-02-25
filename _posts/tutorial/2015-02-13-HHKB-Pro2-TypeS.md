---
layout: post
title: HHKB Pro2 TypeS 入手
tags: [mac, 硬件]
published: true
catogory: 剁手
---

知道这个世界上有 HHKB 的时候, 立刻入手了一个.
不过还在习惯中. 因为我是 vi 党, 所以难度不是很大.

<!--more-->

![](/image/2015-02-13-HHKB-Pro2-TypeS/01.jpg)
![](/image/2015-02-13-HHKB-Pro2-TypeS/02.jpg)

HHKB 配合 Karambiner 还是比较好用的.

DIP 开关: 010001

Karabiner 映射:

- Command_R to Option_R
- Option_R to Control_R

我一般右方的 cmd 用得不多, 而且无论是 MAC 还是 HHKB 都没有右 Control 按键, 就移位了一下.
标准的 HHKB 按键的 Capslock 位置是 Control_L, Capslock 的功能由 `Fn+Tab` 实现, 还是有些
不方便, 而且由于刚开始用 HHKB, 手指头还是不自觉得去按 CapsLock 按键, 于是我用 Karabiner
把 Control_L 按键单独按的时候映射成了 `capslock`. karabiner 缺省不支持 Control_L 单独按下
的时候映射成 CapsLock 的, 不过自己定制一下就 OK 了.

打开 Karabiner 的 配置的 `Misc & Uninstall` 选项卡, 选择 `Open private.xml`, 在 Finder 中
会显示 Private.xml 的自定义配置文件, 修改这个文件, 改成如下内容

    <?xml version="1.0"?>
    <root>
    <item>
        <name>Control_L to Control_L</name>
        <appendix>(+ When you type Control_L only, send CapsLock)</appendix>
        <identifier>private.controlL2controlL_caps_lock</identifier>
        <autogen>__KeyOverlaidModifier__ KeyCode::CONTROL_L, KeyCode::CONTROL_L, KeyCode::CAPSLOCK</autogen>
    </item>
    </root>

退出 Karabiner 再重新启动, 在 `Change Key` 的选项卡中的第一项就出现 `Control_L` 映射成 Control_L + CapsLock 的选单了.


比较欣慰的一点是: HHKB 可以直接盖在 Macbook Pro retina 15 的键盘上, 不会压住键盘, 同时
还能使用触控板. :) 记得不要打开支撑脚, 而且下沿放到触控板上面.

![](/image/2015-02-13-HHKB-Pro2-TypeS/03.jpg)
![](/image/2015-02-13-HHKB-Pro2-TypeS/04.jpg)

