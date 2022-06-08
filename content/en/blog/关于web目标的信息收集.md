---
title: 关于web目标的信息收集
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-26 21:49:32
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [敏感文件](#org075cfdf)
    1.  [源代码  dvcs-ripper](#orgf8dd7ec)
        1.  [.git目录 `.git/config` 存在，漏洞存在](#org0f91b54)
        2.  [.svn目录  `wc.db` 文件存在，漏洞存在](#org90cfaa2)
        3.  [HG文件泄露](#org1207872)
        4.  [.DS<sub>Store文件</sub>  ds<sub>store</sub><sub>exp</sub>](#orgaee4e71)
        5.  [WEB-INF/web.xml文件](#org4a16284)
    2.  [备份文件](#orgc6edac3)
        1.  [网站源码备份](#orgb46d623)
        2.  [gedit备份文件](#org67c723d)
        3.  [vim备份文件](#org400b25e)
        4.  [emacs备份文件](#org05b69f5)
    3.  [常规文件](#org8a78407)
    4.  [fofa信息js脚本采集](#org0f0b5da)
2.  [CMS中间件 wappalyzer](#org2c32552)


<a id="org075cfdf"></a>

# 敏感文件


<a id="orgf8dd7ec"></a>

## 源代码  [dvcs-ripper](https://github.com/kost/dvcs-ripper)


<a id="org0f91b54"></a>

### .git目录 `.git/config` 存在，漏洞存在

-   工具 [scrabble](https://github.com/denny0223/scrabble)

    bash scrabble http://www.example.com/      # make sure target has .git folder
    git reset--hard HEAD^                      # HEAD 代表当前版本   HEAD^代表上一个版本
    git log --stat                             # 查看显示当前的HEAD和它的祖先
    git diff HEAD <commit-id>                  # 比较
    git reflog                                  # 查看所有提交记录，包括分支

-   工具 [lijiejie-Githack](https://github.com/lijiejie/GitHack)    [Githacker](https://github.com/WangYihang/GitHacker)
    
        GitHack.py http://www.openssl.org/.git/  
        pip3 install GitHacker
        githacker --url http://127.0.0.1/.git/ --folder result


<a id="org90cfaa2"></a>

### .svn目录  `wc.db` 文件存在，漏洞存在

    rip-svn.pl -v -u http://www.example.com/.svn/


<a id="org1207872"></a>

### HG文件泄露

hg init的时候会生成.hg

    rip-hg.pl -v -u http://www.example.com/.hg/


<a id="orgaee4e71"></a>

### .DS<sub>Store文件</sub>  [ds<sub>store</sub><sub>exp</sub>](https://github.com/lijiejie/ds_store_exp)

在发布代码时未删除文件夹中隐藏的.DS<sub>store</sub>，被发现后，获取了敏感的文件名等信息。

    python ds_store_exp.py http://www.example.com/.DS_Store


<a id="org4a16284"></a>

### WEB-INF/web.xml文件

-   /WEB-INF/web.xml：Web应用程序配置文件，描述了 servlet 和其他的应用组件配置及命名规则。
-   /WEB-INF/classes/：含了站点所有用的 class 文件，包括 servlet class 和非servlet class，他们不能包含在 .jar文件中
-   /WEB-INF/lib/：存放web应用需要的各种JAR文件，放置仅在这个应用中要求使用的jar文件,如数据库驱动jar文件
-   /WEB-INF/src/：源码目录，按照包名结构放置各个java文件
-   /WEB-INF/database.properties：数据库配置文件

通过找到web.xml文件，推断class文件的路径，最后直接class文件，在通过反编译class文件，得到网站源码。


<a id="orgc6edac3"></a>

## 备份文件


<a id="orgb46d623"></a>

### 网站源码备份

`www.zip/rar/tar.gz` 


<a id="org67c723d"></a>

### gedit备份文件


<a id="org400b25e"></a>

### vim备份文件


<a id="org05b69f5"></a>

### emacs备份文件


<a id="org8a78407"></a>

## 常规文件

    robotx.txt
    readme.md


<a id="org0f0b5da"></a>

## fofa信息js脚本采集

    document.querySelectorAll(".aSpan").forEach((node)=>{console.log(node.innerText);})


<a id="org2c32552"></a>

# CMS中间件 [wappalyzer](https://github.com/AliasIO/wappalyzer)

