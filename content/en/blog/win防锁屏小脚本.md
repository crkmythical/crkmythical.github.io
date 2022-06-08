---
title: win防锁屏小脚本
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-02-18 19:00:36
top:
tags:
layout:
---
编写进度
- [x] 



## 防锁屏.vbe

```vbscript
Dim wshShell
Set wshShell = CreateObject("Wscript.Shell")
msgbox "µã»÷È·¶¨ºó£¬¿ªÊ¼¶¨Ê±Ä£Äâ¼üÅÌÊäÈë£¬ÒÔ·ÀÖ¹ËøÆÁ¡£Òª½áÊøÄ£ÄâÔòÔËÐÐ ½áÊø.bat"
Do While 2>1
        wscript.Sleep 60000
        wshShell.SendKeys "{NumLock}"
        wshShell.SendKeys "{NumLock}"
Loop
```



## 结束锁屏脚本

```vbscript
taskkill /f /im wscript.exe
```

s

