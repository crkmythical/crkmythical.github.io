---
title: 基于docker的网络安全靶场搭建-主机篇
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-03-03 19:06:09
top:
tags:
layout:
---
编写进度
- [x] 

基于docker的网络安全靶场搭建-主机篇

- [操作机-kali搭建](#%E6%93%8D%E4%BD%9C%E6%9C%BA-kali%E6%90%AD%E5%BB%BA)
    - [启动kali容器](#%E5%90%AF%E5%8A%A8kali%E5%AE%B9%E5%99%A8)
    - [安装基础环境包](#%E5%AE%89%E8%A3%85%E5%9F%BA%E7%A1%80%E7%8E%AF%E5%A2%83%E5%8C%85)
    - [生成镜像 kali-2021](#%E7%94%9F%E6%88%90%E9%95%9C%E5%83%8F-kali-2021)
        - [推送镜像到DockerHub](#%E6%8E%A8%E9%80%81%E9%95%9C%E5%83%8F%E5%88%B0dockerhub)
    - [远程桌面环境配置](#%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE)
        - [使用新镜像生成容器并进入容器](#%E4%BD%BF%E7%94%A8%E6%96%B0%E9%95%9C%E5%83%8F%E7%94%9F%E6%88%90%E5%AE%B9%E5%99%A8%E5%B9%B6%E8%BF%9B%E5%85%A5%E5%AE%B9%E5%99%A8)
        - [更换Kali源](#%E6%9B%B4%E6%8D%A2kali%E6%BA%90)
        - [安装桌面环境相关服务](#%E5%AE%89%E8%A3%85%E6%A1%8C%E9%9D%A2%E7%8E%AF%E5%A2%83%E7%9B%B8%E5%85%B3%E6%9C%8D%E5%8A%A1)
        - [配置xrdp设置开机自启动](#%E9%85%8D%E7%BD%AExrdp%E8%AE%BE%E7%BD%AE%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E5%8A%A8)
- [Web靶机制作](#web%E9%9D%B6%E6%9C%BA%E5%88%B6%E4%BD%9C)
    - [启动tomcat-mysql容器](#%E5%90%AF%E5%8A%A8tomcat-mysql%E5%AE%B9%E5%99%A8)

# 操作机-kali搭建

## 启动kali容器

```zsh
docker search kali                    # 搜索kali基础镜像
docker pull kalilinux/kali-rolling    # 拉取基础镜像
docker images                         # 查看镜像
docker run -itd -p 3399:3389 kalilinux/kali-rolling /bin/bash  
                                      # 后台运行容器 宿主3399映射到kali的3389
docker ps -a                          # 查看容器相关信息
docker attach <Container-ID>          # 进入容器
```

## 安装基础环境包

```zsh
apt-get -y update && apt-get -y upgrade && \
   DEBIAN_FRONTEND=noninteractive apt-get install -y \
   kali-tools-top10 \
   pciutils \
   vim \
   iputils-ping \
   bash-completion && \
   apt-get autoremove -y && \
   apt-get clean
```

- `DEBIAN_FRONTEND=noninteractive` 取消交互，直接运行命令，而无需向用户请求输入
- `kali-tools-top10` kai常用工具
- `pciutils` lspci工具
- `vim` 文本编辑器
- `iputils-ping` ping命令
- `bash-completion` 命令自动补全

## 生成镜像 kali-2021

```zsh
docker commit <Container-ID>  <Image-Name>            # 生成镜像
docker commit kali-2021 crkmyth1cal/kali20201:v1      # 可选，生成镜像并发布
```

### 推送镜像到DockerHub

```zsh
docker login                             # 登陆docker
docker push  crkmyth1cal/kali20201:v1    # 推送镜像
docker logout                            # 退出docker
```

## 远程桌面环境配置

### 使用新镜像生成容器并进入容器

`Ctrl+P+Q` 退出容器不终止容器

```zsh
docker run -itd -p 3399:3389 kali-2021                        # 后台运行容器，返回容器ID
docker run -itd -p 3399:3389  -v /tmp/kevin:/data  kali-2021  # 可选： 挂在本地文件
docker exec -it <ID>  /bin/bash                               # 临时开启shell，退出时容器不停止 exec-running container  run-new container
docker start -a <ID>                                          # 进入被Exited的容器
```

### 更换Kali源

```zsh
echo "deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib" | sudo tee /etc/apt/sources.list
apt-get update
```

### 安装桌面环境相关服务

```zsh
apt-get -y install kali-desktop-xfce xorg xrdp
passwd root      # 修改root密码
```

- `kali-desktop-xfce` xfce桌面环境
- `xorg` 是x11的实现，提供X server
- `xrdp` 远程桌面服务

### 配置xrdp设置开机自启动

```zsh
sed -i 's/port=3399/port=3389/g' /etc/xrdp/xrdp.ini     # 修改xrdp服务端口
service xrdp restart                                    # 重启xrdp服务
update-rc.d xrdp enable                                 # xrdp开机自启动
```

# Web靶机制作

## 启动tomcat-mysql容器

```zsh
docker search tomcat-mysql                                     # 搜索tomcat-mysql镜像
docker pull aallam/tomcat-mysql                                # 下载tomcat-msyql镜像
docker run -d --name="tomcat-mysql-run"  \
              -e MYSQL_PASSWORD=root \
                    -p 1306:3306 -p 1080:8080   aallam/tomcat-mysql  # 后台启动， 
docker exec –it ID /bin/bash                                  # 进入tomcat-mysql
```

参考
[基于docker的网络安全靶场搭建-主机篇](https://mp.weixin.qq.com/s/QR9T1TOVIM0sh_TlJhe6Ag)