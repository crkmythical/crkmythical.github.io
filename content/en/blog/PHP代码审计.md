---
title: PHP代码审计
encrypt: false
enc_pwd: askding
categories:
  - Coding
date: 2022-04-09 21:05:43
top:
tags:
layout:
---
编写进度
- [x] 



# Table of Contents

1.  [PHP Audit](#org0fff461)
    1.  [代码审计常见思路](#org8eab023)
    2.  [Untrusted Data ,使用不可信的数据](#orgd90071d)
    3.  [Command Execution, 命令执行](#org593992a)
    4.  [Code Execution, 代码执行](#orgaf7a00a)
    5.  [Information Discloure, 信息泄露](#org00773a4)
    6.  [Insecure Cryptographic Functions, 不安全的加密函数](#org9c1cc21)
2.  [常见危险函数](#org7fd6349)
    1.  [代码执行函数](#orgd42787f)
    2.  [包含函数](#orga4dc3ae)
    3.  [命令执行函数](#orgb5e23c0)
    4.  [文件操作函数](#org7b93cfd)
    5.  [特殊函数](#org8ef722f)
    6.  [变量覆盖](#orgabca0d4)


<a id="org0fff461"></a>

# PHP Audit

[RIPS](https://github.com/ripsscanner/rips)  [VCG](https://github.com/nccgroup/VCG)

代码审计本质： `unfilter_function(param_input)`
找漏洞 -> 找可以控制的变量(参数)=param<sub>input</sub>= -> 危险函数(param<sub>input</sub>)

漏洞形成条件：

-   可以控制的变量(外部输入)       &#x2013;>  内因
-   危险(未过滤参数)函数接收外部输入 &#x2013;> 决定漏洞的类型

隐式输入： 用户传递数据-> 数据库/缓存文件等地方 -> 程序代码处理->程序代码
显式输入： 用户传递数据-> 程序代码处理


<a id="org8eab023"></a>

## 代码审计常见思路

-   正向追踪： 变量  &#x2013;> 函数
-   逆向追踪： 函数  &#x2013;> 变量
-   常见功能点定向审计
-   第三方组件,中间件版本比对
-   补丁比对，反推漏洞位置
-   工具扫描+人工验证


<a id="orgd90071d"></a>

## Untrusted Data ,使用不可信的数据

`$_REQUEST` 参数中的数据是浏览器可控的，黑客可以通过巧妙的构造覆盖PHP全局参数


<a id="org593992a"></a>

## Command Execution, 命令执行

PHP中存在敏感函数可以执行系统命令，常见这类函数如下：

    exec
    shell_exec
    
    system
    passthru
    
    popen
    proc_open
    pcntl_exec


<a id="orgaf7a00a"></a>

## Code Execution, 代码执行

PHP中存在敏感函数可以执行PHP代码段，常见函数如下：

    eval
    assert
    preg_replace
    create_function


<a id="org00773a4"></a>

## Information Discloure, 信息泄露

常见信息泄露的函数有 `phpinfo~、~show_source`


<a id="org9c1cc21"></a>

## Insecure Cryptographic Functions, 不安全的加密函数

-   md5
-   CRYPT<sub>STD</sub><sub>DES</sub>
-   CRYPT<sub>EXT</sub><sub>DES</sub>
-   CRYTPT<sub>MD5等</sub>


<a id="org7fd6349"></a>

# 常见危险函数


<a id="orgd42787f"></a>

## 代码执行函数

-   `eval()`
-   `assert()`
-   `preg_replace`
-   `create_funtion`
-   `call_user_func`
-   `call_user_func_array`
    
        @eval('echo "test-echo";');
        echo '<hr>';
        assert('system("whoami")');
        // preg_replace("/test/e","phpinfo();","just test");
        
        echo '<hr>';
        $cfunc=create_function('$v','return system($v);');
        $cfunc('whoami');
        
        echo '<hr>';
        $sfunc='sys'.'tem';
        $sfunc('whoami');
        
        echo '<hr>';
        function callback($var){
                 echo "call test $var";
        }
        call_user_func('callback','crkmyth1cal');


<a id="orga4dc3ae"></a>

## 包含函数

-   `require`
-   `include`
-   `require_once`
-   `include_once`

    include $file;
    
    include($_GET['file']);
    //     ?file=php://filter/convert.base64-encode/resource=index.php


<a id="orgb5e23c0"></a>

## 命令执行函数

-   `exec`
-   `system`
-   `popen`
-   `proc_open`
-   `passthru`
-   `shell_exec`

    echo shell_exec('ping www.baidu.com');
    echo shell_exec('whoami');


<a id="org7b93cfd"></a>

## 文件操作函数

读取：读取配置文件，获取key
写入：写入shell代码
删除：删除 `.lock` 文件，重新安装

-   `copy`
-   `file_get_contents`
-   `file_put_contents`
-   `file`
-   `glob`
-   `fopen`
-   `move_uploaded_file`
-   `readfile`
-   `rename`
-   `delete`
-   `rmdir`
-   `unlink`
-   `symlink`
-   `readlink`
    
        file_put_contents("baidu.txt",file_get_contents("https://www.baidu.com"));
        unlink('baidu.txt');


<a id="org8ef722f"></a>

## 特殊函数

-   `phpinfo`
-   `getenv`
-   `putenv`
-   `dl`
-   `ini_get`
-   `ini_set`
-   `ini_alter`
-   `ini_restore`
-   `is_numeric`
-   `in_array`


<a id="orgabca0d4"></a>

## 变量覆盖

-   `parse_str`
-   `mb_parse_str`
-   `extract`
-   `import_request_variables`
-   `get_defined_vars`
-   `get_defined_constants`
-   `get_defined_functions`
-   `get_included_files`


