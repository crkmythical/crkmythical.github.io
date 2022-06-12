---
title: AD域环境搭建
encrypt: false
enc_pwd: askding
date: 2022-02-05 18:15:20
top:
categories:
tags:
layout:
---
编写进度
- [x] 

# AD小型域环境搭建

参考[快速搭建精简的小型域环境](https://mp.weixin.qq.com/s/lsIyKJXjywh83btdz186Vw)

- WinServer 2016     10.211.55.5  (DC)
- Win7 专业版           10.211.55.6
- Win10 专业版         10.211.55.3

# 在Winserver部署 `Active Directory域服务`

## 更改本地DNS指向本机地址

```powershell
# 设置静态ip
netsh interface ip set addr "本地连接" static 192.168.0.1 255.255.255.0 192.168.0.254 1
netsh interface ip set dns "本地连接" static 202.103.24.68
netsh interface ip add dns "本地连接" 10.211.55.5
# 设置动态ip
netsh interface ip set addr "本地连接" dhcp
netsh interface ip set dns "本地连接" dhcp
```



![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109300955989AD_DNS.png)

## 部署Active Directory域服务

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301046684add_ad.png)

## 配置Active Directory域服务

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301036336ad_conf.png)

⚠️ 指定与控制功能时，不能选择 **只读域控制器(RODC)**



## 添加域用户

- zhangsan    qwe@123
- lisi               qwe@123

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301140291ad_user.png)

验证是否安装成功

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301142454ad_nettime.png)

# 添加域成员机




## 更改DNS指向域控 `10.211.55.5`

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301148913add_dns.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301154047ad_dns.png)



## win7/10 入/出域操作

- powershell(管理员)

  打开powershell

```shell
hostname    # 查看当前主机名
rename-computer -NewName win10    # win10 更改NetBIOS名
add-computer -domain "域名" -cred "域名\授权用户" -passthru  ;restart-computer
remove-computer -credential "域名\授权用户" -passthru -verbose; restart-computer
```



![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301226617ad_join.png)



![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301236181ad_remuser.png)



- `win+R` 运行 `sysdm.cpl`

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301249165ad_join.png)



![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301251093ad_join.png)

出域

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/202109301254268ad_remo.png)

