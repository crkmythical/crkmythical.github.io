---
title: awk-sed-grep-find
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-01-10 10:20:21
top:
tags:
layout:
---
编写进度
- [x] 

- 记住三个命令的运用形式
     grep    '字符'       文件     擅长查找文件内容
     sed     '命令'       文件     擅长取行和替换
     awk    '条件{命令}'   文件     擅长取列,统计
- 单引号内就是正则表达式的用法


# awk

处理过程：
1. 将文件中的第一行作为输入，然后将此行放入模式空间，并将此行(以换行符结束)赋给内部变量$0
2. 字段分解(默认IFS为空格,可通过-F来修改)，切割整行内容，并将切割后的字段依次赋给$1,$2..$n ,NF=n, NR+=1（NR=0）
3. 检查print函数需要打印的字段，从模式空间中取出字段对应的值，输出结果到屏幕以空格为分割(可通过内部变量OFS进行调整)
4. 继续将第二行载入模式空间，并将其值存储在$0中，覆盖原来的内容
5. 重复1-4


``` shell
awk  [options] 'commands' filename
awk [options] -f awk-script-file filename

<commands>部分：
预处理       行处理       处理后
BEGIN{ }      {}        END{}
```

常见用法： 匹配+处理
``` shell
awk  -F "[ :\t]+"  'BEGIN{ OFS="--" }   /<pattern>/{ if (<condition>) {<if_true_statment>} else{<if_false_statment>} }   END{}'    /path/to/file
```
- `-F "[ :\t]+" `指定多个IFS,可以多个冒号和空格和tab键
- `NF` 每行最后一列的$<n>，将<n>复制给NF, `$NF` 打印最后一列的值 , `FNR`用于多文件处理
- `NR` 记录行号,从1开始
- `OFS` 逗号,映射为OFS，初始情况下OFS变量是空格

## awk分支语句结构
``` shell
awk '/<pattern>/ { if( <condition> ) { if_true_statement } else if ( <condition> ) { elif_true_statement } else { elif_false_statement } }' /path/to/file
```

## awk循环语句结构
- while

``` shell
awk -F : '{ i=1; while ( <condition> ) { while_true_statmen }   }' /path/to/file
```

- for

``` shell
awk -F : ' { for ( : : ) {for_true_statement} } ' /path/to/file
```





## 打印指定行之间的文本
类似于`cat -n`

```zsh
awk 'NR==M,NR==N {printf $1}' filename
cat file | awk 'NR==M,NR==N'   #打印M行到N行之间的文本
```

## 打印模式之间行的文本
```zsh
awk '/start_pattern/,/end_pattern/'  filename 
```
## 匹配指定列的内容

``` shell
awk -F : '$3 ~ /alice/' /etc/passwd
awk -F : '$3 !~ /alice/' /etc/passwd
```
 


# sed #
sed命令若无语法错误，返回值永远是0
  
``` shell
sed [option] '<command>' /path/to/files
sed [option] 's/<str_old>/<str_new>/<g/i>'  /path/to/file
```
- Options
  - `-e` 允许多个'<command>'
  - `-n` 只输出匹配的行
  - `-i` 直接修改文件
  - `-r` 支持扩展元字符


- <command>参数 '<num>/<pattern>/{<a;i;c;d>} [String]'
  - `a` 追加后面    在当前行后添加一行/多行
  - `i` 追加前面    在当前行之前插入文本
  - `c` 行替换      在当前行进行替换/修改 
  - `d` 删除       在当前行进行删除操作
  - `sg` 全局替换
  - `p` 打印匹配/指定的行

## 匹配ip地址
``` shell
ifconfig eth0 | sed -rn '2s#(^.*inet) (.*) (net.*$)#\2#gp'
ifconfig eth0 | sed -rn '2s/(^.*inet) (.*) ( net.*$)/\2/gp'

ifconfig en0 | sed -rn '5s/(.*inet )(.*)( net.*)/\2/gp'  # mac本地ip
ifconfig utun2  | sed  -rn '2s/(^.*inet )(.*)( -->.*$)/\2/gp'
```



# grep/egrep #

## 匹配ip地址 ##

``` shell
ifconfig eth0 | grep netmask | egrep "([0-9]|\.){1,4}"
```
