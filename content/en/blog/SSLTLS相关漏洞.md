---
title: SSLTLS相关漏洞
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-25 10:41:48
top:
tags:
layout:
---
编写进度
- [x] 



## TLSv1.0检测与修复 ##
TLS 1.0于1999年发行，该版本易受各种攻击（如BEAST和POODLE）已有多年，除此之外，支持较弱加密，对当今网络连接的安全已失去应有的保护效力。
### 检测
  * nmap检测
``` shell
nmap --script ssl-enum-ciphers -p 443 www.target.com
```

  * 在线检测
  [ssl协议与套件检测](https://www.ssleye.com/cipher_suites.html)

### 修复 ###
- Apache

``` shell
sed -i "s/^SSLProtocol.*/SSLProtocol all -SSLv1 -SSLv2 -SSLv3/g"  /etc/httpd/conf.d/ssl.conf
systemctl restart httpd
```

- Nginx

```shell
sed -i "s/^ssl_protocols.*/ssl_protocols TLSv1.2 TLSv1.3;/g" /etc/nginx/nginx.conf
/usr/nginx/sbin/nginx -s reload
```


## SWEET32攻击 CVE-201602183
使用64位块大小如3DES的传统分组密码当以CBC（密码分组链接）模式使用时易遭受碰撞攻击。当使用CBC模式时，简单的生日攻击就能识别出64位分组密码碰撞。当碰撞发生时意味着输入和输出是一样的，这样就可能发动BEAST方式的攻击提取加密数据。


```zsh
./testssl.sh -W https://10.199.27.2

 Testing for SWEET32 (Birthday Attacks on 64-bit Block Ciphers)

 SWEET32 (CVE-2016-2183, CVE-2016-6329)    VULNERABLE,uses 64 bit block ciphers


nmap -sV --script ssl-enum-ciphers -p 443 10.199.27.1
 ```
 
### solutions
参考链接：
[CVE-2016-2183](https://segmentfault.com/a/1190000038486902)
- HTTPD
> /etc/httpd/conf.d/ssl.conf
```zsh
SSLCipherSuite HIGH:!aNULL:!MD5:!3DES;
```
-Nginx
>/usr/local/etc/nginx/nginx.conf
```zsh
ssl_ciphers HIGH:!aNULL:!MD5:!3DES;
```


## Logjam攻击 CVE-2015-4000
Logjam漏洞可帮助攻击者(中间人)将TLS连接降级为512位导出级加密。这有助于攻击者读取和修改通过网络连接传输的任何数据。


```zsh
./testssl -J https://10.199.27.1

Testing for LOGJAM vulnerability

 LOGJAM (CVE-2015-4000), experimental      VULNERABLE (NOT ok): common prime: RFC2409/Oakley Group 2 (1024 bits),
                                           but no DH EXPORT ciphers
```
### Solutions
> /etc/httpd/conf.d/ssl.conf
- HTTPD
```zsh
SSLCipherSuite !EXPORT
```

>/usr/local/etc/nginx/nginx.conf
- Nginx
```zsh
ssl_ciphers '!EXPORT'; 
ssl_ciphers ALL:!aNULL:!EXPORT56:+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP:!RC4:!MD5:!3DES!:SSLv3;  # 或者这个
#Note: - If you already have ssl_ciphers configured, you just need to add !EXPORT in existing line instead of adding new one.
```
