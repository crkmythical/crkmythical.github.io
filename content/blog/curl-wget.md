---
title: curl-wget
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-01-06 18:03:15
top:
tags:
layout:
---
编写进度
- [x] 

## curl
curl是一个精简的命令行浏览器,调试API,类似于postman、Apifox
```zsh
curl  
      [-X GET/POST/HEAD/PUT]     \# 请求方法设置 
      [-G ]
      # 请求头设置
      [-H 'xx: xx']               \# 设置请求头信息
      [-A 'Mozilla/5.0***']       \# 指定User-Agent
      [-b 'foo=bar']              \# 指定cookie
      [-c cookies.txt]            \# 指定cookie
      [-e 'https://xxx']          \# 设定Referer
      [-D respHeader.txt]             \# 将服务器响应头信息保存到respHeader.txt 即cookie
	  [-r 0-100 ]                       \#  设定Range: bytes=start-end 请求资源的部分内容

      # 数据设置
      [-d 'xx']                   \# POST请求数据(自动设置Content-Type: application/x-www-form-urlencoded)
      [-d @local_file]            \# POST请求数据
      [--data-urlencode 'xx']     \# 等同于-d,但会对数据进行url编码
      [-F 'file=@photo.png;filename=me.png;type=application/octet-stream']
      \# 上传二进制文件photo.png(自动设置Content-Type: multipart/form-data, MIME:application/octet-stream 服务器收到的文件名为me.png)

      # 认证信息
      [-u 'bob:12345']          \# 用户密码(自动设置Authorization: Basic Ym9iOjEyMzQ1)

      # 代理设置
      [--no-proxy proxyserver]                    \# 跳过passwd@proxyserver代理
      [-x user:passwd@proxyserver:port]           \# 默认HTTP代理
      [-x socks4://user:passwd@proxyserver:port]  \# socks4代理
      [-x socks5://user:passwd@proxyserver:port]  \# socks5代理

      # 其他设置
      [-s]                  \# slient模式
      [--interface eth1 ]   \# 指定网卡
      [--local-port]        \# 指定源端口
      [-S]                  \# 仅输出错误信息
      [-k]                  \# 跳过SSL检测

      [-v]                  \# 显示请求过程
      [-#]                  \# 显示进度条

      [-I]                  \# 显示请求头 =[-X HEAD]
	  [-i]                  \# 显示响应头
      
      [-C -]                \# 断点续传
      [-L]                  \# 自动重定向

      [-o example.html]     \# 保存html页面
      [-O ]                 \# 保存与服务器url中同名的文件名
```
### 常用格式
```zsh
curl -X <VERB>  '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>'  -d '<BODY>'
```
| 参数 |  描述   |
| --- | --- |  
| `VERB` | 适当的 HTTP *方法* 或 *谓词* : `GET`、 `POST`、 `PUT`、 `HEAD` 或者 `DELETE`。 |
| `PROTOCOL` | `http` 或者 `https`（如果你在 Elasticsearch 前面有一个 `https` 代理） |
| `HOST` | Elasticsearch 集群中任意节点的主机名，或者用 `localhost` 代表本地机器上的节点。 |
| `PORT` | 运行 Elasticsearch HTTP 服务的端口号，默认是 `9200` 。 |
| `PATH` | API 的终端路径（例如 `_count` 将返回集群中文档数量）。Path 可能包含多个组件，例如：`_cluster/stats` 和 `_nodes/stats/jvm` 。 |
| `QUERY_STRING` | 任意可选的查询字符串参数 (例如 `?pretty` 将格式化地输出 JSON 返回值，使其更容易阅读) |
| `BODY` | 一个 JSON 格式的请求体 (如果请求需要的话) |

### curl上传文件
```html
<form action="submit.cgi" method="post" enctype="multipart/form-data">
   Name: <input type="text" name="person"><br>
   File: <input type="file" name="secret"><br>
   <input type="submit" value="Submit">
</form>
```
```zsh
curl -F person=anonymous -F secret=@file.txt http://example.com/submit.cgi
```



### 发送JSON格式的请求
```zsh
curl -XGET 'http://localhost:9200/_count?pretty' -d '
{
    "query": {
        "match_all": {}
    }
}
'

{
    "count" : 0,
    "_shards" : {
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    }
}
```


## wget
wget是一个下载工具
```zsh
wget  [-t 5]  \ #网络不稳定导致的终端，重试下载次数,0表示不断重试
      [-O output_name]   \ # 指定输出文件名
      [--limit-rate 20k]   \# 限速 20k
      [-c]     \ # 断点续传
      [--mirror --convert-links]   \# 复制整个网站
      [-r]      \# 递归下载
      [--user username --password pass]  \# 提供身份验证
      [--user-agent="Mozilla/5.0"] \ #指定user-agent
      [-P /tmp]   \ #指定下载目录
      URL1 URL2 ... URLn
```












