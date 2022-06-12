---
title: Android软件安全测试软件_mac平台
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2022-03-07 19:10:29
top:
tags:
layout:
---
编写进度
- [x] 

# 搭建Android程序分析环境

## 常见逆向分析工具

- Adnroid Studio

```shell
brew install --cask android-studio      # 安装Adnroid Studio
```

- apktool

```shell
brew install apktool    # 提供反编译与回编译功能
```

- smali/baksmail

```shell
brew install smali     # dex文件的反编译与回编译工具
```

- dex2jar & jd-gui

```shell
brew install dex2jar         # 将dex文件转成jar包
brew install --cask jd-gui   # 查看jar包源码
```

- jadx

```shell
brew install jadx          # 将.dex/.jar/.class反编译成.java
```

- 010 Editor

```shell
brew install --cask 010-editor    # 二进制编辑器
```

- JEB

- [Androguard](https://github.com/androguard/androguard)

- 集成工具 [Android-Crack-Tool](https://github.com/Jermic/Android-Crack-Tool)

- [Android-Killer](https://github.com/liaojack8/AndroidKiller)

- [APKiD](https://github.com/rednaga/APKiD)

  

## 编译Android源码

下载源码

```shell
repo init -u https://android.googlesource.com/platform/manifest -b android-7.1.1_r1
repo sync --force-sync  --force-broken
```



### macOS下直接编译

需安装XCode命令行工具和macOS SDK

```shell
xcode-select --install
```

编译Android源码

```shell
export USE_CCACHE=1
mkdir ccache
export CCACHE_DIR=ccache
prebuilts //misc/darwin-x86/ccache/ccache -M 50G
sudo xcode-select -s /Applications/Xcode.app/Content/Developer
export JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
source build/envsetup.sh
lunch aosp_angler-userdebug
make clobber
brew uninstall curl && brew install curl --with-openssl
export PATH=$(brew --prefix curl)/bin:$PATH
caffeinate make -j8
```



### 在Docker中编译Android源码-推荐

```shell
brew install --cask docker docker-toolbox
```

打开*Kitematic.app* 搜索 `aosp` 选择4.4版，点击`create` 

下载后

修改android源码和ccache缓存位置,

重启

执行 `make -j4`

