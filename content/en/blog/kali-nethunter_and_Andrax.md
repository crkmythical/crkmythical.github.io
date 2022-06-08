---
title: kali_nethunter and Andrax
encrypt: false
enc_pwd: askding
categories:
  - Kali

date: 2021-03-02 09:01:43
top:
tags:
layout:
---
编写进度
- [x] 


# Kali Nethunter
## 准备工作
- 安装android-plateform-tools `brew install android-platform-tools`
- 三个zip包
  - [TWRP for 10](https://dl.twrp.me/guacamole/）
  - [Magisk](https://github.com/topjohnwu/Magisk/releases/download/v21.4/Magisk-v21.4.zip)
  - [Disable-Dm-Verity_ForceEncrypt](https://zackptg5.com/downloads/Disable_Dm-Verity_ForceEncrypt_11.02.2020.zip)
可选
- [谷歌框架](https://www.huaweicentral.com/download-latest-google-play-store-application-apk/#google_vignette)
## 刷入

- 设置->系统->开发者模式界面，开启**OEM解锁**、**USB调试**两个个选项
- 手机链接电脑运行如下命令
```zsh
adb reboot bootloader                                       # 进入fastboot模式
fastboot oem unlock             						    # 解锁，手机出现解锁界面，用音量键选择UNLOCK THE BOOTLOADER，电源键确认
fastboot boot twrp-3.x.x.x.img                              # 刷入twrp镜像,等待几秒进入临时twrp模式
adb push twrp-3.x.x.zip /sdcard                             # 上传twrp的zip包，并刷入twrp镜像,
adb reboot recovery                                         # 重启到recovery模式
adb push Magisk-vx.zip                                      # 上传Magisk包并刷入
adb push Disable_Dm-Verify-ForceEncrypt_xx.zip /sdcard      # 解锁Data分区，上传Disable_Dm-Verity-ForceEncrypt并刷入
adb reboot                                                  # 重启，进行初始化设置
adb push nethunter-2021.1-oneplus7-oos-ten-kalifs-full.zip  # 上传kali nethunter镜像并刷入
adb reboot                                                  # 重启
```
 
![kali nethunter](../images/nethunter.jpg)


## 关闭系统更新提示
```zsh
adb shell pm disable-user com.oneplus.opbackup    # 屏蔽更新
adb shell pm clear com.oneplus.opbackup           # 清除通知
adb shell enable com.oneplus.opbackup             # 恢复更新
```

## 升级系统保留ROOT权限操作
  
[一加7T如何保留ROOT OTA升级系统](https://cyhour.com/1233/)
[一加7 OTA升级保留ROOT](https://koonchung.github.io/post/2020-05-07/)



# [Andrax](https://andrax.thecrackertechnology.com/)
[Andrax文档](https://andrax.thecrackertechnology.com/documentation/)

[Andrax仓库](https://gitlab.com/crk-mythical/andrax-hackers-platform-v5-2)


下载[Andrax-mobile.zip](https://download2347.mediafire.com/dj83upef4o5g/xvxo6hw9i99kdcn/ANDRAX-Mobile.zip)
手机连接电脑输入
```zsh
unzip ANDRAX-Mobile.zip 
adb push andraxcorev6001.tar.xz.cpt /sdcard/Download
adb install ANDRAXMobile6001.apk
adb shell pm list packages  # 查看所有包
adb shell dumpsys activity  com.thecrackertechnology.andrax  # 获取指定包的Activity
adb shell am start -n com.thecrackertechnology.andrax/com.thecrackertechnology.dragonterminal.ui.term.NeoTermActivity  # 点击”OK“
```

root和andrax默认密码`andrax`
![Andrax](../images/andrax-1.jpg)

![Andrax](../images/andrax-2.jpg)

## SSH
```zsh
sudo service --status-all
sudo service ssh start
```

## VNC
```zsh
sudo service vnc start
vnc-viewer 192.168.43.73:5901
```

## Bluetooth Hacking
1. open Andrax Terminal with Recovery shell
```zsh
su
cd /;cat *.rc | grep -i "bluetooth" | grep -i "net_bt_stack" | grep -i "\/dev"  # 定位串口
```
输出
	chown bluetooth net_bt_stack **/dev/ttyS0**

2. open Andrax Terminal
```zsh
sudo stty -F /dev/ttyS0  # 查看串口速率
sudo hciattach -s 115200 /dev/ttyS0 any 3000000   # 创建接口
sudo hciconfig hci0 up  # 激活接口
``` 
















参考
[一加7基于安卓10版本安装NetHunter](https://www.zerozd.xyz/Android/67.html)
[一加7Pro把玩Nethunter](https://www.blackh4t.org/oneplus7pro-kali-nethunter/)
[就砖](https://www.oneplusbbs.com/thread-4446250-1.html)
[TWRP备份与还原操作](https://www.oneplusbbs.com/forum.php?mod=viewthread&tid=4065238)
[TWRP备份与还原操作2](https://www.xiaoyuanjiu.com/9093.html)

[Andrax和nethunter介绍](https://www.anquanke.com/post/id/223493)
