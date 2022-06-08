---
title: 绕过CDN查找IP
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-02-25 19:03:29
top:
tags:
layout:
---
编写进度
- [x] 







# CDN简介

## [什么是CDN](https://help.aliyun.com/document_detail/27101.html)

将web服务器的静态资源(HTML页面、js文件、css文件)及媒体文件(image,video,audio)缓存到CDN节点上，加快网站的响应速度。



假设您的加速域名为`www.a.com`，接入CDN开始加速服务后，当终端用户在北京发起HTTP请求时，处理流程如下图所示。

![CDN原理](https://help-static-aliyun-doc.aliyuncs.com/assets/img/zh-CN/2902102261/p275210.png)

1. 当终端用户向`www.a.com`下的指定资源发起请求时，首先向LDNS（本地DNS）发起域名解析请求。                        

2. LDNS检查缓存中是否有`www.a.com`的IP地址记录。

   如果有，则直接返回给终端用户；

   如果没有，则向授权DNS查询。                        

3. 当授权DNS解析`www.a.com`时，返回域名CNAME `www.a.tbcdn.com`对应IP地址。                        

4. 域名解析请求发送至阿里云DNS调度系统，并为请求分配最佳节点IP地址。

5. LDNS获取DNS返回的解析IP地址。

6. 用户获取解析IP地址。

7. 用户向获取的IP地址发起对该资源的访问请求。                            

   - 如果该IP地址对应的节点已缓存该资源，则会将数据直接返回给用户，例如图中步骤7和8，此时请求结束。
   - 如果该IP地址对应的节点未缓存该资源，则节点向源站发起对该资源的请求。获取资源后结合用户自定义配置的缓存策略，将资源缓存到CDN节点并返回给用户，例如图中的北京节点，此时请求结束。                             

## 常见CDN提供商

- 阿里云CDN
- 腾讯云CDN
- Cloudflare等



# CDN判断与绕过

## 判断网站是否使用CDN

- 通过 `http://ping.chinaz.com/` 判断网站响应IP数量判断使用CDN

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211010232033.png)

- `dig www.example.com A ` 
- `nslookup www.example.com`
- https://www.cdnplanet.com/tools/cdnfinder

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211010235918.png)

## CDN绕过方法

### 1. 子域名绕过

主域名：example.com   IP:    192.168.1.2

源站(www.example.com )IP：192.168.1.2

子域名 sub.example.com   IP可能性：

- 192.168.1.2   同源站
- 192.168.1.3-254  同网段
- 非192.168.1.*的其他IP  （子域名绕过不可用)



### 2. 邮件服务器

CDN加速通过配置DNS的CNAME记录实现

邮件服务器通过DNS的MX记录实现，一般邮件服务器即目标源IP

查看对方邮件原文

### 3. 国外地址请求 https://get-site-ip.com/

鉴于CDN服务费用，一般仅国内加速



### 4. phpinfo页面

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211010233651.png)

### 5. 网络空间引擎搜索特定字符串[md5]



### 6. [DNS历史记录](https://site.ip138.com/)



# 获取CDN后如何绑定指向源站IP

修改本地hosts文件
