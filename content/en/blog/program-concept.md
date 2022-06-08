---
title: program_concept
encrypt: false
enc_pwd: askding
categories:
  - Coding
date: 2020-12-11 16:40:10
top:
tags:
layout:
---
编写进度
- [x] 


闭包: 一个可以读取外部环境变量的函数。或者说能够读取其他函数内部变量的函数就是闭包。
    本质上，闭包就是函数内部和函数外部连接起来的一座桥梁，或者说一个通道。

因素:
  - 外层函数嵌套内层函数
  - 内层函数使用外层函数的局部变量
  - 将内层函数作为外层函数的返回
```javascript
let a=1
let b =function(){
  console.log(a)
}
```
作用：
  - 可以读取函数内部的变量
  - 让函数内部变量始终保存在内存中
