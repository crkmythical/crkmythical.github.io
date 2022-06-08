---
title: kali inux系统优化
category:
- Kali
- 
tags:
  - system
date: 2020-12-03 20:44:23
updated:
---

## 系统配置

### 设置root密码
```zsh 
sudo passwd root
echo "$USER ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/dont-prompt-$USER-for-sudo-password  # 执行root权限的命令
```

### 更改区域设置
```zsh
sudo dpkg-reconfigure locales

> - en_US.UTF-8
> - zh_CN.UTF-8
```

### 输入法&字体配置
```zsh
wget https://ime.sogoucdn.com/dl/index/1605612770/sogoupinyin_2.4.0.2732_amd64.deb?st=dhRiak9ucl6k3GZibQ0Tfg&e=1605918428&fn=sogoupinyin_2.4.0.2732_amd64.deb   # 下载搜狗输入法软件包

dpkg -i sougoupinyin.deb  #安装输入法
dpkg --configure -a  #软件包更新中断时的修复命令

dpkg -r sogoupinyin   # 删除软件
dpkg -P sougoupinyin  # 删除软件及配置文件

apt install zsh-theme-powerlevel9k # 安装zsh-theme-powerlevel9k字体
```

### 升级系统
```zsh
echo "#deb http://http.kali.org/kali kali-rolling main non-free contrib
deb http://mirrors.aliyum.com/kali kali-rolling main non-free contrib
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib" | tee /etc/apt/sources.list   #配置阿里和中科大源

apt update && apt upgrade -y  # 升级软件列表并更新软件 \&&
apt dist-upgrade -y  # 升级系统 \&&
apt clean && apt autoclean && apt autoremove -y # 清除 已下载的软件包 和 旧软件包

dpkg -l |grep ^rc|awk '{print $2}' |sudo xargs dpkg -P   # 清理系统残存配置
```
常见清理命令理解
* apt remove/autoremove 卸载软件
* apt clean/autoclean   删除软件包
|命令|作用|
|:---|:---:|
|`apt purge `| 删除软件,删除配置信息|
|`apt remove `|删除软件(保留配置信息)|
|`apt autoremove`|删除为了满足其他软件包的依赖而安装，但现在不再需要的软件包|
|`apt autoclean` |删除未安装在系统的软件包|
|`apt clean` | 删除已安装的软件包(/var/cache/apt/archives内的软件包 |
|apt install --fix-broken /-f |dpkg安装失败时修复依赖关系|


### 安装/清理内核头文件
```
apt install linux-headers-$(uname -r)   
dpkg --get-selections | grep linux   
apt purge <kernel-name>  <haders-name>
```

### UI优化
- 主题文件：/usr/share/themes/
- 图标文件：/usr/share/icons/
- 背景壁纸：/usr/share/background/ 
- grub启动图片： /usr/share/images/desktop-bas
	- login-background.png #进如系统界面的背景图  
	- kali-grub.png　　　　　#grub的背景图片  
 	- kali-wallpaper_1024×786 #类似的都是桌面背景图

- 、/usr/share/wallpapers/
- conky: /etc/conky/


### 终端优化
1. zsh优化
```zsh
#oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"  # 安装oh-my-zsh
sed -i 's/robbyrussell/gnzh/g'  .zshrc  # 替换主题
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions  # 下载zsh-autosuggestions插件
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  # 下载syntax-highling插件
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions   # 下载zsh-completions
sed -i "s/plugins.*/plugins=(git web-search autojump zsh-autosuggestions zsh-syntax-highlighting zsh-completions )/g" .zshrc  # 配置插件
apt install autojump  # 安装autojump
curl -fsSL https://starship.rs/install.sh | bash #下载prompt-starship
echo "ZXZhbCAiJChzdGFyc2hpcCBpbml0IGJhc2gpIgo=" | base64 -d >>~/.zshrc    # 添加eval "$(starship init bash)"  到.zshrc
rm "$(which starship)"  # Locate and delete the starship binary
#opt: zsh欢迎语
echo "CiMgenNoIHdlbGNvbWUgYmFubmVyIQpmaWdsZXQgImFza0RpbmciCmVjaG8gLW5lICJUb2RheSBpczpcdFx0JHtyZWR9IiBgZGF0ZWAgO2VjaG8gIiIgI2Rpc3BsYXkgY3VycmVudCB0aW1lCmVjaG8gLW5lICIke2xpZ2h0Z3JlZW59S2VybmVsIEluZm9ybWF0aW9uOiBcdCR7cmVkfSIgYHVuYW1lIC1zbXJgICNkaXNwbGF5IHN5c3RlbSBpbmZvcm1hdGlvbgoK"| base64 -d  >> .zshrc && source .zshrc  # 设置zsh欢迎语并立即生效
```

2. vim/neovim优化
```zsh
curl -sLf https://spacevim.org/install.sh | bash  
```

3. tmux优化
```zsh
git clone https://github.com/gpakosz/.tmux.git ~/.tmux &&\
ln -s -f .tmux/.tmux.conf  &&\
cp .tmux/.tmux.conf.local .   # tmux
```

4. eDux-ui终端
> https://github.com/GitSquared/edex-ui

### 其他优化

#### 永久开启IP转发功能
```zsh
echo "net.ipv4.ip_forward=1"  >> /etc/sysctl.conf

echo 1 > /proc/sys/net/ipv6/conf/all/forwarding
```




#### 右键文件编码转换




## 系统服务配置


### PostgreSQL服务
```zsh
systemctl start postgresql.service
systemctl enable postgresql
```

### 设置 SSH 通过密钥登录
分为服务器`sshd文件配置`和`本地客户端配置`

#### <1>VPS上sshd配置
1. 在vps上生成rsa密钥,并为私钥添加passphrase,
			
```zsh
[root@～]# ssh-keygen -t rsa        #生成rsa密钥
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):      #设置密钥短语
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:9k3RJ8EpR63FIK5PSkCdujrIoP1gmkokxcDuCfV+eoI root@kali
The key's randomart image is:
+---[RSA 3072]----+
|o       .. ..o+= |
| +.    .  o..o+.+|
|..o.    ..  oooo.|
|.o  .   .. . ..o |
|+...    S.o o    |
|oo. . ...o *     |
| +o+ + .  o o    |
|o+E.= +          |
|=  ..o .         |
+----[SHA256]-----+
```

2. 把公钥文件写入到` ~/.ssh/authorized_keys`
```zsh
[root@～]# cat .ssh/id_rsa.pub >> .ssh/authorized_keys     #添加公钥到授权文件
```

3. 更改.ssh目录及文件权限
```zsh
[root@local]# chmod 700 .ssh  &&\
			  chmod 600 .ssh/id_rsa.pub  .ssh/authorized_keys &&\
			  chmod 400 .ssh/id_rsa
    #设置.ssh目录权限
```

4. 修改`/etc/ssh/sshd_config`文件
```zsh
sed -i "s/#Port.*/Port 2020/g" /etc/ssh/sshd_config  #修改端口为2020

sed -i "s/#PermitRootLogin.*/PermitRootLogin yes/g" /etc/ssh/sshd_config   #允许root登陆

sed -i "38a\RSAAuthentication yes" /etc/ssh/sshd_config   #使用RSA认证

sed -i "s/^#AuthorizedKeysFile.*/AuthorizedKeysFile .ssh\/authorized_keys/g" /etc/ssh/sshd_config  #启用authorized_keys文件

sed -i "s/^PasswordAuthentication.*/PasswordAuthentication no/g" /etc/ssh/sshd_config    #关闭密码认证

可选:建立隧道
sed -i "s/#AllowTcpForwarding yes/AllowTcpForwarding yes/g" /etc/ssh/sshd_config  #启用tcp转发

sed -i "s/#TCPKeepAlive yes/TCPKeepAlive yes/g"  /etc/ssh/sshd_config   #防止死连接
```

5. 设置开机自启动服务
```zsh
[root@～]# systemctl restart ssh && systemctl enable ssh
```

#### <2>本地配置
拷贝私钥到本地电脑为id_rsa.vps
```zsh
[root@～]# ssh -i .ssh/id_rsa.vps root@172.16.41.4 -p 2020
```

## 软件安装


### 渗透测试软件

#### [BurpSuite Professional]
> https://pan.baidu.com/s/1klNoVJdPxVdanAbiJBT4kg  密码: mpnk

> Project options > Misc > Embedded Browser

```zsh
apt purge burpsuite  # 清除系统自带社区版
apt-mark hold burpsuite  # 禁止burpsuite自动安装/升级/卸载

echo "W0Rlc2t0b3AgRW50cnldCk5hbWU9QnVycFN1aXRlIFByb2Zlc3Npb25hbApFbmNvZGluZz1VVEYtOApFeGVjPSBzaCAtYyAiY2QgL29wdC9idXJwc3VpdGVfcHJvOyBqYXZhIC1qYXIgL29wdC9idXJwc3VpdGVfcHJvL0J1cnBTdWl0ZUxvYWRlci5qYXIiCkljb249a2FsaS1idXJwc3VpdGUKU3RhcnR1cE5vdGlmeT1mYWxzZQpUZXJtaW5hbD1mYWxzZQpUeXBlPUFwcGxpY2F0aW9uCkNhdGVnb3JpZXM9MDMtd2ViYXBwLWFuYWx5c2lzOzAzLTA2LXdlYi1hcHBsaWNhdGlvbi1wcm94aWVzOwpYLUthbGktUGFja2FnZT1idXJwc3VpdGU=" | base64 -d | tee /usr/share/applications/kali-burpsuite.desktop  # 配置BurpSuite启动器链接，启动路径为Exec= sh -c "cd /opt/burpsuite_pro; java -jar /opt/burpsuite_pro/BurpSuiteLoader.jari"

chattr +i /usr/share/applications/kali-burpsuite.desktop  # 防止升级时被删除或修改
```

#### [Nessus Professional]
> 链接: https://pan.baidu.com/s/1RkXQE5XkeBGHlgWK42pArA  密码: c07r

* 安装
```zsh
dpkg -i Nessus-8.12.1-debian6_amd64.deb   # 安装Nessus软件
systemctl start nessusd.service  # 启动nessud服务
```
* 访问https://localhost:8834/  
* 选择 `Managed Scanner` 然后 `tenable.SC` 最后创建 `user with password` 并登陆
* 升级插件库并替换`plugin_feed_info.inc`文件
```zsh
systemctl stop nessusd.service  # 关闭nessud服务
/opt/nessus/sbin/nessuscli update /root/Desktop/all-2.0.tar.gz  # 升级插件库

cp /root/Desktop/plugin_feed_info.inc  /opt/nessus/var/nessus/plugin_feed_info.inc  # 替换plugin_feed_info.inc
cp /root/Desktop/plugin_feed_info.inc  /opt/nessus/lib/nessus/plugins/plugin_feed_info.inc   # 替换plugin_feed_info.inc

systemctl start nessusd.service  # 启动nessud服务
```




### 常用软件

#### 码字神器Typora
```zsh
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE

wget -qO - https://typora.io/linux/public-key.asc | sudo apt-key add -

# add Typora's repository

sudo add-apt-repository 'deb https://typora.io/linux ./'

sudo apt-get update

# install typora

sudo apt-get install typora
```

#### 码字神器Joplin
```zsh
apt install joplin /joplin-cli
```
或者
```zsh
apt install nodejs npm
npm config set registry https://registry.npm.taobao.org
echo  -e "\n#Jopling configure\nNPM_CONFIG_PREFIX=~/.joplin-bin\n" >> ~/.zshrc
npm install -g joplin
ln -s ~/.joplin-bin/bin/joplin /usr/bin/joplin
```

#### 办公神器WPS
```zsh
wget https://wdl1.cache.wps.cn/wps/download/ep/Linux2019/9719/wps-office_11.1.0.9719_amd64.deb  # 下载wps-office软件

dpkg -i wps-office_11.1.0.9719_amd64.deb 
```

#### 代码差异分析神器diff-so-fancy
```zsh
git clone https://github.com/so-fancy/diff-so-fancy.git
```

#### 远程连接神器Remmina
```zsh
apt  install remmina
```

#### 代理Proxy软件v2rayL
* v2aryL
```zsh
bash <(curl -s -L http://dl.thinker.ink/install.sh)
```
* clash
> https://github.com/Dreamacro/clash/releases
```zsh
tar -zxvf clash-linux-amd64-v1.3.0.gz
mv clash-linux-amd64-v1.3.0 /usr/local/bin/clash
chmod +x /usr/local/bin/clash
```

#### 文件互传神器[croc](https://github.com/mingrammer/diagrams)
```zsh
curl https://getcroc.schollz.com | bash
```

#### 图形化hex编辑器
```zsh
apt install bless
```

#### 系统优化&监视器
* stacer
```zsh
apt install stacer
```
* bashtop
```zsh
git clone https://github.com/aristocratos/bashtop.git &&\
cd bashtop  && \
sudo make install
```

#### Gif录屏工具peek
```zsh
apt install peek
```


## 自动化配置脚本-后续整理
```zsh
echo ""
echo "=========================================================================="
echo "=               Kali Auto Init Tool                                      ="
echo "=                      Powered by Mr.Framework                               ="
echo "=                      https://askding.github.io                        ="
echo "=========================================================================="

echo ""
echo "[*] 即将自动对kali进行基本配置，建议你根据需要修改脚本。安装配置过程可能需要一会儿，并且由你的网速决定...."
read -p "[*] 请按任意键继续...."


echo "[+] 添加kali源"
apt-key adv --recv ED444FF07D8D0BF6
echo "deb http://http.kali.org/kali kali-rolling main non-free contrib" >> /etc/apt/sources.list
echo "deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib" >> /etc/apt/sources.list
echo "deb http://mirrors.aliyun.com/debian stable main contrib non-free"  >> /etc/apt/sources.list
echo "[ok] 添加kali源成功！"
echo ""

echo "[+] 添加一个普通用户"
read -p "请输入用户名: " username
useradd -m -G sudo,video,audio,cdrom -s /bin/bash $username
echo "请设置用户密码："
passwd $username
echo "[ok] 添加普通用户成功！"
echo ""

# 安装内核头文件
echo "[+] 安装内核头文件... "
apt-get -y install linux-headers-$(uname -r)
echo ""
echo "[ok] 内核头文件安装成功！"
echo ""

# 解决kali启动时静音问题
echo "[+] 安装 alsa-utils 解决kali启动时静音问题"
apt-get -y install alsa-utils
echo "[ok] 安装 alsa-utils 成功！"
echo ""

echo "[+] 添加PPPoE拨号连接功能"
apt-get install pppoe pppoeconf
echo "[ok] 安装PPPoE成功!"
echo "      >> 你可以使用 nm-connection-editor 命令管理pppoe连接"
echo ""

echo "[+] 添加VPN支持: PPTP IPsec/IKEv2 vpnc network-manager-ssh"
apt-get -y install network-manager-pptp network-manager-pptp-gnome network-manager-strongswan network-manager-vpnc network-manager-vpnc-gnome network-manager-ssh
echo "[ok] 成功添加vpn支持!"
echo ""


# Base Tool
echo "[+] 安装一些必备系统工具：谷歌拼音输入法、垃圾清理工具、截图工具、快速启动工具、软件包管理工具等"
apt-get -y install  fcitx fcitx-googlepinyin flameshot bleachbit gdebi synaptic synapse catfish scrot vokoscreen chromium
echo "[ok] 成功安装系统必备软件!"
echo ""

# Server Tools
echo "[+] 安装服务器连接管理工具：remmina、filezilla"
apt-get -y install remmina filezilla
echo "[ok] 安装服务器连接管理工具成功!"
echo ""

# 美化
echo "[+] 设置窗口按钮到左侧"
gsettings set org.gnome.desktop.wm.preferences button-layout 'close,maximize,minimize:'
echo "[ok] 设置窗口按钮到左侧成功！"
echo ""

echo "[+] 安装中文字体"
apt-get -y install fonts-wqy-microhei fonts-wqy-zenhei
echo "[ok] 安装中文字体成功！"
echo ""

echo "[+] 安装基本美化工具"
apt-get -y install zsh screenfetch neofetch figlet peek
#apt-get -y install cairo-dock
echo "[ok] 安装成功！"
echo ""

echo "[+] 删除无用主题"
cd /usr/share/themes/ && rm -rf Albatross Blackbird Bluebird HighContrast Greybird*
echo "[ok] 删除成功！"

# Security Tools
echo "[+] 安装图形化十六进制编辑器bless"
apt-get -y install bless
echo "[ok] 安装成功！"
echo ""

echo "[+] 安装firewalld防火墙及iptables图形化管理工具gufw "
apt-get -y install gufw firewalld firewall-applet
#systemctl enable firewalld.service
echo "[ok] 安装成功！"
echo ""

# Install sublime text 3
echo "[+] 安装sublime text 3，速度可能会比较慢"
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
apt-get update
apt-get install sublime-text

echo "[+] 解决sublime-text 中文输入问题"
git clone https://github.com/lyfeyaj/sublime-text-imfix.git
cd sublime-text-imfix
cp ./lib/libsublime-imfix.so /opt/sublime_text/ && cp ./src/subl /usr/bin/
echo "[ok] 修复成功。输入subl命令启动sublime text即可输入中文！"
echo ""

# Install typora
echo "[+] 安装 typora，速度可能会比较慢"
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys BA300B7755AFCFAE
echo "deb http://typora.io linux/" | sudo tee /etc/apt/sources.list.d/typora.list
sudo apt-get update -y
sudo apt-get install typora
echo ""


echo "[+] 安装 node npm"
wget https://npm.taobao.org/mirrors/node/v8.9.3/node-v8.9.3.tar.gz
tar zxvf node-v8.9.3.tar.gz && mv node-v8.9.3-linux-x64 /opt
ln -s /opt/node-v8.9.3-linux-x64/bin/node /usr/local/bin/node
ln -s /opt/node-v8.9.3-linux-x64/bin/npm /usr/local/bin/npm
rm ~/node-v8.9.3.tar.gz
echo ""

echo "[+] 清除垃圾 ......"
apt-get clean && apt-get autoclean &&  apt-get autoremove -y　
echo "[+] Cleaning OK!"

# Install oh-my-zsh
# 普通用户就以普通权限安装
apt-get install zsh
echo "[+] Install oh-my-zsh"
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
echo " Setting oh-my-zsh be the default terminal"
chsh -s /bin/zsh
echo ""

neofetch
echo "[OK] 所有任务完成!"


```

