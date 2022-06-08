---
title: xray配置反连平台
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-24 21:47:17
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [xray独立反连平台配置](#org07ca632)
    1.  [服务器端配置](#org19e459d)
    2.  [客户端配置](#org224b658)


<a id="org07ca632"></a>

# xray独立反连平台配置

在 xray 中，反连平台默认不启用，因为这里面有些配置没有办法自动化，必须由人工配置完成才可使用。
需要反连平台才可以检测出来的漏洞包括但不限于：

-   ssrf
-   fastjson
-   s2-052
-   xxe 盲打
-   所有依赖反连平台的 poc


<a id="org19e459d"></a>

## 服务器端配置

在vps上需配置 `config.yaml` 中 **reverse** 部分

    # 服务端
    reverse:
      db_file_path: "reverse.db"
      token: "please_change_me_to_a_new_token"
      http:
        listen_ip: 0.0.0.0
        listen_port: "80"

执行如下命令

    ./xray reverse


<a id="org224b658"></a>

## 客户端配置

在本地客户端的xray需配置 `config.yaml` 如下 **reverse** 部分

    reverse:
      token: "please_change_me_to_a_new_token"
      client:
        remote_server: true
        http_base_url: "http://YOUR_REVERSE_SERVER_IP:80"

执行如下命令

    ./xray_darwin_amd64 webscan --listen 127.0.0.1:7777  --html-output ~/Desktop/xray-report.html

References:
[xray官方文档](https://docs.xray.cool/#/scenario/burp)

