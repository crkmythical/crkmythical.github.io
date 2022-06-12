---
title: Fail2ban
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-05-24 13:16:52
top:
tags:
layout:
---
编写进度
- [x] 

# Fail2ban
Fail2Ban 是入侵检测软件框架，保护计算机免受暴力破解（brute-force attack）。以 Python 语言编写，能运行于具有包（packet）控制或防火墙的 POSIX 系统，如 iptables 或 TCP Wrapper.

Fail2Ban可以通过描日志文件并用正则匹配分析，然后通过更新防火墙规则来禁止某些有恶意迹象的 IP（密码失败过多，寻求漏洞利用等）来提高服务器安全性。
## fail2ban安装/开机自启
```shell
apt install fail2ban
yum install fail2ban
systemctl restart fail2ban
systemctl enable fail2ban.service # 开机自启
```

## 配置文件 `/etc/fail2ban/`
-   `/etc/fail2ban/filter.d/`：条件文件夹, 内含默认文件，用于定义日志文件内容的过滤规则,其中预设于了 SSH、Nginx、Apache 的监控规则
-   `/etc/fail2ban/action.d/`：动作文件夹, 发现恶意 IP 后采取的操作，其中预设了许多常用操作，其中预设了 iptables、firewalld、sendmail 等操作
-   `/etc/fail2ban/jail.d`：配置文件夹, Jail 由 Filter 和 Action 组成，用于定义错误次数、封禁时长、封禁动作等
-   `/etc/fail2ban/jail.conf` ：定义 fai2ban 自身的日志级别、日志位置等

当 filter 文件监视到的错误记录条数在 jail 中定义的时间内达到 jail 中定义的次数后，告知系统 iptables 执行封禁动作及封禁时长。在封禁时长到期时，告知 iptables 解除封禁。


### 配置过滤规则 
- `/etc/fail2ban/filter.d/<fiter-name>.local`

首先设置过滤规则，在 `/etc/fail2ban/filter.d/` 目前下新建一个 `.conf` 文件，名字自取，比如我新建的是 `nginx-zatp-com.conf`，然后进行设置：

```conf
[Definition]
failregex = 
ignoreregex =
```

其中：

**failregex**：表示过滤规则的正则表达式；  
**ignoreregex**：表示忽略规则的正则表达式，可以设置为 `.*(webp|svg|jpg|png)` 忽略对图片文件的请求，防止图片文件过多误伤；

而这里要实现我们想要的效果，也有两个选择，配合开篇提到的 Nginx 流控产生的日志文件（error.log）进行匹配过滤或者直接对 Nginx 的访问日志文件（access.log）进行匹配过滤。原理都一样，fail2ban 预置了很多常见服务的日志文件匹配模板，在 `/etc/fail2ban/filter.d/` 目录下可以找到。如果你修改了日志格式，那么需要**根据你的日志文件格式改写相应的表达式**。

这里我用 Nginx 的 `limit_req_zone` 流控模块做一个示例，下面是一条超过限制产生的错误信息：

```
2021/10/13 01:02:39 [error] 14174#0: *41792 limiting requests, excess: 5.920 by zone "request", client: x.x.x.x, server: www.zatp.com, request: "HEAD /?feed=rss HTTP/2.0", host: "www.zatp.com"
```

参考自带的 `/etc/fail2ban/filter.d/nginx-limit-req.conf` 模板，可以写成下面的表达式：

```
[Definition]
failregex = ^\s*\[[a-z]+\] \d+#\d+: \*\d+ limiting requests, excess: [\d\.]+ by zone ".*", client: <HOST>,
ignoreregex = 
```

`failregex` 也可以直接简写为：

```
failregex = limiting requests, excess:.* by zone.*client: <HOST>
```

其中 `<HOST>` 是必须包含的，fail2ban 通过这个来获取 IP 地址，测试自定义的规则是否生效：

```
fail2ban-regex /var/log/nginx/access.log /etc/fail2ban/filter.d/nginx-zatp-com.conf
```

如果成功匹配会返回匹配到的信息：

```
Results
=======

Failregex: 4 total
|-  #) [# of hits] regular expression
|   1) [4] ^\s*\[[a-z]+\] \d+#\d+: \*\d+ limiting requests, excess: [\d\.]+ by zone ".*", client: <HOST>,
`-

