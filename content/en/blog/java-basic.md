---
title: java-basic
encrypt: false
enc_pwd: askding
categories:
  - Coding
date: 2021-03-21 09:04:43
top:
tags:
layout:
---
编写进度
- [x] 


# Table of Contents

1.  [编程基础概念](#org9682d96)
    1.  [数据类型](#orgc539f7f)
    2.  [运算符](#org29f236a)
    3.  [控制语句](#org6ba4f21)
        1.  [分支语句 if/switch](#org27dcc48)
        2.  [循环语句 while/do-while/for](#org5fe36c2)
        3.  [跳转语句 break/continue/return/thow](#orgf3c8f96)
2.  [类](#org7bafcaa)
    1.  [抽象类](#orgeb8d4f4)
    2.  [重载-Overloading](#org700c5b0)
    3.  [stu instanceof Person 是Java中的二元运算符](#orga48f328)
    4.  [反射](#org31e157a)
    5.  [线程的五个状态](#org83a6329)
3.  [JVM调优](#org66286aa)
    1.  [Debug with jdb](#orgc6fff5f)
        1.  [编译 & 运行](#org0f424fa)
        2.  [jdb命令格式](#org158f7d4)
        3.  [设置断点](#orga1151db)
        4.  [查看线程](#org5aaabdf)
        5.  [单步调试](#org519362f)
        6.  [查看变量](#org806aab2)
        7.  [其他](#orge917796)

Java 是彻底的、纯粹的面向对象语言，在Java中，一切都是对象，具有封装性、继承性、多态性。
以类的方式组织代码，以对象的形式封装数据

> 多写(代码),多写(笔记),多写(文章)
> 多练(交流),多练(思维),多练(技能)
> 多分享(知识)，多提问(怎么了)，多思考(为什么)


<a id="org9682d96"></a>

# 编程基础概念


<a id="orgc539f7f"></a>

## 数据类型

    graph LR
        dataType[数据类型]-->basic[基础数据类型]
        dataType-->pointer[引用类型]
        basic-->character[字符型]
        basic--> number[数字型]
        basic--> boolean[布尔型]
        character-->char[char]
        number-->byte/short/int/long
        number-->float/double
        boolean-->Boolean
    
        pointer-->数组/类/接口
        pointer-->enum


<a id="org29f236a"></a>

## 运算符

-   算术运算符 ： 加、减、乘、除、取余
-   关系运算符 ：不等、相等、大于、小于、大于等于、小于等于
-   逻辑操作服 ：与、或、非、异或

    graph LR
        operate[运算符]-->calc[算术运算符]
        operate[运算符]-->relation[关系运算符]
        operate[运算符]-->logic[逻辑运算符]
        operate[运算符]-->bit[位运算符]
       
        calc-->calcs[加+ 减- 乘* 除/ 取余%]
        
        relation--> relations[不等!= 相等== 大于> 小于< 大于等于>= 小于等于<=]
    
        logic-->logics[与and 或or 异或xor 非!]
    
        bit-->bits[左移<< >>右移 与& 或I 异或^ 取反]


<a id="org6ba4f21"></a>

## 控制语句

一般程序都是顺序执行代码语句的，通过控制语句可改变程序执行顺序

    graph LR
        control[控制语句]-->switch
        control-->loop[循环语句]
        control-->goto[跳转语句]
        switch[分支语句]--> if/switch
        loop-->for/while/do-while
        goto-->break/continue/return/throw/try-catch


<a id="org27dcc48"></a>

### 分支语句 if/switch


<a id="org5fe36c2"></a>

### 循环语句 while/do-while/for


<a id="orgf3c8f96"></a>

### 跳转语句 break/continue/return/thow


<a id="org7bafcaa"></a>

# 类

类加载顺序: 静态代码块-> 匿名代码块-> 构造函数
类加载过程:
1. 类初始化代码：
	a. 基类初始化代码
	b. 子类初始化代码

2. 基类实例初始化代码、基类构造方法
3. 子类初始化代码、子类构造方法




    package com.example.className;
    
    import package1[.package2 ...].(className|*);
    import package1[.package2 ...].(className|*);
    
    
    [public] [abstract|final] class className [extends superclass] [implements interfaceNameList]{
        //
        [public|protected|private] [static] [final] <dataType> variableName;
    
    
        [public|protected|private] [static] [final|abstract] [native] [synchronized] <dataType> methodName([paramList]) [throws exceptionList]{
    
        }
    }


<a id="orgeb8d4f4"></a>

## 抽象类

-   抽象类可以包含普通方法
-   抽象方法必须在抽象类中


<a id="org700c5b0"></a>

## 重载-Overloading

在一个类中，两个方法名相同，参数列表<sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup> 不同 , 与返回类型无关。


<a id="orga48f328"></a>

## stu instanceof Person 是Java中的二元运算符

左边stu是对象，右边Person是类；
当对象是右边类或子类所创建对象时，返回true；否则，返回false。


<a id="org31e157a"></a>

## 反射


<a id="org83a6329"></a>

## 线程的五个状态

![img](https://raw.githubusercontent.com/crkmythical/PicGo/main/images/20210711180509thread_status.png)


<a id="org66286aa"></a>

# JVM调优

调优是针对方法区和堆区进行调优

双亲委派机制：当某个类加载器加载一个类时，它首先委托它的上级类加载器去夹在，并递归这个操作，
       如果上级的类加载器没有家在，自己才会加载这个类。
       作用： 防止重复加在某个类，保证核心类不被篡改


<a id="orgc6fff5f"></a>

## [Debug with jdb](https://www.shuzhiduo.com/A/A2dme1xxde/)


<a id="org0f424fa"></a>

### 编译 & 运行

    javac -g path/to/src/*.java  -d path/to/bin/                 # 编译
    jar cvfm  path/to/xxx.jar  manifest.mf path/to/bin/*.class   # 打包
    
    # 本地调试
    java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,address=8000 <MainClassName>  # 运行开启调试 or -jar xx.jar , 顺序： -Xdebug ... -jar ..
    jdb -connect com.sun.jdi.SocketAttach:hostname=localhost,port=8000                # jdb调试
    
    # 远程调试
    java -Xdebug -Xrunjdwp:transport=dt_socket,address=192.168.10.205:8000,suspend=y -jar remoting-debug.jar
    jdb -connect com.sun.jdi.SocketAttach:hostname=192.168.10.205,port=8000


<a id="org158f7d4"></a>

### jdb命令格式

`jdb <options>  <className> <arguments>`

    java -Xdebug -Xrunjdwp:transport=dt_socket,address=8888,server=n,suspend=y com.lhx.cloud.javathread.MarkWord.JdbMain
    
    -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,address=3999,suspend=n
    -XDebug               启用调试。
    -Xnoagent             禁用默认sun.tools.debug调试器。
    -Djava.compiler=NONE  禁止 JIT 编译器的加载。
    -Xrunjdwp             加载JDWP的JPDA参考执行实例。
    transport             用于在调试程序和 VM 使用的进程之间通讯。
    dt_socket             套接字传输。
    dt_shmem              共享内存传输，仅限于 Windows。
    server=y/n            =y表示当前是调试服务端，=n表示当前是调试客户端
    address=8000          调试服务器的端口号，客户端用来连接服务器的端口号。
    suspend=y/n           =n表示启动时不中断（如果启动时中断，一般用于调试启动不了的问题）
    
    
    jdb -attach <ip>:8888           #连接到JVM，本机IP即可省略

    where options include:
        -? -h --help -help print this help message and exit
        -sourcepath <directories separated by ":">
                          directories in which to look for source files
        -attach <address>
                          attach to a running VM at the specified address using standard connector
        -listen <address>
                          wait for a running VM to connect at the specified address using standard connector
        -listenany
                          wait for a running VM to connect at any available address using standard connector
        -launch
                          launch VM immediately instead of waiting for 'run' command
        -listconnectors   list the connectors available in this VM
        -connect <connector-name>:<name1>=<value1>,...
                          connect to target VM using named connector with listed argument values
        -dbgtrace [flags] print info for debugging jdb
        -tclient          run the application in the HotSpot(TM) Client Compiler
        -tserver          run the application in the HotSpot(TM) Server Compiler
    
    options forwarded to debuggee process:
        -v -verbose[:class|gc|jni]
                          turn on verbose mode
        -D<name>=<value>  set a system property
        -classpath <directories separated by ":">
                          list directories in which to look for classes
        -X<option>        non-standard target VM option


<a id="orga1151db"></a>

### 设置断点

    stop \ clear                        # 查看断点
    stop at <className>:<Line No>       # 在特定行号处设置断点
    stop in <className>.<method\field>  # 在特定方法\变量处设置断点


<a id="org5aaabdf"></a>

### 查看线程

    threads           # 查看所有线程
    thread <id>       # 查看单个线程
    where             # 查看线程堆栈
    pop               # 当前帧出栈, 且打印当前帧


<a id="org519362f"></a>

### 单步调试

    step              # 执行当前行(进入函数体)  = step into
    step up           # 跳出当前函数,回到当前函数调用处 = step return
    stepi             # 执行当前指令
    next              # 执行当前行(跳过函数调用) = step over
    cont              # 运行到下一断点处 = resume


<a id="org806aab2"></a>

### 查看变量

    list [line|method]               # 查看代码
    locals                           # 查看当前栈所有变量
    set <var>=<expr>                 # 设置变量
    eval/print <expr>                # 显示java基础类型的值
    dump <expr>                      # 输出java引用类型信息


<a id="orge917796"></a>

### 其他

    monitor <command>	         # 当程序暂停时自动执行命令
    monitor	                 # 列出所有的monitor
    watch <var>	             # 运行到变量的值改变时停止
    unwatch <var>	             # 取消watch
    classes                      # 列出所有已知的类


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> 个数，类型，排列顺序






