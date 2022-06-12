---
title: emacs
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2021-01-12 09:46:03
top:
tags:
layout:
---
编写进度
- [x] 

EMACS(Editor MACroS)


在 Emacs 中, 我们有一条规定:

 | key binding     | Description                           |
   |------------|--------------------------------|
   | `C-<字符>` | 在输入 `<字符>` 时按住 Control |
   | `M-<字符>` | 在输入 `<字符>` 时按住 Alt     |
   | `s-<字符>` | 在输入 `<字符>` 时按住 Super   |

# Emacs帮助信息

 | key binding     | Description                           |
   |------------|--------------------------------|
   | `C-h t` | 帮助-教程tutorial |
   | `C-h k` | 帮助-快捷键     |
   | `C-h f` | 帮助-函数介绍functions |
   | `C-h v` | 帮助-变量介绍varialbes |


# 窗口操作

| key binding     | Description                           |
   |------------|--------------------------------|
   | `C-x 0` | 关闭当前窗口       |
   | `C-x 1` | 只保留当前窗口     |
   | `C-x 2` | 水平分割窗口      |
   | `C-x 3` | 垂直分割窗口      |
   | `C-x o` | 切换窗口          |


# 缓冲区buffer操作

| key binding     | Description                           |
   |------------|--------------------------------|
   | `C-x C-b` | 打开buffer列表       |
   | `C-x b`   | 切换buffer       |
   | `C-x <L/R>` | 切换buffer       |
   | `C-x k` | 关闭buffer       |


# 文件操作
## 打开文件

|key binding| Description |
|------------|-------------|
|`C-x d` | 打开目录 |
|`C-x C-f` | 查找/创建文件|

## 搜索
|key binding| Description |
|------------|-------------|
|`C-s` | 向下搜索 |
|`C-r` | 向上搜索 |

## 编辑文件/buffer
### 移动

`C-u <num> <key>`  执行`<num>`次`<key>`

```zsh
    |M-<(开头)					 :
C-l |						 :
居顶 |					       M-v(翻屏)
    |C-a(行首)                               :			C-e(行尾)
    |M-a(句首)			      		 :            		M-e(句尾)
    |				  		 :
    |				  	       C-p(上行)
    |				  	        :
C-l |		     (左移字符/单词)C-b/M-b ...|_(光标) ....  C-f/M-f(右移字符/单词）    
居中|                               	   	 :
    |				  	       C-n(下行)
    |				  	        :
    |				   	        :
    |				  		 :
    |				  		 :
    |				 	       C-v(翻屏)
    |                              	        :
C-l |						 :
居底|                                        :			 M->(结尾)
```

### 删除
```zsh
    |                                           :
    |M-k(删除光标至行首)	             ...|_(光标)...               C-k(光标至行尾、重复则删除换行符)
    |                                           :
    |    (删除字符)<DEL>/(移除单词)M-<DEL> .... |_(光标) ....  C-d(删除字符/M-d(移除单词）
    |                                           :
```




### 选择

|key binding| Description |
|----------|-------------|
|`C-@` | 开启标记|


### 复制/剪贴
|key binding| Description |
|----------|-------------|
|`M-w`|复制|
|`C-w`|剪贴|

### 粘贴
|key binding| Description |
|----------|-------------|
|`C-y` |粘贴|


### 撤销/重做
|key binding| Description |
|----------|-------------|
|`C-x u`|仅撤销|
|`C-/`|撤销/重做|
|`C-_`|撤销/重做|

## 保存
|key binding| Description |
|----------|-------------|
|`C-x C-s`|保存当前buffer到对应文件|
|`C-x s`|保存所有buffer到对应文件|







