---
title: linux上多版本JDK该如何管理
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-03-11 19:07:15
top:
tags:
layout:
---
编写进度
- [x] 
 
### Oracle JDK_8 

[jdk-8u291-linux-x64.tar.gz](https://www.oracle.com/java/technologies/downloads/#java8)

```shell
tar -x -C /opt/jdk -f jdk-8u66-linux-x64.tar.gz   # 解压jdk到/opt/jdk目录
update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_291/bin/java 100   
update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_291/bin/javac 100

update-alternatives --remove java /opt/jdk1.8.0_291/bin/java
update-alternatives --remove javac /opt/jdk1.8.0_291/bin/java
```
