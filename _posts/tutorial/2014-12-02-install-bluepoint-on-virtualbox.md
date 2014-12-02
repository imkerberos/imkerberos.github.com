---
layout: post
title: VirtualBox 上安装 蓝点 Linux 2.0
tags: [linux, 虚拟机]
published: true
catogory: 教程
---

这两天折腾 Win8 的时候翻出来了当年 蓝点Linux 2.0 的光盘。想当初一屋子的 2.0 的光盘没卖出去，
我就拿到家里了一套 2.0，应该还有一套`蓝点Linux 1.0` + `FreeBSD` 的 "双响炮" 的，一直没找到，
现在网上也没有下载的。现在回想起来，应该拿几套豪华版，留作纪念也好。

<!--more-->

正好机器上有 Virtual Box，就装了一下重温当年的时光。:)

![登陆进去](/image/2014-12-02-install-bluepoint-on-virtualbox/login.png)
![控制台中文输入](/image/2014-12-02-install-bluepoint-on-virtualbox/console.png)
![KDE中文输入](/image/2014-12-02-install-bluepoint-on-virtualbox/xwindow.png)

说下怎么安装吧:)

## 安装

蓝点Linux2.0是老的版本了，Linux 的内核还是  2.2 的，很多硬件不支持。安装的时候不要用`图形安装`
要选`文本界面安装`。安装过程中关于`XWindow`部分的配置一律不要检测或者运行，否则屏幕会花掉。在
选择安装软件的时候选择全部安装。安装完成以后，首先修改 grub 的启动菜单，开启 `VESA framebuffer` (缺省是 VGA16 模式的)。

## 配置 FrameBuffer
```
    #vi /boot/grub/menu.lst
```

在第一个选单处修改 VGA 模式

```
    kernel /vmlinuz-2.2.16 vga=auto root=/dev/hda5 ro chinese=gb nosmp video=vesa:ypan video-atyfb:off video=matrox:off video=pm2fb:off
```

修改成

```
    kernel /vmlinuz-2.2.16 vga=788 root=/dev/hda5 ro chinese=gb nosmp video=vesa:ypan video-atyfb:off video=matrox:off video=pm2fb:off
```

`vga=788` 是配置 VESA FB 的模式为 800x600 16bpp。

重新启动。能看到屏幕左上角的小企鹅图片，屏幕大小也变成了 800x600。

## 安装 XFree86 的 FBDev 驱动

把 XFree86_FBDev 做成缺省的 X 服务器。

```
    # cd /etc/X11
    # ls -l X
    lrwxrwxrws 1  root  root   25 Dec  1   09:12 X -> /usr/X11R6/bin/XF86_FBDev   
```

如果 X 不是指向 XF86_FBDev 的话，需要手动修改

```
    # rm X
    # ln -s /usr/X11R6/bin/XF86_FBDev X
```

修改 `/etc/X11/XF86Config` 文件，用下面的文件覆盖即可:

```
```
