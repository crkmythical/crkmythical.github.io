---
title: Gopher
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-05-06 11:04:32
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [Gopher](#orgb2c70fa)
    1.  [发送gopher请求](#org7cb8b1b)
    2.  [发送GET请求](#orgd7c10bf)
    3.  [发送POST请求](#org085483d)


<a id="orgb2c70fa"></a>

# Gopher

    gopher://<host>:<port>/<gopher-path>_[TCP/IP数据]  默认端口70

发起post请求，回车换行需要使用\`%0d%0a\`，如果多个参数，参数之间的&也需要进行URL编码

gopher会将后面的数据部分发送给相应的端口，
这些数据可以是字符串，也可以是其他的数据请求包，比如GET，POST请求，redis，mysql未授权访问等
，同时数据部分必须要进行\*\*URL编码\*\*，这样gopher协议才能正确解析。


<a id="org7cb8b1b"></a>

## 发送gopher请求

    curl gopher://www.example.com/path/_[file]
    curl gopher://192.168.43.26:8888/_Hi%0aNewLine%0aThere


<a id="orgd7c10bf"></a>

## 发送GET请求
```
    GET / HTTP/1.1
    Host: baidu.com
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Accept-Encoding: gzip, deflate
    Accept-Language: en-US,en;q=0.9
    Cookie: BAIDUID=AEC95A01B732C2DED98755D470DEE40D:FG=1; BDUSS=1RYVDRKODZ2cWczNEpFTWhlVWJJTmlUeUlGdEZsQnZYS0trUlBxa2FDVEVubzVnRVFBQUFBJCQAAAAAAAAAAAEAAABlgi8TcXE3NDE0NzQ1OTYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMQRZ2DEEWdgS
    Connection: close
```
1.  URL全编码1次HTTP请求包

    `%47%45%54%20%2f%20%48%54%54%50%2f%31%2e%31%0d%0a%48%6f%73%74%3a%20%62%61%69%64%75%2e%63%6f%6d%0d%0a%55%70%67%72%61%64%65%2d%49%6e%73%65%63%75%72%65%2d%52%65%71%75%65%73%74%73%3a%20%31%0d%0a%55%73%65%72%2d%41%67%65%6e%74%3a%20%4d%6f%7a%69%6c%6c%61%2f%35%2e%30%20%28%57%69%6e%64%6f%77%73%20%4e%54%20%31%30%2e%30%3b%20%57%69%6e%36%34%3b%20%78%36%34%29%20%41%70%70%6c%65%57%65%62%4b%69%74%2f%35%33%37%2e%33%36%20%28%4b%48%54%4d%4c%2c%20%6c%69%6b%65%20%47%65%63%6b%6f%29%20%43%68%72%6f%6d%65%2f%38%39%2e%30%2e%34%33%38%39%2e%39%30%20%53%61%66%61%72%69%2f%35%33%37%2e%33%36%0d%0a%41%63%63%65%70%74%3a%20%74%65%78%74%2f%68%74%6d%6c%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%78%68%74%6d%6c%2b%78%6d%6c%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%78%6d%6c%3b%71%3d%30%2e%39%2c%69%6d%61%67%65%2f%61%76%69%66%2c%69%6d%61%67%65%2f%77%65%62%70%2c%69%6d%61%67%65%2f%61%70%6e%67%2c%2a%2f%2a%3b%71%3d%30%2e%38%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%73%69%67%6e%65%64%2d%65%78%63%68%61%6e%67%65%3b%76%3d%62%33%3b%71%3d%30%2e%39%0d%0a%41%63%63%65%70%74%2d%45%6e%63%6f%64%69%6e%67%3a%20%67%7a%69%70%2c%20%64%65%66%6c%61%74%65%0d%0a%41%63%63%65%70%74%2d%4c%61%6e%67%75%61%67%65%3a%20%65%6e%2d%55%53%2c%65%6e%3b%71%3d%30%2e%39%0d%0a%43%6f%6f%6b%69%65%3a%20%42%41%49%44%55%49%44%3d%41%45%43%39%35%41%30%31%42%37%33%32%43%32%44%45%44%39%38%37%35%35%44%34%37%30%44%45%45%34%30%44%3a%46%47%3d%31%3b%20%42%44%55%53%53%3d%31%52%59%56%44%52%4b%4f%44%5a%32%63%57%63%7a%4e%45%70%46%54%57%68%6c%56%57%4a%4a%54%6d%6c%55%65%55%6c%47%64%45%5a%73%51%6e%5a%59%53%30%74%72%55%6c%42%78%61%32%46%44%56%45%56%75%62%7a%56%6e%52%56%46%42%51%55%46%42%4a%43%51%41%41%41%41%41%41%41%41%41%41%41%45%41%41%41%42%6c%67%69%38%54%63%58%45%33%4e%44%45%30%4e%7a%51%31%4f%54%59%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%4d%51%52%5a%32%44%45%45%57%64%67%53%0d%0a%43%6f%6e%6e%65%63%74%69%6f%6e%3a%20%63%6c%6f%73%65%0d%0a%0d%0a`

![img](https://raw.githubusercontent.com/askDing/PicGo/main/images/20210506104027urlencode_all.png)

1.  将URL全编码后的HTTP请求包的字符串附加到\`\_\`后

```zsh
    curl -v gopher://127.0.0.1/_%47%45%54%20%2f%20%48%54%54%50%2f%31%2e%31%0d%0a%48%6f%73%74%3a%20%62%61%69%64%75%2e%63%6f%6d%0d%0a%55%70%67%72%61%64%65%2d%49%6e%73%65%63%75%72%65%2d%52%65%71%75%65%73%74%73%3a%20%31%0d%0a%55%73%65%72%2d%41%67%65%6e%74%3a%20%4d%6f%7a%69%6c%6c%61%2f%35%2e%30%20%28%57%69%6e%64%6f%77%73%20%4e%54%20%31%30%2e%30%3b%20%57%69%6e%36%34%3b%20%78%36%34%29%20%41%70%70%6c%65%57%65%62%4b%69%74%2f%35%33%37%2e%33%36%20%28%4b%48%54%4d%4c%2c%20%6c%69%6b%65%20%47%65%63%6b%6f%29%20%43%68%72%6f%6d%65%2f%38%39%2e%30%2e%34%33%38%39%2e%39%30%20%53%61%66%61%72%69%2f%35%33%37%2e%33%36%0d%0a%41%63%63%65%70%74%3a%20%74%65%78%74%2f%68%74%6d%6c%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%78%68%74%6d%6c%2b%78%6d%6c%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%78%6d%6c%3b%71%3d%30%2e%39%2c%69%6d%61%67%65%2f%61%76%69%66%2c%69%6d%61%67%65%2f%77%65%62%70%2c%69%6d%61%67%65%2f%61%70%6e%67%2c%2a%2f%2a%3b%71%3d%30%2e%38%2c%61%70%70%6c%69%63%61%74%69%6f%6e%2f%73%69%67%6e%65%64%2d%65%78%63%68%61%6e%67%65%3b%76%3d%62%33%3b%71%3d%30%2e%39%0d%0a%41%63%63%65%70%74%2d%45%6e%63%6f%64%69%6e%67%3a%20%67%7a%69%70%2c%20%64%65%66%6c%61%74%65%0d%0a%41%63%63%65%70%74%2d%4c%61%6e%67%75%61%67%65%3a%20%65%6e%2d%55%53%2c%65%6e%3b%71%3d%30%2e%39%0d%0a%43%6f%6f%6b%69%65%3a%20%42%41%49%44%55%49%44%3d%41%45%43%39%35%41%30%31%42%37%33%32%43%32%44%45%44%39%38%37%35%35%44%34%37%30%44%45%45%34%30%44%3a%46%47%3d%31%3b%20%42%44%55%53%53%3d%31%52%59%56%44%52%4b%4f%44%5a%32%63%57%63%7a%4e%45%70%46%54%57%68%6c%56%57%4a%4a%54%6d%6c%55%65%55%6c%47%64%45%5a%73%51%6e%5a%59%53%30%74%72%55%6c%42%78%61%32%46%44%56%45%56%75%62%7a%56%6e%52%56%46%42%51%55%46%42%4a%43%51%41%41%41%41%41%41%41%41%41%41%41%45%41%41%41%42%6c%67%69%38%54%63%58%45%33%4e%44%45%30%4e%7a%51%31%4f%54%59%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%41%4d%51%52%5a%32%44%45%45%57%64%67%53%0d%0a%43%6f%6e%6e%65%63%74%69%6f%6e%3a%20%63%6c%6f%73%65%0d%0a%0d%0a
    
    ❯ nc -lvv 70  
    GET / HTTP/1.1
    Host: baidu.com
    Upgrade-Insecure-Requests: 1
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Accept-Encoding: gzip, deflate
    Accept-Language: en-US,en;q=0.9
    Cookie: BAIDUID=AEC95A01B732C2DED98755D470DEE40D:FG=1; BDUSS=1RYVDRKODZ2cWczNEpFTWhlVWJJTmlUeUlGdEZsQnZYS0trUlBxa2FDVEVubzVnRVFBQUFBJCQAAAAAAAAAAAEAAABlgi8TcXE3NDE0NzQ1OTYAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAMQRZ2DEEWdgS
    Connection: close

```
<a id="org085483d"></a>

## 发送POST请求

必须参数\`POST\`、\`Host\`、\`Content-Type\`、\`Content-Length\`、、

    POST /v1/pages HTTP/2
    Host: content-autofill.googleapis.com
    X-Goog-Encode-Response-If-Executable: base64
    X-Goog-Api-Key: dummytoken
    X-Client-Data: COrfygE=
    Sec-Fetch-Site: none
    Sec-Fetch-Mode: no-cors
    Sec-Fetch-Dest: empty
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
    Accept-Encoding: gzip, deflate
    Accept-Language: en-US,en;q=0.9
    Connection: close
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 9
    
    alt=proto

1.  URL全编码HTTP请求包

2.  将URL全编码后的HTTP请求包的字符串附加到\`\_\`后

```zsh
    curl -v gopher://127.0.0.1/_%50%4f%53%54%20%2f%76%31%2f%70%61%67%65%73%20%48%54%54%50%2f%32%0d%0a%48%6f%73%74%3a%20%63%6f%6e%74%65%6e%74%2d%61%75%74%6f%66%69%6c%6c%2e%67%6f%6f%67%6c%65%61%70%69%73%2e%63%6f%6d%0d%0a%58%2d%47%6f%6f%67%2d%45%6e%63%6f%64%65%2d%52%65%73%70%6f%6e%73%65%2d%49%66%2d%45%78%65%63%75%74%61%62%6c%65%3a%20%62%61%73%65%36%34%0d%0a%58%2d%47%6f%6f%67%2d%41%70%69%2d%4b%65%79%3a%20%64%75%6d%6d%79%74%6f%6b%65%6e%0d%0a%58%2d%43%6c%69%65%6e%74%2d%44%61%74%61%3a%20%43%4f%72%66%79%67%45%3d%0d%0a%53%65%63%2d%46%65%74%63%68%2d%53%69%74%65%3a%20%6e%6f%6e%65%0d%0a%53%65%63%2d%46%65%74%63%68%2d%4d%6f%64%65%3a%20%6e%6f%2d%63%6f%72%73%0d%0a%53%65%63%2d%46%65%74%63%68%2d%44%65%73%74%3a%20%65%6d%70%74%79%0d%0a%55%73%65%72%2d%41%67%65%6e%74%3a%20%4d%6f%7a%69%6c%6c%61%2f%35%2e%30%20%28%57%69%6e%64%6f%77%73%20%4e%54%20%31%30%2e%30%3b%20%57%69%6e%36%34%3b%20%78%36%34%29%20%41%70%70%6c%65%57%65%62%4b%69%74%2f%35%33%37%2e%33%36%20%28%4b%48%54%4d%4c%2c%20%6c%69%6b%65%20%47%65%63%6b%6f%29%20%43%68%72%6f%6d%65%2f%38%39%2e%30%2e%34%33%38%39%2e%39%30%20%53%61%66%61%72%69%2f%35%33%37%2e%33%36%0d%0a%41%63%63%65%70%74%2d%45%6e%63%6f%64%69%6e%67%3a%20%67%7a%69%70%2c%20%64%65%66%6c%61%74%65%0d%0a%41%63%63%65%70%74%2d%4c%61%6e%67%75%61%67%65%3a%20%65%6e%2d%55%53%2c%65%6e%3b%71%3d%30%2e%39%0d%0a%43%6f%6e%6e%65%63%74%69%6f%6e%3a%20%63%6c%6f%73%65%0d%0a%43%6f%6e%74%65%6e%74%2d%54%79%70%65%3a%20%61%70%70%6c%69%63%61%74%69%6f%6e%2f%78%2d%77%77%77%2d%66%6f%72%6d%2d%75%72%6c%65%6e%63%6f%64%65%64%0d%0a%43%6f%6e%74%65%6e%74%2d%4c%65%6e%67%74%68%3a%20%39%0d%0a%0d%0a%61%6c%74%3d%70%72%6f%74%6f
    
    ❯ nc -lvv 70
    POST /v1/pages HTTP/2
    Host: content-autofill.googleapis.com
    X-Goog-Encode-Response-If-Executable: base64
    X-Goog-Api-Key: dummytoken
    X-Client-Data: COrfygE=
    Sec-Fetch-Site: none
    Sec-Fetch-Mode: no-cors
    Sec-Fetch-Dest: empty
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.90 Safari/537.36
    Accept-Encoding: gzip, deflate
    Accept-Language: en-US,en;q=0.9
    Connection: close
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 9
    
    alt=proto

```
