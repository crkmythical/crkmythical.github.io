---
title: IDEA使用教程
encrypt: false
enc_pwd: askding
categories:
  - Coding
date: 2022-03-17 19:16:17
top:
tags:
layout:
---
编写进度
- [x] 

# IntelJ Idea
教程 https://github.com/judasn/IntelliJ-IDEA-Tutorial

```bash
brew install --cask intellij-idea
```
## [激活](https://www.digit77.com/skills/jetbrains-family-of-products-reset-trial/)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227132127.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227132201.png)


## Settings Import/Export
- 导出配置 ![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227150440.png)


# IDEA [偏好设置](https://mp.weixin.qq.com/s/qEnw1ZoprGmE1iP6zvv85w)
全局配置打开偏好设置
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227142833.png)



## [Appearance & Behavior](https://www.cxymm.net/article/qq_37242720/119349394)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227112327.png)

###  System Settings

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227181318.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227111947.png)

## Keymap
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227181652.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227144310.png)

### [常用快捷键](https://mp.weixin.qq.com/s/qFuAQH8UBxQPVOTV8atICQ)
| Ctrl+h               | 查看当前类的层次结构      |                      |
|----------------------|-----------------|----------------------|
| Command+7            | 查看类结构           |                      |
| Command+O            | 快速搜索类           |                      |
| Command +F           | 关键字检索(当前文件)     | 全局 Command+Shift+F   |
| Command+Alt +B       | 查看方法/类的实现类      |                      |
| Alt + F7             | 查看调用链(方法被调用的情况) | Find Usages          |
| Command+E            | 查看最近使用的文件       |                      |
| Shift+ Alt+Command+U | 查看可视化类继承链       |                      |
| Command+/            | 行注释             | Shift+Command+/  块注释 |
| /**                  | 方法/类注释          | 可自定义类模板              |

可视化类继承链
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227160151.png)










## Editor
### Editor->General
1. 自动导入
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227120933.png)

2. Appearnace

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227121257.png)

3. Code Completion

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227121555.png)


4. Editor Tabs

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227121841.png)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220206192547.png)



#### [Postfix Completion](https://mp.weixin.qq.com/s?__biz=MzUyMzM2ODUwMA==&mid=2247492923&idx=1&sn=e52afa95777cbccbb584aad842d1a22c&chksm=fa3f0460cd488d76420858c663953776765cab8ce04561a825494e813096703d40f89dbbc5db&scene=178&cur_album_id=1909669645461061636#rd)
##### var声明
![](https://mmbiz.qpic.cn/mmbiz_gif/6mychickmupWyfMiax0ryO6Hs4SYOv1IzQdjcSJMicwQYJQs8bTHEC1zOCd6Jkr36fxNyC78jlvp9Odf2e4vsz1yw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
##### null判空
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZMcKIXiaSiajAJWIYibk9ibXe7cH2NkzceukMqjWcCENZ7jceHU5Wjhqaw5g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
##### notnull判非空
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZM6l4tCztov1I8aBj4Itpjgibp0MiaLae7YZvGpQ2cYbvMCOsFGQvrYj0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



##### nn判非空
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZMoNOv2U8f55H037mj52harqvkjhZKC8890vUV0NPqvlFRhgfH8Gs8jw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
##### for遍历
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZMuSA6LYat1ic9Q0FbgMJnPSc6OuGIuPyiazb211nWHpNCehQRBVsyIzqQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
##### fori带索引的遍历
![](https://mmbiz.qpic.cn/mmbiz_gif/6mychickmupWyfMiax0ryO6Hs4SYOv1IzQdd7icPx2AmhkjzppPdpYNe3bZ5KicaGuWd8kE9vKmP7oSb6Jicj25RY5w/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
##### not取反
![](https://mmbiz.qpic.cn/mmbiz_gif/6mychickmupWyfMiax0ryO6Hs4SYOv1IzQd6ZV6iaCS8dGeLt467yT4bws21Z4vVYjnz95wk4f35eQVVuwF30SZ6Q/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

##### if条件判断
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZMnnNuhiaco3oVEWxanVIUo5T1UUw8StFtIFCDVIg8b5x7yiaTXl4O17Uw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
##### cast强转
![](https://mmbiz.qpic.cn/sz_mmbiz_png/knmrNHnmCLGibPt6pn8saKdYrj2oy8FZMQ0U1hCpF8Vwjn293YoODjs1fkbGtZ23VTKSY8yzpTKp99M5uW2MDIA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)
##### return返回值
![](https://mmbiz.qpic.cn/mmbiz_gif/6mychickmupWyfMiax0ryO6Hs4SYOv1IzQ0y2u4vU6p2gBuFQSfxYuqlaB7HJGnlBEUXW0QfLKKSl5FTzvXAdQHg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)





### Editro->Code Editing
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227122143.png)

### Font
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227122258.png)

### Color Scheme

### Code Style
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227180859.png)

### Inspections
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227122840.png)




### File and Code Templates
- File Header

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227124637.png)

- Class
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220107203714.png)

- Interface


- Enum





### File Encodings
BOM（byte-order mark），即字节顺序标记，它是插入到以UTF-8、UTF16或UTF-32编码Unicode文件开s头的特殊标记，用来识别Unicode文件的编码类型。对于UTF-8来说，BOM并不是必须的，因为BOM用来标记多字节编码文件的编码类型和字节顺序（big-endian或little-endian）。

　　不含BOM的UTF-8才是标准形式，UTF-8不需要BOM
　　带BOM的UTF-8文件的开头会有U+FEFF，所以我新建的空文件会有3字节的大小。

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211224114425.png)
### Live Templates
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220107204808.png "")

###  File Types
隐藏项目中 `.idea`目录
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220206190631.png)



### Inlay Hints -Java
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227125319.png)

### Copyright
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220103191410.png)


## [Plugins](https://mp.weixin.qq.com/s/pM87uedBdcwd2L9D9gGjBA)
### 常用插件
#### [Theme ](https://mp.weixin.qq.com/s?__biz=MzUyMzM2ODUwMA==&mid=2247492901&idx=2&sn=e9c77d0fe12e88fc8fdf054b34732366&chksm=fa3f047ecd488d683dcc33df9a81c73dd6e5a336b8b4d1c4908090c4938433e867bc17bd3e1c&scene=178&cur_album_id=1909669645461061636#rd)- Nord / One Dark / Xcode-Dark
#### Dash
#### Edutool
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220104215621.png)

#### Stackoverflow
####  时序图-SequenceDigram
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227161338.png)

#### 项目代码统计 Statistic
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227161734.png)

#### 快捷键展示Presentation Assistant

![](https://mmbiz.qpic.cn/mmbiz_gif/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakBgsgLZPl3maGdf8a62zoYQ3vTZMvu2Py1avbCGCuwibeJ7PBrBn8iccA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)
![](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakiaPz4h5OwibzR6XIO4n2cQme76lXibZEpjibnmegVSFNU77ibsEKtic2VZCg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1 "")

#### 阿里巴巴 Java 代码规范- Alibaba Java Code Guidelines

![](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakiaPz4h5OwibzR6XIO4n2cQme76lXibZEpjibnmegVSFNU77ibsEKtic2VZCg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### MybatisX 高效操作Mybatis插件
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220105150901.png)


#### Mybatis-log-plugin
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220207120910.png)


#### Codota— 代码智能提示
![](https://mmbiz.qpic.cn/mmbiz_gif/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakSXaQcGmbeX7lEiagsB77fuAgwGRevqNpsdE7xiaGzQILQOYekOc4oCGQ/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

#### 必备的翻译插件-Translation
![](https://mmbiz.qpic.cn/mmbiz_gif/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakmYCroPAnOiaaIPozjPBO4pIPqxibLT5cdYQMS6CFibV0t274w68KpgGEg/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

#### GitToolBox-显示代码提交时间
![](https://mmbiz.qpic.cn/mmbiz_png/YbBNmAxYwZKtUbMyD6jfjjTguoBwwHPzKDUaW4Dv5PxwDiayEy9cGVFQaiabiaQEchozF7L7PoiaoeFvboTjv7aHaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### Key Promoter X 快捷键提示插件
![](https://mmbiz.qpic.cn/mmbiz_png/WpLd6CGLSOcjAegECzd4bQdYmUR3YD9rT9Eeerbj7BJ5V6nOXQelRicRB1lLutibuQn7LDVezhnjLWDlNsmro1gA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### Rainbow Brackets ——让你的括号变成不一样的颜色，防止错乱括号
![](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucDB7I8nDz0WVxAlXgVuniak1YXXhjwEpUdwRMk7TQ74E5RKHoRQLIHktdK6RH7wngAB2P2ia2ia9ODA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### Leetcode Editor 可以在IDEA中在线刷题。

![](https://mmbiz.qpic.cn/mmbiz_gif/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakhfa6UVfAab15vkX7VT51CkDO6m0a9EcaqhpnR9g7RLJpsPJUAaqyjw/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)

#### Java Stream Debugger —— Stream 将操作步骤可视化
![](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucDB7I8nDz0WVxAlXgVuniaksC3KO7wkbkmEKDW6FMdhBhaCGyb9bSgQkFNjZRCB0bU8S80uPIxvkw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

##### [Insomnia](https://plugins.jetbrains.com/plugin/17755-insomniac)-防止休眠
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220101222852.png)




### 安全插件
#### [SpotBugs](https://plugins.jetbrains.com/plugin/14014-spotbugs)
https://github.com/find-sec-bugs/find-sec-bugs/wiki/IntelliJ-Tutorial
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220104170209.png)
#### [Black Duck SCA](https://plugins.jetbrains.com/plugin/11516-synopsys-code-sight)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211230210819.png)

#### [WhiteSource Advise](https://plugins.jetbrains.com/plugin/13805-whitesource-advise-for-intellij-idea)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211230222452.png)

#### [Momo Code Sec Inspector](https://github.com/momosecurity/momo-code-sec-inspector-java)
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211230173423.png)
#### Java Decompiler(JD-GUI)
#### Jadx Android Decompiler
#### [Snyk](https://plugins.jetbrains.com/plugin/index?xmlId=io.snyk.snyk-intellij-plugin)
##### CMD模式
```powershell
brew tap snyk/tap  && brew install snyk 
snyk config set api  c70b7d5a-1a74-44df-b52c-3ab5c80d12e6
```
##### IDE模式
先配置`JAVA_HOME`
```powershell
echo '
## JDK
export JAVA_HOME=/Library/Java/JavaVirtualMachines/zulu-8.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH

'  >> ~/.zshrc
```
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211230173024.png)



#### jclasslib bytecode viewer 查看字节码
#### Jadx Android Decompiler
#### RIPS Security Analysis
#### RestfulTool—— 快捷跳转Action方法

![](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbucDB7I8nDz0WVxAlXgVuniakHgF8atqV8PGEuH9JDkHKGKNpfCial2hOF8ONySpxNRBsE9zdAzNX9Lg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

#### 

#### snyk

#### SonarLint 代码质量检查插件
![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/knmrNHnmCLF51wyFzvHFUlCcp8Nms74EYzic9yxWfOEXrmX4zR8SUnyaicfQkgCbxwNl13RcUIYvq62cRPXfJ8Lg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

















## Version Control
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227173633.png)


###  与Gitlab集成
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227135846.png)



## Build, Excution,Deployment

### Maven

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227144953.png)

### Compiler
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220104233533.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227143912.png)

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227113848.png)



### [Debug](https://mp.weixin.qq.com/s?__biz=MzUyMzM2ODUwMA==&mid=2247493487&idx=2&sn=2a117b44b5dd666ddd79ebf90940831a&chksm=fa3f0634cd488f22657c709768ceb59cab1c4dda36185984e6d0dd5c5fd191e80a0b6b21c2c2&scene=178&cur_album_id=1909669645461061636#rd)

当调用`ConcurrentLinkedQueue`类的`toString()`方法时会获取队列的迭代器，而创建迭代器时会调用队列的`first()`方法，在`first()`方法里会修改head的属性，从而导致输出的结果不一致
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220206221748.png)


在远程调试时，我们发现有些类的断点一直断不上问题，该问题可能出在 IntelliJ 的 `Settings -> Debugger -> Stepping` 配置上。若勾选了 `Do not step into the classes`，则会让这些断点失效:

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227143422.png)

#### 本地调试
![local-debug](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211124144856.png)


#### 远程调试
![remote-debug](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211124151022.png)

#### 对第三方jar包进行调试
1. 创建工程，将jar包添加到依赖库中
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227204419.png)

2. 在Main函数处打断点，添加调试配置，运行程序，点击debug
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227204510.png)



## Language & Frameworks

## Tools

### Setting Repository- 使用git仓库保存Idea配置文件Settings
Access-Token `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`

![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20220109130029.png)



## Advanced Settings

## Other Settings
代码对比效果图
![](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20211227130636.png)








