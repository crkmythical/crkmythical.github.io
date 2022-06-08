---
title: 在Linux使用apache2搭建webdav服务
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-04-05 10:56:25
top:
tags:
layout:
Referer: https://www.jianshu.com/p/1bac4f50acb9 https://www.gingerdoc.com/tutorials/how-to-configure-webdav-access-with-apache-on-ubuntu-20-04
---
编写进度
- [x] 

## 安装apache2并启用dav模块
```bash
sudo apt install -y apache2 
sudo a2enmod dav dav_fs dav_lock auth_digest
```

## 创建webdav目录及DavLockDB文件
```bash
sudo mkdir /var/webdav
sudo chown www-data:www-data /var/webdav
sudo touch /var/DavLock
sudo chown www-data:www-data /var/DavLock
```

## 创建访问用户
```bash
sudo htpasswd -Bc /var/passwd.dav admin m,./
sudo chmod 640 /var/passwd.dav
sudo chown www-data:www-data /var/passwd.dav

sudo htpasswd -B /var/passwd.dav admin2 m,./     # 新增用户
sudo htpasswd -D /var/passwd.dav admin2
```




## 配置虚拟主机
```bash
cat <<EOF >/etc/apache2/site-available/webdav.conf
<VirtualHost *:80>
    ServerAdmin webmaster@dummy-host.example.com
    ServerName dummy-host.example.com
    ServerAlias www.dummy-host.example.com
    DocumentRoot  /var/webdav

    ErrorLog "/private/var/log/apache2/dummy-host.example.com-error_log"
    CustomLog "/private/var/log/apache2/dummy-host.example.com-access_log" common

	Alias /webdav /var/webdav
	DavLockDB /var/DavLock
    <Directory /var/webdav/>
      Options Indexes MultiViews
      AllowOverride None
      Order allow,deny
      allow from all
    </Directory>

    <Location /webdav>
      Dav On
      AuthType Basic
      AuthName "webDav"
      AuthUserFile /var/webdav/passwd.dav
      Require valid-user
    </Location>

</VirtualHost>
EOF
sudo apachectl configtest  #测试配置文件
```

## 重启服务并测试
```bash
service apache2 restart
cadaver http://127.0.0.1/webdav
```
