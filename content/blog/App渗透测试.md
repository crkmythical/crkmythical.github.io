---
title: App渗透测试
encrypt: false
enc_pwd: askding
categories:
  - Evolution
tags:
  - null
date: 2022-04-01 19:31:20
top:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [安装Burp证书系统级证书 (>=Android 7.0，需Root)](#org3a5ab9e)
2.  [微信小程序渗透测试流程](#org40251a1)
    1.  [获取微信小程序](#orge937791)
    2.  [下载反编译工具 wxappUnpacker 并反编译小程序](#org55f77a6)
    3.  [代码审计，找出所有API进行测试](#orgec829aa)


<a id="org3a5ab9e"></a>

# 安装Burp证书系统级证书 (>=Android 7.0，需Root)

因Android 7以后，系统不再信任用户级的证书，只能信任系统级的证书，所以需要将burp证书安装到Android系统目录下

1.  获取Burp证书
    
    -   方法一:
        Burp开启代理后，浏览器访问<http://burp> 证书 cacert.der
    
    -   方法二:
    
    ![img](https://raw.githubusercontent.com/askDing/PicGo/main/images/20210524192845burp_ca.png)

1.  证书转换
    
        openssl x509 -inform DER -in cacert.der -out cacert.pem  # 证书格式转换 der --> pem
        openssl version    # 查看openssl版本
        openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1	# 打印证书hash值 9a5ba575  openssl版本在1.0以上执行
        openssl x509 -inform PEM -subject_hash -in cacert.pem	| head -1    # 打印证书hash值  openssl版本在1.0以下执行
        mv cacert.pem <hash>.0         # 将pem格式的证书重命名为  hash值.0
2.  将证书 `9a5ba575.0` 移动到系统证书目录 `/system/etc/security/cacerts`
    
        adb push 9a5ba575.0  /sdcard/    # 推送到sdcard目录上
        adb shell
        su
        mv /sdcard/9a5ba575.0  /system/etc/security/cacerts/    # 将证书移到此目录
        chmod 644 /system/etc/security/cacerts/9a5ba575.0       # 设置权限
        adb reboot                       # 重启生效


<a id="org40251a1"></a>

# 微信小程序渗透测试流程


<a id="orge937791"></a>

## 获取微信小程序

Android 手机最近使用过的微信小程序所对应的 `wxapkg` 包文件都存储在特定文件夹下
~ /data/data/com.tencent.mm/MicroMsg/{User}/appbrand/pkg/~
**{User}** 为当前用户的用户名 `315e07770f778822*********2bfee`

    adb shell
    su
    rm /data/data/com.tencent.mm/MicroMsg/{User}    # 先删除此目录,手机上点开待测的小程序后会重新生成此目录 原因：防止包含其他小程序
    cp -R /data/data/com.tencent.mm/MicroMsg/{User}/appbrand/pkg  /sdcard   # 将小程序目录复制到/sdcard上
    adb pull /sdcard/pkg  ./        # 将小程序拷贝到本地当前目录


<a id="org55f77a6"></a>

## 下载反编译工具 [wxappUnpacker](https://github.com/askDing/wxappUnpacker) 并反编译小程序

1.  下载反编译工具
    
        git clone git@github.com:askDing/wxappUnpacker.git
        cd wxappUnpacker
        ./install.sh -npm  # 安装npm和node
        ./install.sh       # 安装依赖
    
    1.  解包操作
        
            ./de_miniapp.sh -d  path/to/xxx.wxapkg      # 解某个小程序
            ./de_miniapp.sh  /path/to/pkg               # 解pkg目录下所有的小程序
        
        [微信小程序项目目录结构介绍](https://www.wj0511.com/site/detail.html?id=357)
        
            ├── app.js              注册小程序，绑定生命周期回调函数、错误监听和页面不存在监听函数等
            ├── app.json            小程序全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多tab等
            ├── app.wxss            小程序公共样式表
            ├── pages               存放小程序各个页面信息
            │   │── index           
            │   │   ├── index.wxml  必须 页面构造类似html文件
            │   │   ├── index.js    必须 对页面进行注册，指定页面的初始数据、生命周期回调、事件处理函数等
            │   │   ├── index.json  页面窗口配置
            │   │   └── index.wxss  页面样式表相当css文件
            │   └── logs
            │       ├── logs.wxml
            │       └── logs.js
            └── utils               放置一些公用的方法
            │       
            └── sitemap.json        配置小程序及其页面是否允许被微信索引
            │       
            └── project.config.json 小程序项目配置文件
            -------------------------------------------------------
            ├─cloud-functions     ---云函数
            │  └─setCrypto      ---数据加密模块，用户加密一些数据
            │          index.js
            │          package.json
            │          ...
            │          ...
            │          
            ├─components      ---小程序自定义组件
            │  ├─plugins      --- （重点）可独立运行的大型模块，可以打包成plugins
            │  │  ├─comment         ---评论模块
            │  │  │  │  index.js
            │  │  │  │  index.json
            │  │  │  │  index.wxml
            │  │  │  │  index.wxss
            │  │  │  │  services.js    ---（重点）用来处理和清洗数据的service.js，配套模板和插件
            │  │  │  │      
            │  │  │  └─submit    ---评论模块子模块：提交评论
            │  │  │          index.js
            │  │  │          index.json
            │  │  │          index.wxml
            │  │  │          index.wxss
            │  │  │      
            │  │  └─canvasPoster     ---canvas海报生成模块
            │  │          index.js
            │  │          index.json
            │  │          index.wxml
            │  │          index.wxss
            │  │          services.js    ---（重点）用来处理和清洗数据的service.js，配套模板和插件
            │  │          
            │  └─templates   ---（重点）模板，通过外部传参的容器，不做过多的数据处理
            │      │      
            │      ├─slideshow     ---滚屏切换模板
            │      │      index.js
            │      │      index.json
            │      │      index.wxml
            │      │      index.wxss
            │      │      service.js    ---（重点）用来处理和清洗数据的service.js，配套模板和插件
            │      │      
            │      └─works       ---作品模板
            │          │  index.js
            │          │  index.json
            │          │  index.wxml
            │          │  index.wxss
            │          │  service.js
            │          │  
            │          ├─articlePlugin    ---作品模板中的文章类型
            │          │      index.js
            │          │      index.json
            │          │      index.wxml
            │          │      index.wxss
            │          │      
            │          ├─galleryPlugin    ---作品模板中的九宫格类型
            │          │      index.js
            │          │      index.json
            │          │      index.wxml
            │          │      index.wxss


<a id="orgec829aa"></a>

## 代码审计，找出所有API进行测试

1.  查看 `xxx.json` 文件匹配URI(html页面）
    
        {
            "subPackages": [
                {
                    "root": "pages/pageNews/",
                    "pages": [
                        "pages/pageNews/news/notice/index",
                        "pages/pageNews/news/details",
                    ]
                },
                {
                    "root": "pages/pageRetail/",
                    "pages": [
                        "pages/pageRetail/companys/company/company",
                        "pages/pageRetail/companys/companyAdd/companyAdd",
                        "pages/pageRetail/lsms/fghxq"
                    ]
                },
          ]
        }

2.  查看 `xxx.js` 文件，搜索 `module.exports`,找出域名和API接口,进行测试
    
        (function (module, exports, __webpack_require__) {
             "use strict";
        
             var httpUrl = {
                 //本地环境
                 //baseUrl:'http://127.0.0.1:8001/app_name/',
                 //baseysts:'http://127.0.0.1:8004/'
        
                 //开发环境
                 //baseUrl:'http://192.168.1.199:8001/app_name/',
                 //baseysts:'http://192.168.1.199:8004/'
        
                 //安评环境
                 //baseUrl:'http://10.150.86.125:8001/app_name/',
                 //baseysts:'http://10.150.86.125:8004/'
        
                 //正式环境
                 baseUrl: 'https://xx.xx.xx.xx/app_name/'
        
             };
             module.exports = httpUrl;
        
             /***/
         })
        
        (function (module, exports, __webpack_require__) {
             "use strict";
        
             //const base = "http://192.168.1.199:7120/"
             var bases = __webpack_require__(/*! ./https */ "./src/config/https.js");
             var base = bases.baseUrl;
             var baseyst = bases.baseysts;
        
             module.exports = {
                 newsLists: base + 'appnews/news/list', //资讯列表
                 newsDetails: base + 'appnews/news/detail',
                 tbsbqr: base + 'appsbs/refund/tbsbqr', 
                 queren: base + 'appsbs/refund/queren', 
                 ....
                 gang_dong_geo: base + 'appbase/static/js/gang_dong_geo.json' //地图json
             };
         })

References:

-   [小程序开发官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/quickstart/)
-   [微信小程序开发指南](http://www.hcoder.net/books/read/info/1181.html)

