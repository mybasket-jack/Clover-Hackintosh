# Clover-Hackintosh
记录一波自己对宏基**F5-572G**和神州**Z6-SL7R3**的吃果过程,分享相关配置文件和解决过程,实现了亮度，声音，网卡、WIFI、蓝牙、USB正常使用
相关学习网站：
[黑果小兵](https://blog.daliansky.net/)
[MAC 10.14 EFI文件的修改过程](https://www.jianshu.com/p/81e329c50120)

# 制作USB启动盘
网上下载好镜像后，利用 [balenaEtcher](https://www.balena.io/etcher/)
制作USB启动盘
![balenaEtcher.png](https://upload-images.jianshu.io/upload_images/3944205-964066f62b27e18e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
# Clover 
- *[Clover更新地址](https://sourceforge.net/projects/cloverefiboot/)* Clover团队更新 clover的主要发布渠道
- 相关clover列表 【[clover-efi](https://github.com/tsingui/clover-efi)】

- 自己重新配置clover,或者在github上找自己机型的clover

- 相关目录结构
  - ACPI\ORIGIN： 保存提取到的原始DSDT文件，(clover下F4)。
  - ACPI\PATCHED： 存放修改好的用户DSDT.aml和SSDT.aml 。
  - CLOVERX64.efi： 64位CLOVER的主启动文件 。
  - config.plist： CLOVER配置文件。
  - DOC: CLOVER的帮助文档 。
  - DRIVERS64UEFI： 使用UEFI模式加载64位CLOVER所需要的驱动文件。
  - KEXTS： 使用驱动注入时，CLOVER加载的驱动文件的存放位置。
  - MISC： 存放CLOVER环境下的截图文件。
  - OEM: 分文件夹存放ACPI、config.plist等文件。用以加载，实现单个U盘引导多个黑苹果系统。
  - ROM: 保存提取到的显卡ROM文件。
  - THEMES： CLOVER主题存放位置。
  - TOOLS： EFI Shell存放位置，放置用于进入shell环境的.efi文件，不可用于引导OSX，但可运行一些.efi程序。
# 驱动
##启动必备
- ACPIBatteryManager.kext：笔记本电池驱动。
- FakeSMC_v1800.kext：模拟苹果SMC芯片及加密通讯，欺骗MAC系统以为是白苹果设备，为黑苹果必备驱动。
- Lilu_v1.3.2.kext：黑苹果内核扩展补丁驱动。
- WhateverGreen_v1.2.7.kext：
显卡驱动补丁集
- RealtekRTL8111.kext：RTL8168X/8111X（X=无/B/C/D/E/F/G）网卡驱动。
- VoodooPS2Controller_1.9.2.kext：PS2接口驱动，使用USB接口键鼠可以删除。
但本人电脑触控板模拟PS2鼠标，所以保留，
- VoodooI2C.kext：触摸板驱动教程[VoodooI2C说明](https://heipingguowu.top/2019/09/05/2331/)

##其他驱动
- GenericUSBXHCI.kext：
USB3.0驱动
- AppleALC_v1.3.5.kext：仿冒声卡驱动。
- AppleBacklightInjector.kext：笔记本显示屏亮度调节驱动。
- BrcmFirmwareData.kext：蓝牙驱动。
- BrcmPatchRAM2.kext：蓝牙驱动。
FakePCIID.kext：黑苹果必备的一个驱动文件，由于macOS系统会对PCI device-id进行验证，导致黑苹果的硬件不能通过这一验证，所以需要就需要这个PCIID文件屏蔽验证。
- FakePCIID_Broadcom_WiFi.kext：博通WIFI驱动，注：本机无线网卡BCM94352z。
- HibernationFixup.kext：睡眠唤醒补丁。
- CodecCommander.kext：解决睡眠唤醒后声卡无音补丁。
- CPUFriend.kext：动态注入CPU电源管理数据，以实现变频。
# pist配置文件
[配置clover](https://www.jianshu.com/p/2ad57fca5969)

## 网卡
- 利用无线USB，[驱动下载]( https://github.com/chris1111/Wireless-USB-Adapter)
- 替换网卡为博通无线网卡**BCM94352z**，并加入网卡驱动*FakePCIID_Broadcom_WiFi.kext*
## 声卡
加入AppleALC.kext，查看声卡ID，并注入电脑的声卡ID(不断尝试)

     ALC269 常用 28，29，14
     ALC256 常用17
## 亮度
-
## HIDIP
[https://jack006.com/index.php/archives/537/](https://jack006.com/index.php/archives/537/)