Ignoreregex: 0 total
```

### 配置Jail规则 
Fail2ban在安装时会创建两个默认的配置文件:
- `/etc/fail2ban/jail.d/defaults-debian.conf`
- `/etc/fail2ban/jail.conf`
我们不建议直接修改这些文件，因为更新Fail2ban时它们可能会被覆盖。

Fail2ban将按以下顺序读取配置文件。每个`.local`文件都会覆盖`.conf`文件中的设置：
-   `/etc/fail2ban/jail.conf`
-   `/etc/fail2ban/jail.d/*.conf`
-   `/etc/fail2ban/jail.local`
-   `/etc/fail2ban/jail.d/*.local`

#### 创建自定义配置文件
```shell
cp /etc/fail2ban/jail.{conf,local}

ignoreip = 127.0.0.1/8 ::1 123.123.123.123 192.168.1.0/24
```
- `[jail_name]` : jail的名称
- `enabled` : 可以设置为**true**或**false**以启用/禁用过滤器
- `port` : 运行服务的端口，如果使用的端口是默认端口，则可以使用服务名称，否则需要明确指定端口号
- `filter` : 位于`/etc/fail2ban/filter.d/`目录中的过滤器文件的名称(需要与自定义规则里的名称一致，不需要.conf后缀)，其中包含用于解析日志的fileregex信息
- `logpath`: fail2ban检测的日志文件路径
- `backend` : 用于获取日志文件修改的后端 `auto`
- `ignoreip` : 设置白名单ip地址,以空格分隔多个ip/ip段
- `banaction` : 实时锁定行动的类型
- `action` : `iptables-allports` 如果被触发，执行怎样的脚本？
- `banaction_allports` : Fail2ban阻止每个端口上的远程IP地址
- `bantime` : 锁定时长,默认单位为秒, 默认10m ，值-1将永久禁止IP地址
- `maxretry` : 允许IP失败尝试次数。 默认值设置为5，
- `findtime` : 重试次数之前的持续时间


```shell
findtime=10m
maxretry=3
bantime=1d
```
表示 `findtime(10m)`中内失败次数达到`maxretry(3)`次则锁定`bantime(1d)`

### 邮件通知
```bash
apt install -y  bsd-mailx sendmail
systemctl restart sendmail.service
cat <<EOF >> /etc/mail.rc
set bsdcompat
set from= 741474596@qq.com
set smtp=smtp.qq.com:465      
set smtp-auth-user=741474596@qq.com
set smtp-auth-password=gkotyjhergynbdjh     
set smtp-auth=login 
#启用ssl加密
set smtp-user-starttls                                          
set ssl-verify=ignore
#ssl的加密证书
set nss-config-dir=/etc/ssl/certs 
EOF
 
# 测试发送邮件
echo "邮件内容" |mail -v -s "邮件标题" xxx@xxx.com
mail -s "theme" xx@xxx.com < message.txt


```



Fail2ban可以在IP被禁止时发送电子邮件警报。 要接收电子邮件，您需要在服务器上安装SMTP并更改默认的action，该action操作仅将IP禁止为`%(action_mw)s`，如下所示：



```
# /etc/fail2ban/jail.local

action = %(action_mw)s
```

`%(action_mw)s`将禁止有问题的IP，并发送包含Whois报告的电子邮件。 如果要在电子邮件中包含相关日志，请将操作设置为`%(action_mwl)s`。您还可以调整发送和接收电子邮件地址：

```
# /etc/fail2ban/jail.local

destemail = admin@myfreax.com
sender = root@myfreax.com
```

### 配置Fail2ban telegram 机器人通知
```bash
#创建Telegram 机器人为关注`Botfather`，按提示操作即可，
#可查找机器人Token，添加`userinfobot`查找自己聊天ID
#在/etc/fail2ban/action.d/中新建telegram.conf文件并写入
[Definition]
actionstart = /etc/fail2ban/scripts/send_telegram_notif.sh -a start
actionstop = /etc/fail2ban/scripts/send_telegram_notif.sh -a stop
actioncheck =
actionban = /etc/fail2ban/scripts/send_telegram_notif.sh -n <name> -b <ip>
actionunban = /etc/fail2ban/scripts/send_telegram_notif.sh -n <name> -u <ip>
[Init]
init = 123```

```bash
#在/etc/fail2ban/中新建scripts目录，并新增send_telegram_notif.sh脚本文件写入
#!/bin/bash
# Version 1.0
# Send Fail2ban notifications using a Telegram Bot
# Add to the /etc/fail2ban/jail.conf:
# [sshd]
# ***
# action  = iptables[name=SSH, port=22, protocol=tcp]
#                       telegram
# Create a new file in /etc/fail2ban/action.d with the following information:
# [Definition]
# actionstart = /etc/fail2ban/scripts/send_telegram_notif.sh -a start
# actionstop = /etc/fail2ban/scripts/send_telegram_notif.sh -a stop
# actioncheck =
# actionban = /etc/fail2ban/scripts/send_telegram_notif.sh -n <name> -b <ip>
# actionunban = /etc/fail2ban/scripts/send_telegram_notif.sh -n <name> -u <ip>
#
# [Init]
# init = 123
# Telegram BOT Token
telegramBotToken='xxxxx' 
#此处替换为自己Telegram 机器人Token
# Telegram Chat ID
telegramChatID='xxxxx'#此处替换为自己的Chat ID
function talkToBot() {
        message=$1
        curl -s -X POST https://api.telegram.org/bot${telegramBotToken}/
sendMessage -d text="${message}"-d chat_id=${telegramChatID} > /dev/null 2>&1
}
if[ $# -eq 0 ]; then
        echo "Usage $0 -a ( start || stop ) || -b $IP || -u $IP"
        exit 1;
fi
while getopts "a:n:b:u:" opt; do
case"$opt"in
                a)
                        action=$OPTARG
;;
                n)
                        jail_name=$OPTARG
;;
                b)
                        ban=y
                        ip_add_ban=$OPTARG
;;
                u)
                        unban=y
                        ip_add_unban=$OPTARG
;;
                ?)
                        echo "Invalid option. -$OPTARG"
                        exit 1
;;
esac
done
if[[ ! -z ${action} ]]; then
case"${action}"in
                start)
                        talkToBot "Fail2ban has been started on `hostname`."
;;
                stop)
                        talkToBot "Fail2ban has been stopped on `hostname`."
;;
*)
                        echo "Incorrect option"
                        exit 1;
;;
esac
elif[[ ${ban} == "y"]]; then
        talkToBot "[${jail_name}] The IP: ${ip_add_ban} has been banned on `hostname`."
        exit 0;
elif[[ ${unban} == "y"]]; then
        talkToBot "[${jail_name}] The IP: ${ip_add_unban} has been unbanned on `hostname`."
        exit 0;
else
        info
fi
```

```bash
#给send_telegram_notif.sh脚本添加可执行权限
chmod +x send_telegram_notif.sh
#修改jail.local配置文件，将启用的jail的action下添加一个telegram，如下
action  = iptables[name=SSH,port=2202,protocol=tcp]
            telegram
#重启fail2ban验证
systemctl restart fail2ban
```

Telegram 机器人告警通知效果如图
![](https://devops.chat/wp-content/uploads/2021/09/88d658795fad1c4_1_post.png)



## 激活fail2ban
```shell
systemctl restart fail2ban
service fail2ban restart

```

## 检测生效脚本
```shell
#!/bin/bash
for ((i=1;i<=50;i++)); do
curl -H "Fail2ban test" https://your-domian/test > /dev/null 2>&1
done
echo "done"```

# 使用Fail2ban客户端
`fail2ban-client <COMMAND>`
以下是 Fail2ban-client 命令列表：
-   `start`: 用于启动fail2ban服务器和jails
-   `reload`: 用于重新加载 Fail2ban 配置
-   `stop`: 停止服务器
-   `status`: 用于检查服务器状态并启用 jails
-   `status JAIL` : 显示监狱的状态和当前被禁止的 IP

## 查看所有命令
```shell
fail2ban-client -help
```


## 检查启动状态
```shell
fail2ban-client ping   # 正确启动的话fail2ban会以pong作为回应(Server replied: pong)
sudo fail2ban-client version #查看 Fai2ban 的版本
sudo fail2ban-client ping #检查 Fail2ban 是否正常运行（正常将显示 pong）
sudo systemctl start fail2ban #启动 Fail2ban
sudo systemctl stop fail2ban #停止 Fail2ban
sudo systemctl restart fail2ban #重启 Fail2ban
sudo tail -f /var/log/fail2ban.log #打开 Fail2ban 的日志监控

sudo iptables --list -n #显示系统当前 iptables
sudo iptables -D INPUT -s xxx.xxx.xxx.xxx -j DROP #解除封禁的 IP
```

## 查看指定 Jail 规则下被封禁的IP情况
```shell
fail2ban-client status [jailname]
```


## 封禁/解封限制IP
`sudo fail2ban-client set <jailname> banip/unbanip <IP>`

```shell
sudo fail2ban-client set sshd banip 23.34.45.56
sudo fail2ban-client set sshd unbanip 23.34.45.56
```

## 添加/解除指定IP的忽略
```shell
fail2ban-client set <JAIL> addignoreip/delignoreip <IP>
```

## 验证自定义规则
```bash
fail2ban-regex [OPTIONS] <LOG> <REGEX> [IGNOREREGEX]
```
- `LOG`为需要匹配的日志文件路径，
- `REGEX`为正则表达式所在的文件路径(通常位于`filter.d`文件夹内)

以下为常用的`OPTIONS`
```bash
// Do not print any missed lines
--print-no-missed

// Do not print any ignored lines
--print-no-ignored

// Print all matched lines
--print-all-matched

// Print all missed lines, no matter how many
--print-all-missed

// Print all ignored lines, no matter how many
--print-all-ignored
```

## 查看fail2ban的日志
```shell
tail -f /var/log/fail2ban.log
```