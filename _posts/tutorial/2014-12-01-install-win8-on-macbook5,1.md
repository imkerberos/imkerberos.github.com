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

## 我的 Macbook467 的配置:

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

### 安装过程真的是好麻烦, 先列一下遇到的问题:

1. 启动安装，由于光驱被 SSD 占用了，所以需要 U 盘启动。这个失败了无数次。
2. 安装驱动。BootCamp 4.0.xxx 版本。
3. 第一次用 Windows 8，很多不习惯的地方，进行修改和优化。
4. LCD 背光问题。
5. 其他系统优化，顺便推荐科学上网的工具。
6. 折腾 ....... 存储备份资料的 iPod Video 坏了，悲剧，九牛二虎之力才把资料找回来。

### 安装完未解决的问题:

1. 开机长时间白屏问题。
2. 有个协处理的驱动没有安装。

## 安装过程:

### 启动 Windows 安装盘

由于光驱已经被 SSD 占用了，所以需要使用外部启动，真的是懒得再拆开换上光驱了。不过据说有些早期的 MacBook Pro 不支持外置光驱安装 Windows。我好像遇到过一台，记得当时使用外置光驱都无法启动 Windows7 安装盘，只好拆开机器把光驱换上去，安装完 Windows 以后再把第二块硬盘换上去，那个折腾啊 ...

安装源盘: 我使用了 

为了省事，决定使用 U 盘启动，事后才后悔，这真是一个傻到底的决定。首先是制作 U 盘启动盘，失败过程如下：
因为 MB467 是 Win7 ThinPC 单系统，所以没法用 BootCamp 制作启动 U 盘，所以在另外我得 MBP 15 Retina (10.9) 上制作 U 盘启动盘。到下载驱动的步骤 Cancel 掉操作 (因为这个过程是下载 MBP 的驱动，对 MB467 来说没有用处)。在 MBP 15 Retina 上测试 U 盘能否启动，reboot, 按住 Option， 成功出现了 `Windows` 和 `EFI` 两个额外的启动选项，选择 `Windows`，成功进入了  `Windows 8.1` 的安装界面。再把U盘插到 MB467 上，按住 Option 键启动，悲剧，居然只有 `EFI` 选项。选择 `EFI` 之后，U 盘灯长亮，几分钟以后没反应了，没启动.....

