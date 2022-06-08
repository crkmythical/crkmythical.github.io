---
title: linux如何添加自定义服务
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-01-27 18:08:19
top:
tags:
layout:
---
编写进度
- [x] 

# linux如何添加自定义服务-service命令

## 添加配置
```bash
cat <<EOF >/etc/systemd/system/clash.service
[Unit]
Description=Clash service
After=network.target

[Service]
Type=simple
User=skylee
ExecStart=/usr/bin/clash
Restart=on-failure
RestartPreventExitStatus=23

[Install]
WantedBy=multi-user.target
```


## 服务操作
```bash
systemctl list-units --type=service # 查看已识别的服务
systemctl daemon-reload   # 让系统重新读取服务
service <service-name> start/restart/stop
```
## 启/禁用开机自启
```shell
systemctl enable/disable <service-name>
```

