---
title: Telegram
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-04-09 12:42:22
top:
tags:
layout:
---
编写进度
- [x] 

Telegram（非正式簡稱TG）是跨平台的即時通訊软件

## 如何通过电报机器人给自己发消息(类似微信公众号推送消息通知)

```zsh
curl -X POST "https://api.telegram.org/bot<token>/sendMessage" -d "chat_id=<chat-id>5&text=<message>"
```
所需参数
* *<token>*  机器人token 类似于 1234567890:AAGRfRacMktFHtM1XSpj477m33RSONKlPWo
* *<chat-id>* 会话id 类似于 509654615
* *<message>* 推送内容

* [ ] 创建Telegram机器人 获取Token (任意聊天窗口发送@BotFather，并点击此字符串即可)
  ```zsh
/start
 /newbot 
 <bot_name>         //机器人名字
 <user_name>_bot    //机器人ID  输入完毕后返回此机器人的Token 《1234567890:AAGRfRacMktFHtM1XSpj477m33RSONKlPWo》
  
  Done! Congratulations on your new bot. You will find it at t.me/Bili_auto_checkIn_bot. You can now add a description, about section and profile picture for your bot, see /help for a list of commands. By the way, when you've finished creating your cool bot, ping our Bot Support if you want a better username for it. Just make sure the bot is fully operational before you do this.

	Use this token to access the HTTP API:
	1234567890:AAGRfRacMktFHtM1XSpj477m33RSONKlPWo
	Keep your token secure and store it safely, it can be used by anyone to control your bot.

	For a description of the Bot API, see this page: https://core.telegram.org/bots/api
  ```
  
* [ ] 获取会话id (任意聊天窗口发送@userinfo，并点击此字符串)
  ```zsh
	  @ethan1
  Id: 509654615
	First: Ethan
	Last: Hunter
	Lang: en
  ```
  
* [ ] curl测试推送内容到机器人
```zsh
curl -X POST "https://api.telegram.org/bot173220123420:AAGRfRacMKAtFHtM1XSpj477mMFRSONKlPWo/sendMessage" -d "chat_id=509654615&text=https://askding.github.io"
{
    "ok": true, 
    "result": {
        "message_id": 11, 
        "from": {
            "id": 17020603330, 
            "is_bot": true, 
            "first_name": "Bili", 
            "username": "Bili_checkIn_bot"
        }, 
        "chat": {
            "id": 5096546232, 
            "first_name": "Ethan", 
            "last_name": "Hunter", 
            "username": "et7an", 
            "type": "private"
        }, 
        "date": 1617958429, 
        "text": "https://askding.github.io", 
        "entities": [
            {
                "offset": 0, 
                "length": 25, 
                "type": "url"
            }
        ]
    }
}
```
  

参考
[如何通过电报机器人给自己或群组发消息](https://zhuanlan.zhihu.com/p/146062288)
