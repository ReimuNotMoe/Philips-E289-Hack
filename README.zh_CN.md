# 飞利浦E289的一些折腾

#### 目录
- [解锁 bootloader](#00-unlock-bootloader)
- [提取固件](#01-dump-rom)
- [root](#02-root)


<a name="00-unlock-bootloader"/>

## 解锁 bootloader

**警告：此操作会清除手机中所有的用户数据。**

首先，我们使用安卓系统通用的方式开启开发者选项。

进入 **系统设置 -> 系统 -> 关于手机**，然后选中 **版本号** 并快速地按 DPAD Center。

![](https://reimunotmoe.github.io/images/Philips-E289-Hack/Screenshot_20180101-081036.png)

返回，找到 **开发者选项**，打开这个 **OEM 解锁**。

![](https://reimunotmoe.github.io/images/Philips-E289-Hack/Screenshot_20180101-081055.png)

并且开启 adb 调试。

把手机连接上电脑，运行 `adb reboot bootloader`。

此时手机会进入 fastboot 状态，但是屏幕是白色的，你无法看到任何信息。

P.S：进 recovery 屏幕也无显示，我猜是他们故意的，也可能是这两者没有这个屏幕的驱动。

然后，在电脑上运行 `fastboot oem unlock`。在终端里出现省略号时，**同时**按**一下**手机的 **音量加** 和 **音量减**。

```
$ fastboot oem unlock
...
```

然后手机的 bootloader 就被成功解锁了。

```
(bootloader) Start unlock flow

OKAY [  4.236s]
finished. total time: 4.236s
```

<a name="01-dump-rom"/>

## 提取固件

首先，请自行寻找并安装最新版的 MTK SP Flash Tool 及其 SP Driver（Linux 系统无需 SP Driver）。

打开 SP Flash Tool，并载入 [Scatter 文件](https://github.com/ReimuNotMoe/Philips-E289-Hack/blob/master/MT6739_Android_scatter.txt) 。

点击 **Readback** 标签，然后点击 **Add**，并双击列表中新出现的条目。

软件会让你先选择 ROM dump 的位置，选好后点击保存。

在出来的对话框里按如下设置，并点击 **OK**：

```
Type:           Decimal
Region:         EMMC_USER
Start Address:  0
Length:         3917479936
```

![](https://raw.githubusercontent.com/ReimuNotMoe/ReimuNotMoe.github.io/master/images/Philips-E289-Hack/Screenshot_20190201_202725.png)

然后点击 **Read Back**，并将手机**关机**，5秒钟后插上电脑。

等下面的进度条走完即可。

提取出来的东西是 EMMC 的完整镜像，有 GPT 分区表，在 Linux 下可直接用 `losetup -P /dev/loopX /path/to/image` 载入。

请妥善保管此镜像，一旦你后面折腾坏了还可以靠它救命。

<a name="02-root"/>

## root

这个就很简单了，把 EMMC 镜像里的 boot 分区提取出来，让 Magisk Manager 去 patch 一下 boot image，再 fastboot flash boot 就好了。

