---
title: Android软件测试基础
encrypt: false
enc_pwd: askding
categories:
  - Evolution
tags:
  - null
date: 2022-04-20 21:36:31
top:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [Android App安全基础](#orgef93074)
    1.  [Android APP生成过程](#org6a476ce)
    2.  [安全测试工具](#orgd65fb7b)
        1.  [静态分析工具 - 测试App是否存在防反编译和防篡改的问题](#orge8f77e9)
        2.  [动态分析工具 - 测试防调试、防注入、防内存转储、漏洞测试等问题](#orgb66f7e9)
        3.  [数据包分析工具- 测试数据通信时存在明文传输、数据弱加密、中间人攻击漏洞](#org871ae98)
        4.  [挂钩工具 - 解决加密时上述工具无法使用的问题](#org703824e)
    3.  [App相关的信息资产](#org6e514a2)
2.  [程序代码安全测试 - APP文件、程序进程](#org8721875)
    1.  [运行环境测试](#orgdcbf70e)
    2.  [防反编译测试](#orge5164c6)
        1.  [反编译工具检测](#org8b4c3f1)
        2.  [代码混淆检测](#orge6edb94)
        3.  [混淆强度检测](#org241f12f)
        4.  [关键代码(敏感逻辑和数据保护)检测](#org086bde5)
    3.  [防篡改测试](#org69ea40d)
        1.  [程序文件防篡改检测](#orgb74ed5b)
    4.  [防调试测试](#orgcd438a0)
        1.  [调试工具防护检测](#org0692e69)
        2.  [调试行为防护检测](#org4816187)
        3.  [内存防护检测](#org103f672)
    5.  [防注入测试](#org6e4485b)
        1.  [进程防护检测](#org0649028)
3.  [服务交互安全测试 - 程序进程、前段界面、接口端口](#org98765cf)
    1.  [进程间交互](#org6bdf04a)
        1.  [检测目的](#org7012d36)
        2.  [检测方法](#orge4d99d8)
        3.  [检测结论](#org0f48c98)
        4.  [修复建议](#org0829600)
    2.  [屏幕交互](#org5a97a7a)
        1.  [界面劫持检测](#org5bcf1f3)
        2.  [防截/录屏检测](#org41702b1)
    3.  [WebView交互](#org5f9f4dc)
        1.  [克隆攻击检测](#org8205a3e)
        2.  [WebView安全检测](#orgceee83d)
    4.  [接口端口交互](#org8fb541b)
        1.  [对象反序列化检测](#org4a7dda8)
        2.  [Wormhole漏洞检测](#org6df9c6d)
4.  [鉴权认证安全测试 - 接口端口、内存数据、网络通信](#org414f599)
    1.  [注册过程测试](#orgf61b415)
        1.  [注册信息保护检测](#orge9b7dc3)
        2.  [注册信息传输检测](#org61850ac)
        3.  [注册过程防爆破检测](#org155ab47)
        4.  [注册过程防嗅探检测](#org0f50b35)
    2.  [登陆过程测试](#org1371bd6)
        1.  [密码安全验证检测](#orga447a0e)
        2.  [登陆信息加密传输检测](#orgf9ae294)
        3.  [登陆过程防爆破检测](#org5300a15)
        4.  [登陆过程防嗅探检测](#org771fe4a)
        5.  [登陆过程防绕过检测](#orgbc121bd)
        6.  [加强认证检测](#orgabc9a4b)
    3.  [会话过程测试](#orgfefe1c8)
        1.  [有状态会话标志检测](#orgf7b44a1)
        2.  [无状态会话Token检测](#org9be6f93)
        3.  [会话不活跃检测](#org1ce3ca7)
        4.  [加强认证检测](#org71ebb3f)
    4.  [登出过程测试](#org783c152)
        1.  [会话终止检测](#orga37a056)
        2.  [残留数据检测](#org22db691)
    5.  [注销过程测试](#org13ec53d)
        1.  [重新注册检测](#org643d8f5)
        2.  [数据清除检测](#org0942197)
5.  [本地数据安全测试 - 内存数据、本地存储](#org8c9cd3a)
    1.  [数据创建测试](#org0bb8c18)
        1.  [用户协议检测](#org2f44160)
        2.  [数据采集检测](#orgba2f37e)
        3.  [数据输入检测](#orga315219)
        4.  [数据生成检测](#orga14671a)
    2.  [数据存储测试](#org0e9c3ed)
        1.  [访问控制检测](#org9fc890b)
        2.  [数据加密检测](#org717a8be)
    3.  [数据处理测试](#org80440e9)
        1.  [程序日志检测](#org428e884)
        2.  [敏感数据不当使用检测](#orge80508d)
    4.  [数据共享测试](#org203f9e0)
        1.  [第三方SDK用户协议检测](#orgdcc9854)
        2.  [与第三方SDK数据共享检测](#org4e8592a)
    5.  [数据备份测试](#orgdbf7381)
        1.  [敏感数据备份检测](#orgbd332d3)
        2.  [备份数据加密强度检测](#org1c7a1d8)
    6.  [数据销毁测试](#org1c83575)
        1.  [后台运行数据检测](#org8a07238)
        2.  [敏感数据清除检测](#org57b8027)
6.  [网络传输安全测试 - 网络通信](#org3894cd3)
    1.  [安全传输层测试](#org2412456)
        1.  [TLS实现检测](#orgefeec07)
        2.  [CA证书检测](#orgfb44226)
        3.  [证书校验检测](#org090d933)
        4.  [主机名校验](#org467e66d)
    2.  [数据加密检测](#org764d1f9)
        1.  [检测目的](#org9f7544e)
        2.  [检测方法](#orgb800ad3)
        3.  [检测结论](#orgbdb1f45)
        4.  [修复建议](#orgace36cd)
    3.  [中间人攻击测试](#org30371d5)
        1.  [HTTP中间人会话劫持检测](#orgb561cf4)
        2.  [HTTPS中间人会话劫持检测](#org493ff20)
7.  [App加固技术](#org5b71f45)


<a id="orgef93074"></a>

# Android App安全基础


<a id="org6a476ce"></a>

## Android APP生成过程

    graph LR
        java[.java文件] -->|javac|cls[.class文件]
        cls[.class文件] --> dx[dx]
        jar[.jar文件] --> dx[dx]
        dx[dx] --> dex[class.dex]
        dex[class.dex] -->|dex2oat| oat[oat格式的class.dex文件]
        dex[class.dex] -->aapt[aapt]
        resource[resource] -->aapt[aapt]
        aapt --> .apk文件
        .apk文件 --> jarsigner 
        jarsigner--> zipalign
        zipalign --> signsign[signed .apk文件]

1.  通过~javac~ 将java源代码生成字节码.class文件
2.  通过~dx &#x2013;dex &#x2013;output=class.dex Test.class~将.class文件和jar包生成Android App可执行文件class.dex   
    oat格式文件是android系统自带的ELF文件格式，包括classes.dex文件内容，及classes.dex文件转换的机器指令，存储在~/data/dalbik-cache/arm/data@App@com.demo.test-1@test.apk@classes.dex~
3.  通过~aapt~将.dex文件和其他音视频资源文件打包成.apk文件
4.  通过jarsigner对生成的apk文件进行数字签名，防止APP被篡改
5.  通过zipalign使apk文件压缩部分在字节边界上使对齐


<a id="orgd65fb7b"></a>

## 安全测试工具


<a id="orge8f77e9"></a>

### 静态分析工具 - 测试App是否存在防反编译和防篡改的问题

-   [ ] apktool 检测防篡改能力
    1.  反编译
        
            java -jar apktool.jar d -f Test.apk
    2.  重打包
        
            java -jar apktool.jar b -f directory_name -o Test.apk
            java -jar signapk.jar testkey.x509.pem testkey.pk8   old.apk new.apk
-   [ ] baksmali  作用 classes.dex -反编译-> smali
    
        java -jar  baksmali.jar  classes.dex  -o smalifile

-   [ ] smail 作用 smali &#x2013;> classes.dex
    
        java -jar smali.jar smalifile -o classes.dex
-   [ ] [dex2jar](https://github.com/pxb1988/dex2jar)  作用 dex &#x2013;> jar
    
        sh d2j-dex2jar.sh -f ~/path/to/apk_to_decompile.apk

-   [ ] jd-gui 作用 展示jar包中源码
-   [ ] [jeb](https://bbs.pediy.com/thread-259895.htm) 用于逆向工程/审计apk文件的反编译工具


<a id="orgb66f7e9"></a>

### 动态分析工具 - 测试防调试、防注入、防内存转储、漏洞测试等问题

-   [ ] DDMS(Dalvik debug monitor service)是安卓开发环境中的Dalvik虚拟机调试监控服务
-   [ ] gdb(GNU project debugger)是LInux系统的GCC调试工具
-   [ ] IDA Pro 逆向神器 脱壳
-   [ ] Drozer是一个进行综合安全评估的Android安全测试框架


<a id="org871ae98"></a>

### 数据包分析工具- 测试数据通信时存在明文传输、数据弱加密、中间人攻击漏洞

-   [ ] Burpsuite/Fiddler
-   [ ] Wireshark


<a id="org703824e"></a>

### 挂钩工具 - 解决加密时上述工具无法使用的问题

-   [ ] Xposed框架<sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup> 在不修改APK文件的情况下控制程序运行
-   [ ] Frida 开源的跨平台挂钩框架，用来脱壳关键的函数，达到内存转储的目的
-   [ ] inject App进程注入评测工具


<a id="org6e514a2"></a>

## App相关的信息资产

-   App文件<sup><a id="fnr.2" class="footref" href="#fn.2" role="doc-backlink">2</a></sup>
-   程序进程
-   内存数据
-   前端界面<sup><a id="fnr.3" class="footref" href="#fn.3" role="doc-backlink">3</a></sup>
-   本地存储
-   网络通信
-   交互接口
-   云端平台


<a id="org8721875"></a>

# 程序代码安全测试 - APP文件、程序进程


<a id="orgdcbf70e"></a>

## 运行环境测试<sup><a id="fnr.4" class="footref" href="#fn.4" role="doc-backlink">4</a></sup>


<a id="orge5164c6"></a>

## 防反编译测试<sup><a id="fnr.5" class="footref" href="#fn.5" role="doc-backlink">5</a></sup>


<a id="org8b4c3f1"></a>

### 反编译工具检测

1.  检测目的

    检测App是否可以防止反编译工具，是否具有防逆向保护措施

2.  检测方法

    1.  通过反编译工具对apk文件进行反编译，查看是否具有防逆向保护措施
    2.  通过IDA Pro等反汇编工具对动态库so文件进行反汇编，查看App是否具有防反汇编的能力

3.  检测结论

    若App的dex文件和so文件无法正常反编译或者App经过加固处理，则通过测试

4.  修复建议

    对App文件结构进行变形或加密，让反编译工具无法识别，或对App文件进行加固处理


<a id="orge6edb94"></a>

### 代码混淆检测

1.  检测目的

    检测App反编译后的源码是否经过混淆处理

2.  检测方法

    通过反编译工具对apk文件进行反编译，查看代码中的类、字段和方法是否经过混淆处理

3.  检测结论

    若反编译后源码中的类、字段和方法使用a、b、c、d等无意义的字符重命名，则通过测试

4.  修复建议

    对App源码进行混淆处理


<a id="org241f12f"></a>

### 混淆强度检测

1.  检测目的

    检测App反编译后的源码的混淆强度，查看是否能够有效地保护代码安全

2.  检测方法

    1.  检测dex文件代码中所有的类名、函数名、字段、方法，是否都经过混淆处理，例如反编译后无法正常识别Java层函数的功能
    2.  检测so文件中所有类名、函数名、字段、方法，是否都经过混淆处理，例如反汇编后无法正常识别Native层函数功能

3.  检测结论

    若反编译后代码\*不能识别\*出App函数的功能，则通过测试

4.  修复建议

    针对dex文件和so文件的类名、函数名、字段、方法进行高强度混淆


<a id="org086bde5"></a>

### 关键代码(敏感逻辑和数据保护)检测

1.  检测目的

    检测App是否对关键代码和数据实施有效的保护措施，是否暴露业务逻辑

2.  检测方法

    通过反编译工具对apk文件进行反编译，结合manifest.xml配置文件，分析App注册、登陆、支付过程、加密算法、数据通信等关键功能代码，查看相关代码逻辑是否有明显的暴露

3.  检测结论

    若App关键业务代码(如相关业务字符串)未暴露，且关键数据经过加密和隐藏保护处理，则通过测试

4.  修复建议

    将App关键代码进行隐藏、混淆、加壳等处理，从而无法逆向出重要的代码信息


<a id="org69ea40d"></a>

## 防篡改测试<sup><a id="fnr.6" class="footref" href="#fn.6" role="doc-backlink">6</a></sup>


<a id="orgb74ed5b"></a>

### 程序文件防篡改检测

1.  检测目的

    检测App启动时是否进行了完整性校验，是否对客户端代码、资源文件进行修改，是否具有防篡改机制

2.  检测方法

    1.  使用反编译工具Apktool对目标文件进行反编译
        `java -jar apktool.jar d -f /path/to/test.apk`
    2.  修改相关代码，篡改AndroidManifest.xml、assets文件、res文件等
    3.  使用apktool重新打包签名后再运行App查看运行结果
        `java -jar apktool b -f /path/to/test`

3.  检测结论

    若打包后安装后\*不能正常运行\*，则通过测试

4.  修复建议

    采用完整性校验技术对安装包进行校验，校验对象包括原包中代码、资源文件、配置文件等所有文件，一旦校验失败，立即退出


<a id="orgcd438a0"></a>

## 防调试测试<sup><a id="fnr.7" class="footref" href="#fn.7" role="doc-backlink">7</a></sup>


<a id="org0692e69"></a>

### TODO 调试工具防护检测

1.  检测目的

2.  检测方法

3.  检测结论

4.  修复建议


<a id="org4816187"></a>

### TODO 调试行为防护检测

1.  检测目的

2.  检测方法

3.  检测结论

4.  修复建议


<a id="org103f672"></a>

### 内存防护检测

1.  检测目的

    检测App内存是否具有内存防护功能，防止内存转储

2.  检测方法

    1.  运行App，使用ps命令查看进程PID
    2.  使用 `gdb -p <PID>` 挂载App进程后，使用 `(gdb) gcore` 转储内存

3.  检测结论

    若未生成corefile `core.<pid>` ,则通过测试

4.  修复建议

    通过监控 `/proc/pid/mem` 和 `/proc/pid/pagemep` 来防止内存转储


<a id="org6e4485b"></a>

## 防注入测试<sup><a id="fnr.8" class="footref" href="#fn.8" role="doc-backlink">8</a></sup>


<a id="org0649028"></a>

### 进程防护检测

1.  检测目的

    检测App进程空间是否可以被注入第三方动态so文件

2.  检测方法

    1.  运行App，通过注入工具或脚本，将第三方动态库文件注入App进程空间，查看第三方动态库是否在进程的内存空间中

3.  检测结论

    若第三方动态库文件\*不能注入\*到目标进程空间，则通过测试

4.  修复建议

    -   增加ptrace函数的检测功能，使第三方无法使用该函数附加进程
    -   修改linker中的dlopen函数，防止第三方进行so加载
    -   定时检测App加载的第三方so库，若发现使被注入的so库，程序进程立即报异常


<a id="org98765cf"></a>

# 服务交互安全测试 - 程序进程、前段界面、接口端口


<a id="org6bdf04a"></a>

## 进程间交互


<a id="org7012d36"></a>

### 检测目的

检测进程间数据通信是否具有泄露用户信息的风险


<a id="orge4d99d8"></a>

### 检测方法

查看AndroidManifest.xml文件中的<activity>、<provider>、<receiver>、<service>标签内的exported属性是否为false

-   `<activity android:exported="true"` 则可以被第三方App启动
-   `<provider android:authorities="com.bgy.ssm.fileprovider" android:exported="true"` 则可以被第三方app调用，实现增、删、改、查
-   `<receiver android:exported="true"` 则可以接收第三方App发送的广播消息
-   `<service android:enabled="true" android:exported="true"` 则可以被第三方app启动


<a id="org0f48c98"></a>

### 检测结论

客户端App用于跨进程通信的4种组建分别为Activity、ContentProvider、BroadCast、Service
在 **未明确要求** 的情况下，只要以上配置中存在任一 `exported=true` 则测试 **不通过**


<a id="org0829600"></a>

### 修复建议

在未明确要求的情况下，在AndroidManifest.xml配置文件中设置该组建的exported属性为false，或对组建进行权限


<a id="org5a97a7a"></a>

## 屏幕交互


<a id="org5bcf1f3"></a>

### 界面劫持检测

1.  检测目的

    App是否具有防界面劫持(UI欺骗)功能，防止黑客伪造界面对原有界面进行覆盖，窃取用户和密码等敏感信息

2.  检测方法

    1.  反编译源码，查看是否具有检测程序进入后台运行的代码, 当程序不是因为触摸返回键和HOME键今后后台运行时，提醒用户具有被劫持的风险
        
            @Override
            public boolean onKeyDown(int keyCode, KeyEvent event){
                // 判断程序进入后台运行是否未触摸返回键和HOME键造成的
                if((keyCode == KeyEvent.KEYCODE_BACK || keyCode == KeyEvent.KEYCODE_HOME)
                   && event.getRepeatCount()==0){
                    flag = false;
                }
                return super.onKeyDown(keyCode, event);
            }
            
            @Override
            protected void onPause(){
                // 程序进入后台如果不是触摸返回键和HOME键造成的，则进行劫持风险提示
                if(flag){
                    // 弹出警示信息
                    Toast.makeText(getApplicationContext(),
                                   "程序已经进入后台运行，具有劫持的风险",
                                   Toast.LENGTH_SHORT).show();
                }
                super.onPause();
            }
    
    2.  编写透明界面，当运行至登陆、支付等界面时进行覆盖，查看是否具有风险提示

3.  修复建议

    对App的UI界面进行校验，强制将自身UI时刻设置成顶层显示，其中HOME键除外，或自身UI界面进入后台运行后弹框提示用户App已经进入后台运行，有界面劫持风险


<a id="org41702b1"></a>

### 防截/录屏检测

1.  检测目的

    检测App运行后是否存在防截/录屏保护措施

2.  检测方法

    1.  通过 `screencap` 命令进行连续截屏，查看界面后的图片是否显示敏感信息
    2.  通过 `screenrecord` 命令进行录屏，查看录制后的视频是否显示敏感信息

3.  修复建议

    App要实现防截录屏的保护措施


<a id="org5f9f4dc"></a>

## WebView交互


<a id="org8205a3e"></a>

### 克隆攻击检测

1.  检测目的

    检测App中是否存在设置为可被导出的Activity组件，且组建中包含WebView调用，存在导致敏感信息泄露的风险

2.  检测方法

    1.  通过JEB攻击反编译dex文件的源码，查看客户端是否使用了WebView空间，并将 `setAllowFileAccessFromeFileURLs` 或 `setAllowUninvertedFileAccessFromFileURLs` API设置为 `true`
    2.  检测file域的路径是否做了严格限制

3.  检测结论

    若App使用WebView组建，并将 `setAllowFileAccessFromeFileURLs` 或 `setAllowUninvertedFileAccessFromFileURLs` API设置为 `false` ，则通过测试
    若App使用WebView组建，并将 `setAllowFileAccessFromeFileURLs` 或 `setAllowUninvertedFileAccessFromFileURLs` API设置为 `true` ，且file域的路径做了严格限制，则通过测试

4.  修复建议

    严格限制包含WebView调用的Activity组建的导出权限，关闭导出权限或限制导出组建的发起者


<a id="orgceee83d"></a>

### WebView安全检测

1.  检测目的

    检测App使用的WebView空间加载的外部资源是否存在潜在风险

2.  检测方法

    检测App源码，查看客户端是否对WebView对空间加载的资源文件进行了校验，过滤风险代码

3.  检测结论

    App使用WebView空间减灾的HTML未明确要求使用Javascript，
    未对加载文件进行校验，或未使用安全的通信协议，
    并在WebView加载的程序中有实现发送短信，拨打电话等敏感行为的操作代码，则本项测试不通过

4.  修复建议

    -   WebView加载的HTML页面，在未明确要求的情况下，禁用Javascript
    -   对WebView加载的外部文件进行校验
    -   采用HTTPS安全通信协议，不要在WebView加载的外部文件中实现敏感操作的代码


<a id="org8fb541b"></a>

## 接口端口交互


<a id="org4a7dda8"></a>

### 对象反序列化检测

1.  检测目的

    检测App是否使用安全的API实现序列化和反序列化，是否存在反序列化漏洞

2.  检测方法

    1.  通过JEB工具反编译dex文件的源码，查看客户端App是否具有实现序列化和反序列化的源代码
    2.  检测实现序列化和反序列化的API是否具有潜在风险和漏洞

3.  检测结论

    若客户端App不具有序列化和反序列化的代码，或实现序列化和反序列化的API不具有潜在风险和漏洞，则通过测试

4.  修复建议

    App采用安全框架的API实现序列化和反序列化


<a id="org6df9c6d"></a>

### Wormhole漏洞检测

1.  检测目的

    检测App是否存在Wormhole漏洞

2.  检测方法

    1.  检测App是否私自开启HTTP服务，是否进行身份认证
    2.  通过nmap工具扫描，检测App代码中是否开放某个TCP端口

3.  检测结论

    如App私自开启了HTTP服务，开放某个TCP端口，同时该服务无身份认证，则本测试 **不通过** 

4.  修复建议

    App关闭HTTP服务和端口，增加App的访问权限控制机制


<a id="org414f599"></a>

# 鉴权认证安全测试 - 接口端口、内存数据、网络通信


<a id="orgf61b415"></a>

## 注册过程测试


<a id="orge9b7dc3"></a>

### 注册信息保护检测

1.  检测目的

    检测App注册密码的复杂度(密码内容要求字母大小写、数字、特殊字符组合，长度等)和注册信息在本地存储时的保护程度是否足够高

2.  检测方法

    1.  检测App注册密码的复杂度
        
            public static boolean isPasswordChecked(CharSequence data){
                return Pattern.compile("^((a-z0-9A-Z)+[_]?){6,20}$").matcher(data).find();
            }
    2.  检测App在本地存储的注册信息是否加密存储，加密密钥是否进行了隐藏处理

3.  检测结论

    如果对注册密码复杂度、长度进行了限制处理，且对本地存储的注册信息进行了加密保护，加密密钥隐藏，则通过测试

4.  修复建议

    -   对注册密码的复杂度和长度进行限制
    -   对在本地存在的用户注册信息进行加密处理，隐藏加密密钥


<a id="org61850ac"></a>

### 注册信息传输检测

1.  检测目的

    检测App将用户注册信息传输到服务器端的过程中是进行了加密保护

2.  检测方法

    使用Burpsuite拦截App注册用户的数据包，查看数据包是否加密

3.  检测结论

    若App在将用户注册信息传输到服务器端时进行了加密处理，则通过测试

4.  修复建议

    在将用户注册信息传输到服务器端的过程中，对用户注册信息进行加密处理


<a id="org155ab47"></a>

### 注册过程防爆破检测

1.  检测目的

    检测App在注册账户时，是否可以爆破获取正确的验证码，注册任意用户

2.  检测方法

    在注册界面填写完注册信息后，点击&ldquo;获取验证码&rdquo;，使用抓包工具对其抓包，对数据包中的验证码进行暴力破解，爆破成功后，便可注册任意账号

3.  检测结论

    若在注册App时验证码被爆破，可以任意注册账户，则本项测试不通过

4.  修复建议

    -   使用复杂的验证码，验证码长度不低于6位，包含数字及字母
    -   对发送验证码请求进行时间和次数限制
    -   验证码在传输时进行有效的加密处理


<a id="org0f50b35"></a>

### 注册过程防嗅探检测

1.  检测目的

    检测在App注册过程中是否可以利用已有社工库中的手机号、邮箱、用户名、密码等信息，通过撞库方式频繁嗅探注册账户

2.  检测方法

    利用Burpsuite拦截注册用户时的数据包，分析查看是否暴露账户、密码参数，然后利用社工库数据替换账户、密码参数，进行撞库，从而获取用户注册

3.  检测结论

    若在注册时暴露账户、密码参数，具有利用撞库对用户注册信息进行嗅探的风险，则本项测试不通过

4.  修复建议

    -   对传输的注册账户、密码等敏感信息进行强加密处理
    -   服务器端限制访问次数


<a id="org1371bd6"></a>

## 登陆过程测试


<a id="orga447a0e"></a>

### 密码安全验证检测

1.  检测目的

    检测App登陆密码的验证方案是在本地验证还是在服务器端验证，验证过程中是否加入了设备信息

2.  检测方法

    利用JEB逆向分析App登陆功能的源码，分析密码验证中是否加密了设备信息IMEI

3.  检测结论

    若密码验证在服务器端进行，且加入了设备信息，避免在非法设备登陆，则通过测试

4.  修复建议

    App登陆密码在服务器端进行验证，并加入设备信息，以降低用户登陆密码泄露的风险


<a id="orgf9ae294"></a>

### 登陆信息加密传输检测

1.  检测目的

    检测App在将用户登录信息传输到服务器端的过程中是否进行了加密保护，以免被攻击者拦截网络流量，窃取用户登陆信息

2.  检测方法

    使用Burpsuite拦截app登陆操作的数据包，分析是否明文传输用户信息

3.  检测结论

    若App在将用户注册信息传输到服务器端时进行了加密处理，则通过测试

4.  修复建议

    App在将用户登陆信息传输到服务器端的过程中，要对用户登陆信息进行加密处理


<a id="org5300a15"></a>

### 登陆过程防爆破检测

1.  检测目的

    检测App在登录时，是否可以抓取数据包，利用数据包中的验证码字段/密码字段进行暴力破解

2.  检测方法

    检测App在登陆时，是否可以爆破验证码/密码，获取正确的验证码和登陆密码

3.  检测结论

    若App在登陆时验证码和登陆密码可以被爆破，则本项测试不通过

4.  修复建议

    -   使用复杂的验证码和登陆密码
    -   对发送验证码的请求进行时间和次数限制
    -   对验证码、登陆密码进行输入错误次数限制
    -   验证码、登录密码在传输时进行有效的加密处理


<a id="org771fe4a"></a>

### 登陆过程防嗅探检测

1.  检测目的

    检测App是否可以通过爆破验证码实现任意账户登陆、任意重置用户密码等操作

2.  检测方法

    1.  验证码爆破检测
        在登陆界面填写完手机号等信息后点击“获取验证码”，使用抓包工具对其抓包，对数据中的验证码进行暴力破解，爆破成功后便可实现登陆任意账户、任意重置用户密码
    2.  短信轰炸检测
        在登陆界面填写完手机号等信息后点击“获取验证码”，若短信验证码无获取时间、获取次数限制，便可重放发送短信验证码数据进行短信轰炸
    3.  探测是否具有撞库风险
        利用工具拦截用户登陆时的数据包，分析查看是否暴露账户、密码参数，然后利用社工库数据替换账户、密码参数进行撞库，从而获取用户登陆信息

3.  检测结论

    若App通过爆破验证码实现登陆任意账户、任意重置用户密码、短信炸弹，或通过撞库获取用户登陆信息，则此项测试 **不通过** 

4.  修复建议

    -   使用复杂验证码、登陆密码
    -   对发送验证码的请求进行时间和次数限制
    -   对验证码输入错误次数进行限制
    -   验证码在传输时进行有效的加密处理
    -   服务端限制访问次数


<a id="orgbc121bd"></a>

### 登陆过程防绕过检测

1.  检测目的

    检测App是否可以绕过验证码登陆任意账户、修改用户ID获取其他用户信息

2.  检测方法

    1.  抓取登陆成功后的响应包，之后退出，在登陆其他用户账户时，用登陆成功的响应包替换登陆失败的数据包，检测是否可以绕过验证码、密码验证，进而成功登陆其他用户的账户
    2.  修改用户ID，检测是否可以获取任意用户信息，若用户身份认证采用单一ID值判断，攻击者可以修改数据包中的用户ID进行重放，从而获取其他用户信息

3.  检测结论

    若App可以绕过验证码登陆其他用户，或可以修改用户ID获取其他用户信息，则本项测试 **不通过**

4.  修复建议

    加强身份验证机制，使用Token或Session机制，设置访问控制策略，敏感数据采用高强度加密传输


<a id="orgabc9a4b"></a>

### 加强认证检测

1.  检测目的

    检测App客户端是否具有双因子认证机制，保护用户登陆信息

2.  检测方法

    1.  检测App在登陆时，是否具有双因子认证机制(密码+令牌/指纹/设备信息/短信)
    2.  使用用户登陆信息在新设备登陆时，查看是否具有短信提醒

3.  检测结论

    如Apple具有双因子认证机制和不同设备登录时的短信提醒认证机制，则通过测试

4.  修复建议

    App采用双因子认证机制和不同设备登陆短信提醒认证机制，保护用户登陆信息安全


<a id="orgfefe1c8"></a>

## 会话过程测试


<a id="orgf7b44a1"></a>

### 有状态会话标志检测

1.  检测目的

    检测客户端与服务器端交互的会话，是否存在复杂的会话ID，同时服务器是否对其进行校验

2.  检测方法

    1.  模拟客户端与服务器端登陆，查看是否采用 **简单的<sub>Session</sub><sub>id方式标识</sub>** 客户端
    2.  利用服务端返回的 **\_Session<sub>id构建新的URL</sub>** 访问服务器端，查看是否能够绕过验证
    3.  查看客户端与服务器交互时是否采用 \*复杂的Key\*，是否存在时间有效性校验，防止被伪造

3.  检测结论

    若客户端与服务器端会话时采用来复杂加密的Key，同时服务器端对客户端发送的Key进行了校验，攻击这无法伪造，服务器端无响应，则通过测试

4.  修复建议

    客户端与服务器端通信会话时采用复杂的算法对随机的<sub>Session</sub><sub>id进行加密</sub>，同时服务器端对随机的<sub>Session</sub><sub>id进行校验</sub>


<a id="org9be6f93"></a>

### 无状态会话Token检测

1.  检测目的

    在客户端与服务器端通信会话过程中，检测是否存在Token机制，是否容易被攻击者截取利用

2.  检测方法

    1.  检测客户端与服务器端通信会话的URL中是否使用携带Token，Token是否明文显示
    2.  检测客户端与服务器端认证的复杂性，是否采用类似 **UID+Toekn+timestamp+密钥** 的Toekn机制，并尝试破解

3.  检测结论

    若客户端与服务器端通信会话的Toekn能被轻易获取利用或被破解，则本项测试不通过

4.  修复建议

    -   每次登陆时重新生成Token，并设置有效期，每次操作后更新Token的时间戳，保证Token有效期往后延续
    -   为了避免Token被截获，伪造非法请求，在每次请求时，建议采用 **UID+Token+timestamp+密钥+请求参数签名** ，服务器同时验证Token和签名，以保证请求的安全性


<a id="org1ce3ca7"></a>

### 会话不活跃检测

1.  检测目的

    若客户端与服务器端通信临时终端或长时间不活跃，检测服务器是否立即终止会话

2.  检测方法

    1.  在客户单与服务器端通信过程中，若长时间不操作，然后再操作时，查看客户端与服务器端是否已中断
    2.  在客户单与服务器端通信过程中，若临时中断，例如打开微信等其他操作，让服务在后台运行，查看客户端与服务器端是否已中断

3.  检测结论

    若客户端与服务器端通信临时中断或长时间不活跃时，服务器端立即与客户端中断会话，需要重新认证，则本项测试结果为通过

4.  修复建议

    在客户端与服务器端通信会话过程中，增加时间的有效性，例如设置时间为5min，若客户端与服务器长时间不活跃或者客户端服务在后台运行，服务器立即中断本次会话


<a id="org71ebb3f"></a>

### 加强认证检测

1.  检测目的

    在客户端与服务器端进行敏感交易时，检测服务器端是否存在双因子身份认证机制

2.  检测方法

    1.  检测客户端与服务器端进行支付、转账等敏感交易时，客户端是否需要多个身份认证方式，同时服务器端是否对其双因子进行校验
    2.  检测客户端与服务器端进行身份认证过程中，数据是否进行加密处理，加密强度如何

3.  检测结论

    若客户端与服务器端存在双因子身份认证，则通过测试

4.  修复建议

    App中涉及敏感用户信息的界面，要求使用双因子身份认证机制，例如采用支付密码和用户预留短信验证码等认证方式


<a id="org783c152"></a>

## 登出过程测试


<a id="orga37a056"></a>

### 会话终止检测

1.  检测目的

    在用户执行登出操作后，检测服务器端是否立即终止与客户端之间的会话连接

2.  检测方法

    1.  登陆App，执行一些需要在App进行身份验证的操作，并拦截
    2.  退出App
    3.  重放步骤1中的操作，显示错误消息或重定向到登录页面

3.  检测结论

    若在客户端用户执行登出操作后，服务器立即终止与客户端之间的会话连接，需要用户重新进行登陆认证

4.  修复建议

    在用户执行登出操作后，立即终止客户端与服务器端的会话连接


<a id="org22db691"></a>

### 残留数据检测

1.  检测目的

    当用户执行登出操作后，检测服务器是否及时删除客户端对应的Token或<sub>Session</sub><sub>id</sub>

2.  检测方法

    操作客户端登出功能，通过用户名、密码、及之前的Token值或<sub>Session</sub><sub>id值能够登陆成功</sub>，则本项测试不通过

3.  检测结论

    在用户执行登出操作后，若客户端使用之前的Token或<sub>Session</sub><sub>id值能够登陆成功</sub>，则本项测试不通过

4.  修复建议

    当用户执行登出操作后，服务端及时删除户端对应的Token或<sub>Session</sub><sub>id值</sub>


<a id="org13ec53d"></a>

## 注销过程测试


<a id="org643d8f5"></a>

### 重新注册检测

1.  检测目的

    检测在客户端注销后，使用相同账户能否重新注册

2.  检测方法

    1.  检测客户端是否存在注销功能
    2.  在客户单注销之后，使用相同账户注册，查看是否可以重新注册，测试第三方关联的账户是否也已经注销，还能否正常登陆

3.  检测结论

    若注销账户后仍可以使用相同的账户注册，关联的第三方数据无法使用，则通过测试

4.  修复建议

    在客户端注销操作后，可以使用相同账户重新注册，确认原来的账户信息已经清除


<a id="org0942197"></a>

### 数据清除检测

1.  检测目的

    检测在App卸载后，本地存储的数据或账户缓存等信息是否全部清除

2.  检测方法

    1.  安装App，先注册，登录试用，然后卸载，查看本地注册的用户账户信息等数据是否及时删除
    2.  重新安装App，查看使用之前的账户和密码是否可以直接登陆

3.  检测结论

    若卸载App后，本地数据完全及时清除，则通过测试

4.  修复建议

    在App卸载后，及时删除本地存储的全部数据


<a id="org8c9cd3a"></a>

# 本地数据安全测试 - 内存数据、本地存储


<a id="org0bb8c18"></a>

## 数据创建测试


<a id="org2f44160"></a>

### 用户协议检测

1.  检测目的

    检测App是否存在用户协议声明。若存在，是否对使用用户信息用户及保护措施进行声明，是否存在违规行为

2.  检测方法

    1.  安装运行App，试用App的所有主要功能，并抓包，通过数据包和源代码了解其行为特征
    2.  查看是否存在用户协议，以及协议内容是否声明App需要手机的用户信息及保护措施

3.  检测结论

    若App存在用户服务协议，且声明了用户信息用途及保护措施，则通过测试

4.  修复建议

    App手机用户个人信息前，必须在用户服务协议中声明，需要收集用户设备的哪些信息、具体用途、及保护用户信息的安全措施和具体承诺


<a id="orgba2f37e"></a>

### 数据采集检测

1.  检测目的

    检测App是否过度申请系统敏感权限，使用该权限时是否提示用户授权，是否过度手机用户数据，数据传输过程是否安全

2.  检测方法

    1.  查看AndroidManifest.xml文件<users-permission>标签，分析App所申请的系统权限，是否存在过度申请的敏感权限
    2.  查看App调用系统的敏感权限时是否提示用户授权
    3.  分析App源码及数据包内容，查看是否过度收集在用户协议声明范围外的用户数据，确认数据传输过程中的安全性

3.  检测结论

    若App无过度申请系统敏感权限，且在使用该权限时提示用户授权，同时没有过度收集用户数据，则通过测试

4.  修复建议

    App发布时需要删除不需要的系统敏感权限，在申请系统敏感权限时，需要提示用户授权，不得私自上传在协议中未声明的用户信息


<a id="orga315219"></a>

### 数据输入检测

1.  检测目的

    检测App是否实现了自带的安全键盘，且启动键盘时数字是否随机分布，关键的输入框是否禁用复制粘贴功能，是否存在验证码校验机制，验证码是否安全

2.  检测方法

    1.  检测App是否实现了自定义软键盘，在每次启动时按键数字随机分布，且按键时不存在按键阴影，按键回显等特效
    2.  检测要求输入敏感数据(登陆密码、支付密码、银行卡账户等)输入框禁用复制粘贴功能
    3.  检测验证码是否由图形验证码或短信验证码组成，是否通过服务器端返回给客户端

3.  检测结论

    若App实现了自定义软键盘，键盘数字实现了随机分布，具有安全的验证码，同时密码输入框禁用了复制粘贴功能，则通过测试

4.  修复建议

    -   App客户端实现自定义的软键盘，软键盘每次启动时都要随机分布，且按键无回显、阴影等特效
    -   要求输入登陆、支付密码、银行卡账户等输入框禁用复制粘贴功能 设置 `android:longClickable="false"` 关闭其功能
    -   增加复杂图形验证码或短信验证码，且在传输过程中对数据进行加密


<a id="orga14671a"></a>

### 数据生成检测

1.  检测目的

    检测App生成数据的存储形式时是结构话还是非结构话，数据是否经过加密后存储

2.  检测方法

    1.  检测App生成的结构化数据，要求数据内容加密后存储
    2.  检测App生成的非结构化数据，要求数据内容加密后存储

3.  检测结论

    若本地存储的数据经过加密处理，则通过测试

4.  修复建议

    不管生成的数据是采用结构化还是非结构化形式存储，都要求加密后存储


<a id="org0e9c3ed"></a>

## 数据存储测试


<a id="org9fc890b"></a>

### 访问控制检测

1.  检测目的

    检测App是否具备完善的权限管理机制，是否能够与其他App隔离，是否在权限允许的范围之外存在数据被其他客户端访问的风险

2.  检测方法

    查看App本地存储文件的权限
    
        ls -al files        # 本地存储file文件权限
        ls -al shared_pref  # 本地存储xml文件权限
        ls -al app_webview  # 本地存储的cache文件权限
        ls -al databases    # 本地存储的db文件权限

3.  检测结论

    如客户端具备完善的权限管理机制，以最小权限为原则，则通过测试

4.  修复建议

    App客户端严格控制本地生成敏感数据访问权限，避免被第三方App非法访问导致用户信息泄露


<a id="org717a8be"></a>

### 数据加密检测

1.  检测目的

    检测App在本地存储的用户信息是否经过了加密处理，加密密钥是否进行了保护，加密算法是否合理，生成的随机数强度是否较高，避免造成用户信息泄露风险

2.  检测方法

    检测App在本地生成的数据文件是否加密，检测App在本地存储的文件是否加密

3.  检测结论

    若本地数据进行了加密处理，加密密钥进行了保护处理，且采用多种加密算法组合加密，对不同的数据采用了不同的加密算法，采用安全的方式生成随机数，则通过测试

4.  修复建议

    -   对App在本地存储的用户信息进行加密处理
    -   对对称加密算法的加密密钥进行加密保护和隐藏处理
    -   对APp在本地存储的用户信息进行多重加密，并对用户数据采用多种加密方式
    -   避免使用不安全的随机数生成类
    -   避免使用不安全的加密算法


<a id="org80440e9"></a>

## 数据处理测试


<a id="org428e884"></a>

### 程序日志检测

1.  检测目的

    检测App源码中的调试信息是否关闭，在调试信息中是否写入敏感信息

2.  检测方法

    1.  反编译源码，查看是否存在日志调试代码，要求不得存在日志调试代码
        
            private void save(){
                String mName=etUsername.getText().toString();
                String mPwd=etPwd.getText().toString();
                mEditor.putString("Name",mName);
                mEditor.putString("Pwd",mPwd);
                mEditor.commit();
                Log.d("TEST","本地存储"+"用户名"+mName+"密码"+mPwd);
            }
    2.  动态运行App客户端，使用~logcat~查看后台打印日志是否存在用户敏感数据，要求后台不得打印日志调试信息

3.  检测结论

    若App关闭了源码中的调试信息，则通过测试

4.  修复建议

    App发布时应删除源码中的日志调试代码


<a id="orge80508d"></a>

### 敏感数据不当使用检测

1.  检测目的

    检测App源码和行为特征是否符合App安全相关标准的规定

2.  检测方法

    反编译App代码，查看是否私自手机用户敏感信息，抓包拦截，检测是App是否私自上传用户隐私到服务器

3.  检测结论

    逆向分析源码和数据包，若符合App安全相关标准的规定，则通过测试

4.  修复建议

    App源码要进行严格审核处理，禁止在用户未知情的情况下私自收集用户信息


<a id="org203f9e0"></a>

## 数据共享测试


<a id="orgdcc9854"></a>

### 第三方SDK用户协议检测

1.  检测目的

    检测在App服务协议中是否声明第三方SDK收集用户信息的用途，是否过度收集用户个人信息

2.  检测方法

    查看用户协议内容是否声明共享用户信息给第三方SDK，并通过抓包查看第三方SDK的行为特征

3.  检测结论

    若用户协议中明确声明App信息与第三方共享情况，则通过测试

4.  修复建议

    App要明确声明是否会与第三方共享用户信息，以及共享用户信息的具体用途


<a id="org4e8592a"></a>

### 与第三方SDK数据共享检测

1.  检测目的

    检测App是否在用户未知情的情况下，私自共享用户个人信息给第三方SDK，以及第三方SDK是否私自收集用户个人信息到指定服务器

2.  检测方法

    1.  分析App源码和数据包，查看是否在用户未知情的情况下，将收集的用户信息私自上传至第三方服务器
    2.  分析App嵌入的第三方SDK源码和数据包，查看是否存在第三方SDK在用户不知情的情况下，将收集的用户信息私自上传至第三方服务器

3.  检测结论

    若分析源码内容和数据包后，符合App安全相关标准的规定，则通过测试

4.  修复建议

    在App共享数据给第三方SDK的服务协议之外，禁止App和第三方SDK私自采用短信或数据包等形式，收集用户信息并上传到指定服务器


<a id="orgdbf7381"></a>

## 数据备份测试


<a id="orgbd332d3"></a>

### 敏感数据备份检测

1.  检测目的

    检测App应用数据是否可以备份，是否能够防止攻击者复制App数据

2.  检测方法

    查看Androidmanifest.xml文件中的allowBackup属性是否为true
    `<application android:allowBackup="true"`

3.  检测结论

    在App不具备备份功能的情况下，若 `<application android:allowBackup="false"` 则通过测试

4.  修复建议

    在App不具备备份功能的情况下，应将 `<application android:allowBackup="false"` 


<a id="org1c7a1d8"></a>

### 备份数据加密强度检测

1.  检测目的

    检测App备份的数据是否进行加密处理，并且要求使用复杂的加密强度高的算法

2.  检测方法

    若采用对称算法加密，则判断对称算法的密码是否存储安全，加密算法的源代码是否可以被破解

3.  检测结论

    若App备份的数据进行了加密处理，则通过测试

4.  修复建议

    采用多种混合算法加密，例如AES256,MD5,HASH,DES,BASE64


<a id="org1c83575"></a>

## 数据销毁测试


<a id="org8a07238"></a>

### 后台运行数据检测

1.  检测目的

    检测App客户端在切入后台运行时是否对收集存储的文件、数据库、配置文件、缓存文件等进行及时清理操作

2.  检测方法

    1.  App切入后台运行时，查看本地生成的db文件、xml文件或者内存中的数据是否进行了删除
    2.  导出本地的缓存信息文件，查看是否有敏感信息暴露

3.  检测结论

    若App在切入后台运行时，本地生成的临时文件db、xml、cache中的数据或者运行时内存中的用户数据做到了及时清理，则通过测试

4.  修复建议

    App在切入后台运行后，应及时清理本地存储的用户敏感信息和内存中的数据信息


<a id="org57b8027"></a>

### 敏感数据清除检测

1.  检测目的

    检测App在退出或被卸载时，是否彻底删除在手机本地存储的文件、数据库、缓存、配置信息等文件

2.  检测方法

    使用反编译工具apktool对目标文件进行反编译，查看App代码中是否具有清除缓存信息的方法 `removeSessionCookie()/deleteCookie()`
    
        if("ClearWebView" , "webView.clearCache"){
            try{
                CookieSyncManager.createInstance(this.Y.getApplicationContext());
                CookieSyncManager.getInstance().removeSessionCookie();
                CookieSyncManager.getInstance().sync();
            }catch(Exception v0_1){
        
            }
        }

3.  检测结论

    若本地生成文件仍然存在，则本项测试 **不通过**

4.  修复建议

    检测App在退出或被卸载时，应彻底删除在手机本地存储的文件、数据库、缓存、配置信息等信息


<a id="org3894cd3"></a>

# 网络传输安全测试 - 网络通信


<a id="org2412456"></a>

## 安全传输层测试


<a id="orgefeec07"></a>

### TLS实现检测

1.  检测目的

    检测客户端与服务器端交互核心的通信会话是否采用HTTPS，同时是否为现有最佳实践方式

2.  检测方法

    使用Burpsuite/Wireshark抓包，判定用户登录、交易等私密连接是否使用HTTPS进行网络通信，查看TLS版本是否高于1.0

3.  检测结论

    若客户端与服务器端通信采用HTTPS，且TLS版本高于1.0，则通过测试

4.  修复建议

    客户端与服务器端核心的通信会话均采用HTTPS，同时TLS版本高于1.0


<a id="orgfb44226"></a>

### CA证书检测

1.  检测目的

    检测客户端与服务器建立安全通道时，客户端是否验证远程端点的X.509证书，是否只接受受信任的CA签名证书

2.  检测方法

    检测CA证书的合法性，是否为受信任的CA签名证书，App是否只接受受信任的CA签名证书
    
    1.  抓取App与服务器端交互的数据，校验证书的颁发机构
    2.  在源码中检查客户端是否只接受受信任的CA签名证书

3.  检测结论

    若截获的数据包中证书由可信任机构签发，且在有效期内，且访问服务器与证书绑定的一致，同时只接受信任的CA签名证书，则通过测试

4.  修复建议

    客户端验证远程端点的X.509证书，只接受受信任的CA签名的证书


<a id="org090d933"></a>

### 证书校验检测

1.  检测目的

    检测客户端和服务器是否对证书进行双向校验

2.  检测方法

    1.  反编译App代码，检测是否存在客户端验证服务器端证书的代码
        -   验证证书内容有效性、数字摘要是否一致
    2.  反编译App代码，检测是否存在客户端发送本地证书给服务器端认证的代码

3.  检测结论

    若客户端对服务器端返回的证书进行了验证，同时服务器端也对客户端证书进行了校验，则通过测试

4.  修复建议

    建议一般的App要实现客户端对服务器端证书的单向验证，对于安全要求比较高的App，要实现客户端与服务器端证书的双向验证


<a id="org467e66d"></a>

### 主机名校验

1.  检测目的

    检测客户端是否对主机名进行校验

2.  检测方法

    反编译App代码，查找App通信的代码，查看 `setHostnameVerifier()` 方法接受任意域名还是进行了主机名验证
    
        public static SSLSocketFactory getFixedSocketFactory(){
            MySSLSocketFactory v0;
            try{
                v0=new MySSLSocketFactory(MySSLSocketFactory.getKeystore());
                //缺陷代码
                ((SSLSocketFactory)v0).setHostnameVerifier(SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);
            }catch(Throwable v1){
                v1.printStackTrace();
                SSLSocketFactory v0_1 = SSLSocketFactory.getSocketFactory();
            }
            return ((SSLSocketFactory)v0);
        }

3.  检测结论

    若App接受任意域名，则本项测试 **不通过**

4.  修复建议

    App对主机名进行校验，不能接受任意域名


<a id="org764d1f9"></a>

## 数据加密检测


<a id="org9f7544e"></a>

### 检测目的

检测在客户端与服务器端通信过程中，业务数据是否以明文方式在网络中传输，数据加密的复杂度如何


<a id="orgb800ad3"></a>

### 检测方法

1.  对客户端与服务器端通信的登陆、支付、转账等核心功能进行抓包，查看业务数据是否以明文方式在网络中传输
2.  检测数据加密方式的复杂度，url编码、Base64编码、AES\DES加密等


<a id="orgbdb1f45"></a>

### 检测结论

若客户端与服务器端交互的业务数据经过多个复杂的算法加密，且无法破解，则通过测试


<a id="orgace36cd"></a>

### 修复建议

客户端与服务器端交互的上行/下行数据要经过多个复杂算法进行加密，同时加密存储对称加密算法密钥


<a id="org30371d5"></a>

## 中间人攻击测试


<a id="orgb561cf4"></a>

### HTTP中间人会话劫持检测

1.  检测目的

    检测客户端与服务器端交互的数据是否可以被任意篡改，导致重放攻击漏洞

2.  检测方法

    运行测试App，点击登陆，拦截数据包，并进行修改后放行，查看App运行结果是否能够修改成功

3.  检测结论

    若客户端与服务器端交互的数据经过加密处理，且数据无法修改，则通过测试

4.  修复建议

    -   采用高强度的加密算法对交互数据进行加密/或使用HTTPS
    -   对客户端请求的数据和服务器端返回的数据进行完整性校验，防止被篡改


<a id="org493ff20"></a>

### HTTPS中间人会话劫持检测

1.  检测目的

    检测App在使用HTTPS时，是否存在中间人攻击漏洞

2.  检测方法

    1.  反编译源码，查看是否校验服务器端是否可信- 查看实现X509TrustManager接口中 `checkServerTrusted()` 方法实现是否为空
        
            public MySSLSocketFactory(KeyStore truststore) throws NoSuchAlgorithmException,
                                                                  KeyManagementException{
                this.sslContext = SSLContext.getInstance("TLS");
                this.sslContext.init(null,new TrustManager[]{new X509TrustManager(){
                        public X509Certificate[] getAcceptedIssuers(){
                            return null;
                        }
                        public void checkServerTrusted(X509Certificate[] arg0, String arg1) throws CertificateException{
                            //实现逻辑为空
                        }
                        public void checkClientTrusted(X509Certificate[] arg0, String arg1) throws CertificateException{
                            //实现逻辑为空
                        }
            
                    }},null);
            }
    
    2.  反编译源码，查看站点域名与站点证书的域名是否匹配- 查看 `HostnameVerifier()` 方法中的 `verify()` 函数是否存在域名校验
    
        NetworkUtils.conn=null;
        NetworkUtils.is =null;
        NetworkUtils.os=null;
        NetowrkUtils.DO_NOT_VERIFY = new HostanmeVerifier(){
                public boolean verify(String s, SSLSession sslSession){
                    return 1; // 不检查站点域名和站点证书的域名
                }
            }
    
    1.  查看 `sethostnameverifier()` 方法是否接受任意域名
        
            public static SSLSocketFactory getFixedSocketFactory(){
                MySSLSocketFactory v0;
                try{
                    v0=new MySSLSocketFactory(MySSLSocketFactory.getKeystore());
                    //缺陷代码
                    ((SSLSocketFactory)v0).setHostnameVerifier(SSLSocketFactory.ALLOW_ALL_HOSTNAME_VERIFIER);
                }catch(Throwable v1){
                    v1.printStackTrace();
                    SSLSocketFactory v0_1 = SSLSocketFactory.getSocketFactory();
                }
                return null;
            }

3.  检测结论

    若客户端不进行服务器端是否可信、不进行域名校验、接受任意域名，对APP数据包进行拦截和篡改，则造成中间人攻击的风险。
    若客户端对服务器端返回的SSL证书进行强校验，则通过测试

4.  修复建议

    对SSL证书进行签名CA是否合法、证书是否自签名、主机域名是否匹配、证书是否过期等校验。


<a id="org5b71f45"></a>

# App加固技术

1.  第一代加固技术： 通过对源码进行
    -   压缩： 检测并一处代码中无用的类、字段、方法和特性
    -   优化： 对字节码进行优化，移除无用指令
    -   混淆： 使用a、b、c、d等无意义字符对类、字段、方法进行重命名
    -   预检： 在Java平台上对处理后的代码进行预检，确保加载的class文件时可执行

2.  第二代加固技术： 对原始App的dex文件加密，并外包一层克，将App的核心代码进行隐藏
3.  第三代加固技术： 对dex文件中所有的类及方法函数内容进行抽取、加密和隐藏，单独加密后存放在apk中的特定文件内
4.  第四代加固技术： DVMP(dex虚拟机保护) 具有自定义虚拟机、指令集和解释器，未保护的代码在系统虚拟机中运行，保护代码在自定义虚拟机运行


# Table of Contents

1.  [应用脱壳](#org775b335)
    1.  [安装Frida客户端](#org8b17b93)


<a id="org775b335"></a>

# 应用脱壳


<a id="org8b17b93"></a>

## 安装Frida客户端

1.  在Android上安装[Frida Server](https://github.com/frida/frida/releases)
    
        adb shell getprop ro.build.version.release     # 获取Android版本号
        adb shell getprop ro.product.cpu.abi           # 查看CPU架构,根据架构下载对应的frida-server-14.2.18-android-arm64.xz
        xz -d frida-server-14.2.18-android-arm64.xz    # 解压frida-server
        adb push frida-server-14.2.18-android-arm64 /data/local/tmp      # 传输到/data/local/tmp目录下
        adb shell
        su
        chmod a+x /data/local/tmp/frida-server-14.2.18-android-arm64
        adb forward tcp:27042 tcp:27042
        adb forward tcp:27043 tcp:27043
        ./frida-server-14.2.18-android-arm64
        python dexDump.py com.test.aspiredoctor

2.  在macOS上安装Frida Client
    
        pip3 install frida frida-tools







# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> 替换Android系统/system/bin/app<sub>process文件</sub>

<sup><a id="fn.2" href="#fnr.2">2</a></sup> 如apk文件和dex文件

<sup><a id="fn.3" href="#fnr.3">3</a></sup> 可导致调用系统功能截取屏幕和录像窃取用户信息，界面劫持，对用户进行钓鱼

<sup><a id="fn.4" href="#fnr.4">4</a></sup> 检测客户端程序是否对已经root的android系统、模拟器和逆向框架进行检测

<sup><a id="fn.5" href="#fnr.5">5</a></sup> 检测客户端程序是否进行代码加密、代码混淆和代码加固，是否易被逆向并泄露程序的设计原理和运行流程

<sup><a id="fn.6" href="#fnr.6">6</a></sup> 检测客户程序是否对自身进行校验

<sup><a id="fn.7" href="#fnr.7">7</a></sup> 检测客户端程序是否可被外部程序动态调试并输出敏感信息

<sup><a id="fn.8" href="#fnr.8">8</a></sup> 检测客户端程序是否存在进程保护和内存保护，防止被外部程序动态注入so文件到指定进程、以及任意修改、转储内存代码行为
