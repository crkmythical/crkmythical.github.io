---
title: ActiveMQ
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-07 17:26:21
top:
updated: 2020-12-08 18:26:20
tags:
layout:
---
[toc]

ActiveMQ是Apache使用纯Java语言编写的开源消息中间件,

https://juejin.cn/post/6844903920209231886


## 信息收集
```
╭─ethan@ethan.local ~
╰─➤  nmap  10.184.67.63 -sV -Pn -T4  -p 61616
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-04 17:00 CST
Nmap scan report for 10.184.67.63
Host is up (0.047s latency).

PORT      STATE SERVICE  VERSION
61616/tcp open  apachemq ActiveMQ OpenWire transport 5.14.0 (Java 1.8.0_172; arch: amd64)
Service Info: OS: Linux 3.10.0; CPE: cpe:/o:linux:linux_kernel:3.10.0

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.54 seconds

```


## Apache ActiveMQ Information Leak-[CVE-2017-15709]
Apache ActiveMQ默认消息队列61616端口对外，61616端口使用了OpenWire协议，这个端口会暴露服务器相关信息，这些相关信息实际上是debug信息,会返回应用名称，JVM，操作系统以及内核版本等信息。

### `telnet`测试
```
telnet 10.184.67.53 61616
```

### MSF相关漏洞模块