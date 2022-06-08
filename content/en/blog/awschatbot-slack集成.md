---
title: awschatbot_slack集成
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-12-20 12:08:47
top:
tags:
layout:
---
编写进度
- [x] 

# AWS告警通知与Slack集成
> 前提：Slack channel中已添加AWS Chatbot应用

## 配置[AWS SNS](https://ap-northeast-1.console.aws.amazon.com/sns/v3/home?region=ap-northeast-1#/dashboard)服务
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220110106.png)



## 配置[AWS Chatbot](https://us-east-2.console.aws.amazon.com/chatbot/home#/)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220104434.png)

第7步配置好SNS服务时再进行第八步


![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220105454.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220110835.png)

## 配置AWS Cloudwatch通知服务

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220111056.png)


## 测试告警
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211220111807.png)


# FAQ
## 添加了 频道 ID 后，在AWS Chatbot的Channel中显示不出来频道的名字 
需在Slack的频道内添加应用 **AWS Chatbot**

## 通过cloudwatch观察发送记录的日志，报错：
```
Encountered error while sending message to Slack: Slack Web API returned unsuccessful response ( ok: false, error code: channel_not_found, full response body: ChatPostMessageResponse(ok=false, warning=null, error=channel_not_found, needed=null, provided=null, deprecatedArgument=null, responseMetadata=null, channel=null, ts=null, message=null)).
```
需在Slack的频道内添加应用 **AWS Chatbot**
