# 飞利浦E289的一些折腾

#### 目录
- [解锁 bootloader](#00-unlock-bootloader)
- [提取固件](#01-dump-rom)


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
