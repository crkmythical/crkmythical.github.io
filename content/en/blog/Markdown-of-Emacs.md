---
title: Markdown_of_Emacs
encrypt: false
enc_pwd: askding
categories:
  - Evolution
tags:
  - null
date: 2021-01-14 11:22:07
top:
layout:
---
编写进度
- [x] 



# 安装[markdown-mode](https://jblevins.org/projects/markdown-mode/ "官网")
## macOS

``` shell
brew install markdown pandoc multimarkdown
```



```emacs-lisp
(use-package markdown-mode
  :ensure t
  :commands (markdown-mode gfm-mode)
  :mode (("README\\.md\\'" . gfm-mode)
         ("\\.md\\'" . markdown-mode)
         ("\\.markdown\\'" . markdown-mode))
  :init (setq markdown-command
	      (concat
	       "/usr/local/bin/pandoc"
	       " --from=markdown --to=html"
	       " --standalone --mathjax --highlight-style=pygments")))
```

## 前缀键 ##
| Prefix  | Function |
|:-------:|:--------:|
| C-c C-s | 样式类   |
| C-c C-l | 链接     |
| C-c C-i | 图片     |
| C-c C-c | 命令类   |
| C-c C-x | 开关类   |


## 标记区域和移动 ##
| 操作           | keybinding | function |
|:--------------:|:----------:|:--------:|
| 标记段落       | M-h        |          |
| 下一段         | M-}        |          |
| 上一段         | M-{        |          |
| 标记块         | C-c M-h    |          |
| 下一块         | C-M-}      |          |
| 上一块         | C-M-{      |          |
| 标记section    | C-M-h      |          |
| 标记子标题区域 | C-c C-M-h  |          |
| 右移           | C-c >      |          |
| 左移           | C-c <      |          |

## 预览和导出 ##

| 操作      | keybinding | function |
|:---------:|:----------:|:--------:|
| 编译      | C-c C-c m  |          |
| 预览      | C-c C-c p  |          |
| 导出      | C-c C-c e  |          |
| 导出+预览 | C-c C-c v  |          |
| 打开      | C-c C-c o  |          |
| 实时导出  | C-c C-c l  |          |


# Markdown 语法 #

| 操作                 | keybinding  | function |
|:--------------------:|:-----------:|:--------:|
| 快捷键列表           | C-c C-h C-h |          |
| 折叠/展开            | TAB         |          |
| 移动到父级标题       | C-c C-u     |          |
| 移动到标题上一个     | C-c C-p     |          |
| 移动到标题下一个     | C-c C-n     |          |
| 移动到同级标题上一个 | C-c C-b     |          |
| 移动到同级标题下一个 | C-c C-f     |          |
	
## 标题 ##

| 操作                 | keybinding    | function |
|:--------------------:|:-------------:|:--------:|
| 自动设置标题         | C-c C-s h/H   |          |
| 插入n级标题          | C-c C-s/t \<n> |          |
| 上移标题             | C-c \<up>      |          |
| 下移标题             | C-c \<down>    |          |
| 升级标题             | C-c \<left>    |          |
| 降级标题             | C-c \<right>   |          |
| 水平分割线           | C-c C-s -     |          |


## 文本 ##
| 操作      | keybinding  | function |
|:---------:|:-----------:|:--------:|
| 斜体      | C-c C-s i/e |          |
| 粗体      | C-c C-s b   |          |
| <kbd>标签 | C-c C-s k   |          |
| 高亮      |             |          |
| 删除线    | C-c C-s s   |          |
| 引用      | C-c C-s q   |          |


## 链接 ##
| 操作          | keybinding  | function                       |
|:-------------:|:-----------:|:------------------------------:|
| URL           | C-c C-l     | markdown-insert-link           |
| Image         | C-c C-i     | markdwon-insert-image          |
| 显示/关闭图片 | C-c C-x C-i | markdown-toggle-inline-images  |
| 打开URL       | C-c C-o     | markdown-follow-thing-at-point |
| 下一个链接    | M-n         |                                |
| 上一个链接    | M-p         |                                |

## 代码 ##
| 语法          | keybinding  |
|:-------------:|:-----------:|
| 行内代码      | C-c C-s c   |
| 代码块        | C-c C-s C   |
| 编辑块        | C-c '       |
| 开/关代码颜色 | C-c C-x C-f |



## 列表 ##
| 操作   | keybinding  | function |
|:------:|:-----------:|:--------:|
| 插入   | C-c C-j     |          |
| 上移   | C-c \<up>    |          |
| 下移   | C-c \<down>  |          |
| 列右移 | C-c \<right> |          |
| 列左移 | C-c \<left>  |          |


## 复选框 ##
| 操作 | keybinding | function |
|:----:|:----------:|:--------:|
|      |  C-c C-d          |          |


## 表格 ##
| 语法     | keybindings   | function |
|:--------:|:-------------:|----------|
| 插入表格 | C-c C-s t     |          |
| 行上移   | C-c \<up>      |          |
| 行下移   | C-c \<down>    |          |
| 列左移   | C-c \<left>    |          |
| 列右移   | C-c \<right>   |          |
| 删除行   | C-c \<S-up>    |          |
| 插入行   | C-c \<S-down>  |          |
| 删除列   | C-c \<S-left>  |          |
| 插入列   | C-c \<S-right> |          |
| 行排序   | C-c C-c ^     |          |
| 表格转置 | C-c C-c t     |          |
| 区域转表 | C-c C-c       |          |


## Latex/数学公式 ##
| 操作     | keybinding  | function |
|:--------:|:-----------:|:--------:|
| 开关支持 | C-c C-x C-e |          |


## 流程图 ##
[Mermaid](https://mermaid-js.github.io "官网")
[在线编辑器](https://mermaid-js.github.io/mermaid-live-editor/)

## 其他 ##

### 脚注 ###
| 操作 | keybinding | function |
|:----:|:----------:|:--------:|
|      | C-c C-s f  |          |
|      | C-c C-a f           |          |


### 邮箱 ###
<askding@qq.com>
``` markdown
<askding@qq.com>
```

### 强制分页 ###

在 markdown 文本中需要分页的地方添入:
``` html
 <div STYLE="page-break-after: always
     ;"></div>
```

### 文本换行 ###

``` markdown
<br/>
```


参考
[Markdown-mode](https://leanpub.com/markdown-mode/read)