好吧，看来不行，难道在 10.9 上制作的 EFI 引导文件有问题，于是我到网上搜索资料，找到一篇帖子，是修改 `BootCamp` 制作 U 盘的，网址在这里[Windows 8 on old MacBook](http://blog.mdstn.com/windows-8-on-old-macbook/): 。重新制作 U 盘，启动，还是老样子，只有一个无法启动的 `EFI` 额外启动选项。贴一下脚本:

```
    BACKUP_DIR="." # Where the backup of "Boot Camp Assistant" exist  
    UTILITIES_DIR="/Applications/Utilities"  
    TEMP_DIR="/tmp"

    PLIST="Info.plist"

    APP="Boot Camp Assistant.app" # (editable) The name of "Boot Camp Assistant"  
    APP_CONTENTS="$APP/Contents"  
    APP_PLIST="$APP_CONTENTS/$PLIST"

    ModelIdentifierMajor="MacBook5" # (editable) Model Name with a version major number and without minor number (not: 5,1; but: 5)  
    ModelIdentifier="MacBook5,1" # (editable) Model Identifier  
    k="MB51.007D.B03" # (editable) Boot ROM Version

    echo "$APP_PLIST"

# cp "Boot Camp Assistant" to temporary dir
    if [ -d "$TEMP_DIR/$APP" ]  
    then  
        rm -rf "$TEMP_DIR/$APP"
    fi  
    cp -R "$BACKUP_DIR/$APP" "$TEMP_DIR"

# read out the plist
    defaults read "$TEMP_DIR/$APP_PLIST"

# edits
# defaults write "$TEMP_DIR/$APP_PLIST" DARequiredROMVersions -array-add "$BootROMVersion"
# defaults write "$TEMP_DIR/$APP_PLIST" PreESDRequiredModels -array-add $ModelIdentifierMajor
# defaults write "$TEMP_DIR/$APP_PLIST" PreUEFIModels -array-add $ModelIdentifierMajor
# defaults write "$TEMP_DIR/$APP_PLIST" PreUSBBootSupportedModels -array-add "$ModelIdentifier"
# defaults write "$TEMP_DIR/$APP_PLIST" Win7OnlyModels -array-add "$ModelIdentifier"
# defaults rename "$TEMP_DIR/$APP_PLIST" PreUSBBootSupportedModels USBBootSupportedModels

    defaults write "$TEMP_DIR/$APP_PLIST" DARequiredROMVersions -array-add "$BootROMVersion"  
    defaults write "$TEMP_DIR/$APP_PLIST" PreESDRequiredModels -array-add $ModelIdentifierMajor  
    defaults write "$TEMP_DIR/$APP_PLIST" PreUEFIModels -array-add $ModelIdentifierMajor  
    defaults write "$TEMP_DIR/$APP_PLIST" PreUSBBootSupportedModels -array "$ModelIdentifier" "MacBookAir3,2" "MacBookPro8,3" "MacPro5,1" "Macmini4,1" "iMac12,2"  
    defaults rename "$TEMP_DIR/$APP_PLIST" PreUSBBootSupportedModels USBBootSupportedModels  
    defaults delete "$TEMP_DIR/$APP_PLIST" Win7OnlyModels

# sudo codesign
    sudo codesign -fs - "$TEMP_DIR/$APP"

# Kill Boot Camp Assistant if it's running before replacing the modified
    ps aux | grep Boot\ Camp | grep -v grep | awk '{print $2}' | xargs kill -9  
# replace "Boot Camp Assistemp" from Utilities folder
    sudo rm -rf "$UTILITIES_DIR/$APP"  
    sudo mv "$TEMP_DIR/$APP" "$UTILITIES_DIR"

# read out the "Boot Camp Assistant" plist from Utilities folder
    sudo defaults read "$UTILITIES_DIR/$APP_PLIST"

    sudo open "$UTILITIES_DIR/$APP"  
```

时候猜想：可能因为我是在我的 MBP 15 Retina 上做的启动盘，先在 MB467 上安装 OS X 10.8.3 再用上面的脚本制作 U 盘启动盘可能好用，（上面的脚本是修改 BootCamp，使之可以在 MB467 上制作 Windows8  启动盘），有试过的同学可以告诉我结果。

再换一个方法吧，直接用 Windows 制作 U 盘启动盘吧，还好我得 MBP Retina 里面还有虚拟机。用 `Windows 7 USB/DVD Download Tool1` 做了一个启动盘，照样失败.....

再换一个方法，根据这个帖子号称是可以的: [How To: Install Windows 7 Or Windows 8 From USB Drive \[Detailed 100% Working Guide\]](http://www.intowindows.com/how-to-install-windows-7vista-from-usb-drive-detailed-100-working-guide/)，失败....

奇怪的是，这些方式在 MBP 15 Retina 上都好用。

没办法了，只好做光盘启动了，由于白盘和外置光驱没在手头，下午拿到之后继续。

### 不得不用外置光驱

下午拿到光驱和白盘，刻录了 Windows 8.1 VL 的启动盘，顺利启动把 Windows 8.1 安装到了 MB467 上，接下来就是安装驱动了。

### 安装驱动

新的 BootCamp 版本 (5.x) 不支持老的 MacBook，只好用 4.0.4033 的，但是在 MB467 上，直接安装是无法通过版本检查的，安装程序一直报错 `Boot Camp x64 is unsupported on this computer model`。好办，需要一点技巧就是了：

1. 不要使用 BootCamp 4.0.4033 的 setup 程序安装。
2. *使用管理员权限*打开命令窗口，运行: bootcamp64.msi

这样就顺利安装上 BootCamp 了。

在 BootCamp 安装上之前，蓝牙驱动没安装，无法使用 `Magic Mouse`，触控板又是没有右键功能的，又悲剧了，很多操作没有右键不知道怎么开启，悲催的 `Magic Mouse`.还好旁边机器上有个 USB 鼠标，偷过来用一下。:)

### Windows 8.1 升级和打补丁

安装上 Windows 的第一件事情就是 Update了，经过半个小时的安装。终于搞定，咦？显示器怎么亮度这么高？原来是更新到了 nVidia 的驱动以后，Fn + F1 控制 LCD 背光不好用了。打开 BootCamp 控制面板调整亮度，还是没反应，又悲剧了，100% 亮度会亮瞎我的狗眼的 ...

#### 背光亮度调节


## Windows 8 太 .....

我用 Windows 8 不超过两天。操作太 SB 了. 赶紧看看怎么能改成传统 Windows 的操作（家里其他人用 Win8 会疯的，真的不会啊...）

1. Start8 恢复成 `开始菜单`。
2. 删除 `微软拼音输入法`。我用 `紫光华宇输入法`，用了 10+ 年了。
3. 怎么？输入法切换怎么成了 `CMD + Space`，虽然这样跟 MAC 一致了，但是 ...  还是很不习惯好吧。怎么修改？
4. SendKeyForIME 需要安装 .NET Framework 3.5，郁闷，还好 Win8 带了，需要打开。
5. 科学上网， X GFW。

## 科学上网 `X-Wall`
