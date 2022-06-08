---
title: 源码监控系统code6搭建
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-01-23 16:29:37
top:
tags:
layout:
---
编写进度
- [x] 


# GitHub代码泄露监控快速实践
## [Code6](https://www.freebuf.com/articles/es/273357.html)docker快速部署
```bash
yum install -y docker git    # 安装docker和git
git clone git clone https://github.com/4x99/code6.git  /opt/code6 ; # 下载code6源码
cd /opt/code6 ; docker build -t code6 .   # 构建docker镜像
docker run --name mysqldb -e MYSQL_ROOT_PASSWORD=code6@2022 -d mysql  # 启动mysql
docker inspect mysqldb | grep IPAddress     # 查看数据库容器的内部IP   
docker exec -it mysqldb  /bin/bash          # 进入mysql容器
    mysql -h 172.17.0.2 -u root -p code6@2022     # 登录mysql数据库
    ALTER USER 'root' IDENTIFIED WITH  mysql_native_password  BY  'code6@2021'; # 修改mysql数据库密码
    create database code6;   # 创建code6数据库
    FLUSH PRIVILEGES;        # 刷新权限
    exit;                    # 退出mysql数据库
docker run -d -p 60080:80 \  # 启动code6-server容器
    -e MYSQL_HOST=172.17.0.2 \  
    -e MYSQL_PORT=3306 \  
    -e MYSQL_DATABASE=code6 \  
    -e MYSQL_USERNAME=root \  
    -e MYSQL_PASSWORD=code6@2022 \ 
    --restart=always   \
    --name code6-server code6
docker exec -it code6-server /bin/bash   # 进入code6-server容器
php artisan code6:user-list              # 查看用户
php artisan code6:user-add  sec@upex.co Sec@upex.co2022  # 添加用户
```
访问 http://<ip>:60080 即可
## 配置使用
### 令牌配置


### 任务配置

