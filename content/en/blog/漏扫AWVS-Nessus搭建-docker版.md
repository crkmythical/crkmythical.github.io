---
title: 漏扫AWVS-Nessus搭建-docker版
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-02-09 18:54:33
top:
tags:
layout:
---
编写进度
- [x] 

## AWVS搭建

```shell
docker pull xrsec/awvs:latest
docker run -it -d  --name awvs  -p 3443:3443  --restart=always  xrsec/awvs:latest
```

访问 https://ip:3443

账户密码  ` awvs@awvs.com/Awvs@awvs.com`



## [Nessus](https://github.com/stevemcgrath/docker-nessus_scanner)

```shell
docker pull stevemcgrath/nessus_scanner:latest
docker run -dt \
    -e LINKING_KEY={LINKING_KEY}\
    -e SCANNER_NAME={SCANNER_NAME}\
    --name nessus_scanner\
    stevemcgrath/nessus_scanner:latest
```


## [AWVS&NESSUS](https://mp.weixin.qq.com/s/ecPLUbVGuaHMhPMPE6oWmA)

```shell
docker pull leishianquan/awvs-nessus:v04
docker run -it -d -p 13443:3443 -p 8834:8834 leishianquan/awvs-nessus:v04
echo  `docker ps -a | grep  awvs-nessus` | awk '{print $1}'
docker exec –it 容器id   /bin/bash
/etc/init.d/nessusd start
```

访问 https://ip:8834/

用户密码 `leishi/leishianquan`



https://www.showdoc.com.cn/903737475535911/4808693697997263
