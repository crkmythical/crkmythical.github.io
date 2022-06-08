---
title: Mac下搭建Nginx+php开发平台
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-07 21:01:16
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [软件安装](#org5a512c0)
    1.  [配置路径](#orge939893)



<a id="org5a512c0"></a>

# 软件安装

-   php-fpm : FastCGI进城管理器(mac自带)
-   nginx : 高性能的HTTP和反向代理web服务器

      brew install nginx
      Updating Homebrew...
      ==> Downloading from https://pkg-containers.githubusercontent.com/ghcr1/blobs/sha256:8ba34676e573272aa1f73d4dcf6bfddbaa69746a92bf812f6760b######################################################################## 100.0%
      ==> Pouring nginx--1.21.1.big_sur.bottle.tar.gz
      ==> Caveats
      Docroot is: /usr/local/var/www
    
      The default port has been set in /usr/local/etc/nginx/nginx.conf to 8080 so that
      nginx can run without sudo.
    
      nginx will load all files in /usr/local/etc/nginx/servers/.
    
      To have launchd start nginx now and restart at login:
        brew services start nginx
      Or, if you don't want/need a background service you can just run:
        nginx
      ==> Summary
      🍺  /usr/local/Cellar/nginx/1.21.1: 25 files, 2.2MB
    
    brew services start nginx  # 开机自启nginx


<a id="orge939893"></a>

## 配置路径

-   nginx安装路径 `/usr/local/Cellar/nginx/1.21.1`
-   nginx配置文件 `/usr/local/etc/nginx/nginx.conf`
-   Docroot `/usr/local/var/www`
-   php-fpm配置文件 `/etc/php-fpm.conf.default`

    cp /private/etc/php-fpm.conf.default /private/etc/php-fpm.conf
    cp /private/etc/php-fpm.d/www.conf.default /private/etc/php-fpm.d/www.conf

-   创建error<sub>log文件</sub>
    
        mkdir /usr/local/var/log/php-fpm
        touch /usr/local/var/log/php-fpm/php-fpm.log
        
        //修改php-fpm.conf
        error_log = /usr/local/var/log/php-fpm/php-fpm.log

参考：
[基于Mac自带nginx,php配置php运行环境](https://segmentfault.com/a/1190000025138860)

