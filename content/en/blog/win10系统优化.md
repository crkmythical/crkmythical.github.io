---
title: win10系统优化
encrypt: false
enc_pwd: askding
categories:
  - Evolution
tags:
  - null
date: 2021-03-30 13:09:36
top:
layout:
---
编写进度
- [x] 

## 开启卓越性能
win+r键输入`powershell`
```powershell
powercfg -duplicatescheme  e9a42b02-d5df-448d-aa00-03f14749eb61
```
【win+r】键输入`control` 查看方式改为【大图标】，选择【电源选项】-【卓越性能】
	【选择电源按钮的功能】-【更改当前不可用的设置】-勾选【启用快速启动】-【保存修改】

## 存储感知清理
	打开【设置】-【存储】-【配置存储感知并立即运行】

	
## 关闭视觉效果
【win+r】运行sysdm.cpl打开【系统属性】面板-【高级系统设置】-标签页【高级】-性能【设置】-【视觉效果】-【最佳性能】

## 解除网络限速
【win+r】输入`gpedit.msc`,定位到一下路径:
【计算机配置】-【管理模版】-【网络】-【QoS数据包计划程序】-《限制可保留带宽》-勾选【已启用】，修改【带宽限制】： 0


## 关闭Defender和实时防护
【win+r】输入`gpedit.msc`,定位到一下路径:
【计算机配置】-【管理模版】-【windows组件】-【windows Defender 防病毒】:
	          1.  启用【关闭Microsoft Defender防病毒】
			  2. 【实时防护】-启用【关闭实时防护】

## 关闭win10自动更新
【计算机配置】-【管理模版】-【Windows更新】-禁用【配置自动更新】

## 磁盘优化
右键单击【C盘】-选择【属性】-标签页【工具】-对驱动器进行优化和碎片清理【优化】-【更改设置】


## 关闭UAC通知并优化启动引导
【win+r】输入`msconfig`-标签页【工具】-【更改UAC设置】-「从不通知」
                      -标签页【引导】-勾选【无GUI引导】-应用并确定

## 禁用相关服务
【win+r】键输入`services.msc` ，查找以下服务并禁用
  - 【Windows Search】
  - 【Connected User Experiences and Telemetry】
  - 【Diagnostic Execution Service】
  - 【Diagnostic Policy Service】
  - 【Diagnostic Service Host】
  - 【Diagnostic System Host】
  - 【HomeGroup Listener】
  - 【HomeGroupProvider】
  - 【SecurityCenter】
  - 【Program CompatibilityAssistant Service】

## 禁用P2P分享服务
【win+i】键单击【更新与安全】-【传递优化】-关闭【允许从其他电脑下载】

## 关闭休眠功能，保留睡眠功能
【win+x】选择命令行<admin>,执行`powercfg -h off`,重启后生效
