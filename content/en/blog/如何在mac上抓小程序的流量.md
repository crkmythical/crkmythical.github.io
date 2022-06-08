---
title: 如何在mac上抓小程序的流量
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - PostExploit
date: 2021-11-15 20:57:33
top:
tags:
layout:
---
编写进度
- [x] 

## Burpsuite抓取微信小程序流量

通过Proxifier工具代理微信小程序流量到Burp

1. 安装Burpsuite证书到macOS系统中

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291302084burp_cert.png)

2. 安装proxifier并添加Proxifier规则

>Your name or company name:
>macwk.com
>Your registration key:
>2DNRX-V3PNK-TEGYN-DR01D-9UGGT

```shell
brew install --cask proxifier
```

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291312960proxif_reg.png)

添加代理和配置代理规则

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291359940proxi_prox.png)

>微信小程序路径 : /Applications/WeChat.app/Contents/MacOS/Mini Program.app

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291408305proxi_rule.png)

> ⚠️ 先启动Proxifier，再启动Burpsuite，最后打开微信小程序,(最好关掉其他代理)

抓包成功

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291449850prox_burp.png)

存在其他代理(如clashX)时，Proxifier设置方法

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109291505365proxi.png)

