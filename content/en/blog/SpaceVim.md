---
title: SpaceVim
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2020-12-12 12:20:18
top:
tags:
layout:
---
编写进度
- [x] 

[Vim Cheat Sheet
](https://vim.rtorr.com/lang/zh_tw)## SpaceVim

[官方文档](https://spacevim.org/cn/documentation/)实在太难阅读了

帮助信息

`SPC h k` 显示各个导航快捷键的绑定信息

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC h k` | 使用快捷键导航，展示 SpaceVim 所支持的前缀键 |
| `SPC h m` | 使用 Unite 浏览所有 man 文档 |

报告一个问题：

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC h I` | 根据模板展示 Issue 所必须的信息 |

### 导航键

#### Spacevim中的前缀键：

| 前缀名称 | 用户选项以及默认值 | 功能描述 |
| --- | --- | --- |
| `[SPC]` | 空格键 | SpaceVim 默认前缀键 |
| `[Window]` | `s` | SpaceVim 默认窗口前缀键 |
| `<leader>` | `\` | Vim/Neovim 默认前缀键 |
| `g` | `g` | unkown |
| `z` | `z` | unkown |

#### SPC前缀键大体命令分类

| SPC主体划分 | 相关功能 |
| --- | --- |
| `SPC b` | 缓冲区相关命令 |
| `SPC c` | 注释相关命令 |
| `SPC e` | 错误相关命令 |
| `SPC f` | 文件操作相关命令 |
| `SPC h` | 帮助相关命令 |
| `SPC i` | 插入相关命令 |
| `SPC j`|跳转、合并、拆分相关命令| 
| `SPC l` | 编程相关命令 |
| `SPC q` | 退出vim相关命令 |
| `SPC s` | 搜索相关命令 |
| `SPC w` | 窗口相关命令 |
| `SPC x` | 文本相关命令 |

导航窗口内，按下 `Ctrl-h` 键,可显示帮助信息 帮助信息快捷键：

| 按键  | 功能描述 |
| --- | --- |
| `u` | 撤销按键 |
| `n` | 向下翻页 |
| `p` | 向上翻页 |

如果要自定义以 `[SPC]` 为前缀的快捷键，可以使用 `SpaceVim#custom#SPC()`，示例如下：
启动函数文件应放置在 Vim &runtimepath 的 autoload 文件夹内。例如：
文件名：~/.SpaceVim.d/autoload/myspacevim.vim
```vimscript
call SpaceVim#custom#SPC('nnoremap', ['f', 't'], 'echom "hello world"', 'test custom SPC', 1)
```

### 自定义配置

SpaceVim 相关的快捷键均以 `SPC f v` 为前缀，这便于快速访问 SpaceVim 的配置文件：

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC f v v` | 复制并显示当前 SpaceVim 的版本 |
| `SPC f v d` | 打开 SpaceVim 的用户配置文件 |
| `SPC h l` | 列出所有可用模块 |

[全部模块](https://spacevim.org/cn/layers/)

```toml
[options]
 automatic_update = true # SpaceVim自动更新
 
 
  disabled_plugins = ["clighter", "clighter8"]  # 禁用插件
 
[[custom_plugins]]    # 自定义插件
    repo = "lilydjwg/colorizer"
    on_cmd = ["ColorHighlight", "ColorToggle"]   # 可以通过 :h dein-options 查阅。
    merged = false
 
```

### 界面美化

所有的界面元素切换快捷键都以 `[SPC] t` 或 `[SPC] T` 开头

```toml
[options]
colorscheme = "gruvbox"
colorscheme_bg = "dark"
filemanager = "nerdtree"
enable_googlesuggest = true  # 启用google搜索
enable_guicolors = false   # 启用真色
guifont = "SourceCodePro Nerd Font Mono:h11"  # 字体配置

```

#### 界面元素切换

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC t 8` | 高亮所有超过 80 列的字符 |
| `SPC t f` | 高亮临界列，默认 `max_column` 是第 120 列 |
| `SPC t h h` | 高亮当前行 |
| `SPC t h i` | 高亮代码对齐线 |
| `SPC t h c` | 高亮光标所在列 |
| `SPC t h s` | 启用/禁用语法高亮 |
| `SPC t i` | 切换显示当前对齐(TODO) |
| `SPC t n` | 显示/隐藏行号 |
| `SPC t b` | 切换背景色 |
| `SPC t c` | 切换 conceal 模式 |
| `SPC t p` | 切换 paste 模式 |
| `SPC t t` | 打开 Tab 管理器 |
| `SPC T ~` | 显示/隐藏 Buffer 结尾空行行首的 `~` |
| `SPC T F` | 切换全屏(TODO) |
| `SPC T f` | 显示/隐藏 Vim 边框(GUI) |
| `SPC T m` | 显示/隐藏菜单栏 |
| `SPC T t` | 显示/隐藏工具栏 |

#### 状态栏

```toml
statusline_separator = 'arrow'
```

状态栏中功能模块内的字符显示与否，快捷键

| 快捷键 | Unicode | ASCII | 功能  |
| --- | --- | --- | --- |
| `SPC t 8` | ⑧   | 8   | 高亮指定列后所有字符 |
| `SPC t f` | ⓕ   | f   | 高亮指定列字符 |
| `SPC t s` | ⓢ   | s   | 语法检查 |
| `SPC t S` | Ⓢ   | S   | 拼写检查 |
| `SPC t w` | ⓦ   | w   | 行尾空格检查 |

其他快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC t m b` | 显示/隐藏电池状态 (需要安装 acpi) |
| `SPC t m c` | toggle the org task clock (available in org layer)(TODO) |
| `SPC t m i` | 显示/隐藏输入法 |
| `SPC t m m` | 显示/隐藏 SpaceVim 已启用功能 |
| `SPC t m M` | 显示/隐藏文件类型 |
| `SPC t m n` | toggle the cat! (if colors layer is declared in your dotfile)(TODO) |
| `SPC t m p` | 显示/隐藏光标位置信息 |
| `SPC t m t` | 显示/隐藏时间 |
| `SPC t m d` | 显示/隐藏日期 |
| `SPC t m T` | 显示/隐藏状态栏 |
| `SPC t m v` | 显示/隐藏版本控制信息 |

### 模糊搜索

#### Denite搜索快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `<Leader> f <Space>` | 模糊查找快捷键，并执行该快捷键 |
| `<Leader> f e` | 模糊搜索寄存器 |
| `<Leader> f h` | 模糊搜索 history/yank |
| `<Leader> f j` | 模糊搜索 jump, change |
| `<Leader> f l` | 模糊搜索 location list |
| `<Leader> f m` | 模糊搜索 output messages |
| `<Leader> f o` | 模糊搜索函数列表 |
| `<Leader> f q` | 模糊搜索 quickfix list |
| `<Leader> f r` | 重置上次搜索窗口 |

##### Denite模糊搜索窗口内的快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `<Tab>` / `Ctrl-j` | 下一个选项 |
| `Shift-<Tab>` / `Ctrl-k` | 上一个选项 |
| `jk` | 离开输入模式（仅支持 denite 和 unite 模块） |
| `Ctrl-w` | 删除光标前词语 |
| `<Enter>` | 执行默认动作 |
| `Ctrl-s` | 在分割窗口内打开 |
| `Ctrl-v` | 在垂直分割窗口内打开 |
| `Ctrl-t` | 在新的标签页里打开 |
| `Ctrl-g` | 推出模糊搜索插件 |

##### Denite模块可视模式下快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `Ctrl`+`h/k/l/r` | 未定义 |
| `Ctrl`+`l` | 刷新窗口 |
| `<Tab>` | 选择即将执行的动作 |
| `Space` | 切换标记当前选项 |
| `r` | 替换或者重命名 |
| `Ctrl`+`z` | 切换窗口分割方式 |

#### 实时代码检索

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC s /` | 在工程中使用默认工具实时检索代码 |

##### Flygrep 搜索窗口结果窗口内的常用快捷键：

| 快捷键 | 功能描述 |
| --- | --- |
| `<Esc>` | 关闭搜索窗口 |
| `<Enter>` | 打开当前选中的文件位置 |
| `Ctrl-t` | 在新标签栏打开选中项 |
| `Ctrl-s` | 在分屏打开选中项 |
| `Ctrl-v` | 在垂直分屏打开选中项 |
| `Ctrl-q` | 将搜索结果转移至 quickfix |
| `<Tab>` | 选中下一行文件位置 |
| `Shift-<Tab>` | 选中上一行文件位置 |
| `<Backspace>` | 删除上一个输入字符 |
| `Ctrl-w` | 删除光标前的单词 |
| `Ctrl-u` | 删除光标所在行的内容 |
| `Ctrl-k` | 删除光标后所有内容 |
| `Ctrl-a` / `<Home>` | 将光标移至行首 |
| `Ctrl-e` / `<End>` | 将光标移至行尾 |

### 基本操作

#### 光标及滚屏操作

| 快捷键 | 功能描述 |
| --- | --- |
| `h` | 向左移动光标 |
| `j` | 向下移动光标 |
| `k` | 向上移动光标 |
| `l` | 向右移动光标 |
| `<Up>` | 向上移动光标，不跳过折行 |
| `<Down>` | 向下移动光标，不跳过折行 |
| `H` | 移动光标至屏幕顶部 |
| `M` | 移动光标至屏幕中部 |
| `L` | 移动光标至屏幕底部 |
| `}` | 向前移动一个段落 |
| `{` | 向后移动一个段落 |
| `Ctrl-f` | 向下翻页 (`Ctrl-f` / `Ctrl-d`) |
| `Ctrl-b` | 向上翻页 (`C-b` / `C-u`) |
| `Ctrl-e` | 向下滚屏 (`3 Ctrl-e/j`) |
| `Ctrl-y` | 向上滚屏 (`3Ctrl-y/k`) |
| `Ctrl-Shift-Up` | 向上移动当前行 |
| `Ctrl-Shift-Down` | 向下移动当前行 |

#### 命令行Ex模式

常规模式下按下`:` 键后，可进入命令行Ex模式，

| 快捷键 | 功能描述 |
| --- | --- |
| `Ctrl-a` | 移动光标至行首 |
| `Ctrl-b` | 向左移动光标 |
| `Ctrl-f` | 向右移动光标 |
| `Ctrl-w` | 删除光标前词 |
| `Ctrl-u` | 移除光标前所有字符 |
| `Ctrl-k` | 移除光标后所有字符 |
| `Ctrl-c`/`Esc` | 离开命令行模式 |
| `Tab` | 选择下一个匹配 |
| `Shift-Tab` | 选择上一个匹配 |

#### 快速定位操作

easy-motion快捷键 `<Leader> \`

| 快捷键 | 功能描述 |
| --- | --- |
| `<Leader>f{char}` | Find {char} to the right. See f. |
| `<Leader>F{char}` | Find {char} to the left. See F. |
| `<Leader>t{char}` | Till before the {char} to the right. See t. |
| `<Leader>T{char}` | Till after the {char} to the left. See T. |
| `<Leader>w` | Beginning of word forward. See w. |
| `<Leader>W` | Beginning of WORD forward. See W. |
| `<Leader>b` | Beginning of word backward. See b. |
| `<Leader>B` | Beginning of WORD backward. See B. |
| `<Leader>e` | End of word forward. See e. |
| `<Leader>E` | End of WORD forward. See E. |
| `<Leader>ge` | End of word backward. See ge. |
| `<Leader>gE` | End of WORD backward. See gE. |
| `<Leader>j` | Line downward. See j. |
| `<Leader>k` | Line upward. See k. |
| `<Leader>n` | Jump to latest “/” or “?” forward. See n. |
| `<Leader>N` | Jump to latest “/” or “?” backward. See N. |
| `<Leader>s` | Find(Search) {char} forward and backward. |

#### 对齐操作

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC x a #` | 基于分隔符 # 进行文本对齐 |
| `SPC x t c` | 交换当前字符和前一个字符的位置 |
| `SPC x t C` | 交换当前字符和后一个字符的位置 |
| `SPC x t w` | 交换当前单词和前一个单词的位置 |
| `SPC x t W` | 交换当前单词和后一个单词的位置 |
| `SPC x t l` | 交换当前行和前一行的位置 |
| `SPC x t L` | 交换当前行和后一行的位置 |
| `SPC x u` | 将选中字符串转为小写 |
| `SPC x U` | 将选中字符串转为大写 |
| `SPC x w c` | 统计选中区域的单词数 |
| `SPC x w d` | show dictionary entry of word from [wordnik.com](http://wordnik.com) (TODO) |
| `SPC x <Tab>` | indent or dedent a region rigidly (TODO) |

#### 复制/粘贴
| 快捷键 | 功能描述 |
| --- | --- |
| `<Leader> p` | 粘贴系统剪切板文字至当前位置之后 |
| `<Leader> P` | 粘贴系统剪切板文字至当前位置之前 |

#### 跳转

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC j d` | 跳至当前目录某个文件夹 |
| `SPC j D` | 跳至当前目录某个文件夹（在另外窗口展示文件列表） |
| `SPC j i` | 跳至当前文件的某个函数，使用 Denite 打开语法树 |
| `SPC j u` | 跳至窗口某个 URL  |
| `SPC j v` | 跳至某个 Vim 函数的定义处  |
| `SPC j w` | 跳至 Buffer 中某个单词 (easymotion) |


### 其他操作

#### 以`g`为前缀的快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `g '` | 跳至标签 |
| `g a` | 打印光标字符的 ascii 值 |
| `g d` | 跳至定义处 |

#### 以`z`为前缀的快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `z =` | spelling suggestions |
| `z A` | toggle folds recursively |
| `z C` | close folds recursively |
| `z D` | delete folds recursively |
| `z E` | eliminate all folds |
| `z a` | toggle a fold |
| `z b` | redraw, cursor line at bottom |
| `z c` | close a fold |
| `z d` | delete a fold |
| `z e` | right scroll horizontally to cursor position |
| `z f` | create a fold for motion |
| `z g` | mark good spelled |
| `z h` | scroll screen N characters to right |
| `z i` | toggle foldenable |
| `z j` | mode to start of next fold |
| `z k` | mode to end of previous fold |
| `z l` | scroll screen N characters to left |
| `z m` | subtract one from `foldlevel` |
| `z n` | reset `foldenable` |
| `z o` | open fold |

#### 移动代码块

| 快捷键 | 功能描述 |
| --- | --- |
| `<` / `Shift-Tab` | 向左移动文本 |
| `>` / `Tab` | 向右移动文本 |
| `Ctrl-Shift-Up` | 向上移动选中行 |
| `Ctrl-Shift-Down` | 向下移动选中行 |

#### 代码缩进

```toml
[options]
default_indent = 2
expand_tab = true
```

#### 增减数字

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC n +` | 为光标下的数字加 1 并进入 临时快捷键状态 |
| `SPC n -` | 为光标下的数字减 1 并进入 临时快捷键状态 |

 `10 SPC n +` 将为光标下的数字加 `10`

 #### 增删注释

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC ;` | 进入注释操作模式 |
| `SPC c l` | 注释/反注释当前行 |
| `SPC c u` | 反注释行 |
| `SPC c p` | 注释/反注释段落 |
| `SPC c t` | 注释/反注释到行 |

`SPC ; 4 j`，这个组合键会注释当前行以及下方的 4 行。这个数字即为相对行号

#### 编辑历史记录

快捷键 `F7` 查看，默认会在左侧打开一个编辑历史可视化窗口

在编辑历史窗口内的快捷键如下：

| 快捷键 | 功能描述 |
| --- | --- |
| `?` | toggle_help |
| `G` | move_bottom |
| `J` | move\_older\_write |
| `K` | move\_newer\_write |
| `P` | play_to |
| `/` | search |
| `<CR>` | preview |
| `d` | diff |
| `i` | toggle_inline |
| `j` | move_older |
| `k` | move_newer |
| `n` | next_match |
| `o` | preview |
| `q` | quit |
| `r` | diff |
| `gg` | move_top |

### 窗口window管理

窗口管理快捷键有一个统一的前缀，默认的前缀 `[Window]` 是按键 `s`

```toml
[options]
windows_leader = "s"
```

常见编辑器窗口

| 按键  | 功能描述 |
| --- | --- |
| `<F1>` | 打开VIM帮助文档 |
| `<F2>` | 打开、关闭语法树/toc |
| `<F3>` | 打开、关闭文件树 |
| `<F7>` | 打开、关闭历史编辑窗口 |
| `Ctrl-<Down>` | 切换至下方窗口 |
| `Ctrl-<Up>` | 切换至上方窗口 |
| `Ctrl-<Left>` | 切换至左边窗口 |
| `Ctrl-<Right>` | 切换至右边窗口 |

#### 窗口跳转

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC 1～9` | 跳至窗口 1～9 |

#### 窗口分屏操作

| 快捷键 | 功能描述 |
| --- | --- |
| `q` | 智能关闭当前窗口 |
| `[Window] v` | 水平分屏 |
| `[Window] V` | 水平分屏，并编辑上一个文件 |
| `[Window] g` | 垂直分屏 |
| `[Window] G` | 垂直分屏，并编辑上一个文件 |
| `[Window] t` | 新建新的标签页 |
| `[Window] o` | 关闭其他窗口 |
| `[Window] x` | 关闭当前缓冲区，并保留新的空白缓冲区 |
| `[Window] q` | 关闭当前缓冲区 |
| `[Window] Q` | 关闭当前窗口 |
| `Shift-<Tab>` | 切换至前一窗口 |

常规模式下的按键 `q` 被用来快速关闭窗口，其原生的功能可以使用 `<Leader> q r` 来代替。

#### 窗口其他操作

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC w .` | 启用窗口临时快捷键 |
| `SPC w <Tab>` | 在同一标签内进行窗口切换 |
| `SPC w =` | 对齐分离的窗口 |
| `SPC w c` | 进入阅读模式，浏览当前窗口 (需要 tools 模块) |
| `SPC w C` | 选择某一个窗口，并且进入阅读模式 (需要 tools 模块) |
| `SPC w d` | 删除一个窗口 |
| `SPC w D` | 选择一个窗口并关闭 |
| `SPC w F` | 新建一个新的标签页 |
| `SPC w h` | 移至左边窗口 |
| `SPC w H` | 将窗口向左移动 |
| `SPC w j` | 移至下方窗口 |
| `SPC w J` | 将窗口向下移动 |
| `SPC w k` | 移至上方窗口 |
| `SPC w K` | 将窗口向上移动 |
| `SPC w l` | 移至右方窗口 |
| `SPC w L` | 将窗口向右移动 |
| `SPC w m` | 最大化/最小化窗口（最大化相当于关闭其它窗口） |
| `SPC w M` | 选择窗口进行替换 |
| `SPC w o` | 按序切换标签页 |
| `SPC w p m` | 使用弹窗打开消息 |
| `SPC w p p` | 关闭当前弹窗窗口 |
| `SPC w r` | 顺序切换窗口 |
| `SPC w R` | 逆序切换窗口 |
| `SPC w s/-` | 水平分割窗口 |
| `SPC w S` | 水平分割窗口，并切换至新窗口 |
| `SPC w u` | 恢复窗口布局 |
| `SPC w U` | 撤销恢复窗口布局 |
| `SPC w v//` | 垂直分离窗口 |
| `SPC w V` | 垂直分离窗口，并切换至新窗口 |
| `SPC w w` | 切换至前一窗口 |
| `SPC w W` | 选择一个窗口 |
| `SPC w x` | 切换窗口文件 |

### 标签tab管理

#### 标签管理器

可使用 `SPC t t` 打开内置的标签管理器，标签管理器内的快捷键如下：

| 快捷键 | 功能描述 |
| --- | --- |
| `o` | 展开或关闭标签目录 |
| `r` | 重命名光标下的标签页 |
| `n` | 在光标位置下新建命名标签页 |
| `N` | 在光标位置下新建匿名标签页 |
| `x` | 删除光标下的标签页 |
| `Ctrl-S-<Up>` | 向上移动光标下的标签页 |
| `Ctrl-S-<Down>` | 向下移动光标下的标签页 |
| `<Enter>` | 跳至光标所对应的标签窗口 |

#### 标签跳转

| 快捷键 | 功能描述 |
| --- | --- |
| `<Leader> 1~0` | 跳至标签栏序号 1~10 |
| `<Leader> <Shift> 1~9` | 跳至标签栏序号 11~20 |
| `<Mouse-left>` | 切换至该标签 |
| `<Mouse-middle>` | 删除该标签 |

### 缓冲区Buffer管理

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC b .` | 启用缓冲区临时快捷键 |
| `SPC b b` | 通过模糊搜索工具进行缓冲区切换，需要启用一个模糊搜索工具模块 |
| `SPC b h` | 打开欢迎界面, 等同于快捷键 `SPC a s` |
| `SPC b m` | 打开消息缓冲区 |

缓冲区（Buffer）操作相关快捷键都是以 `SPC b` 为前缀的

#### 新建空白buffer

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC b N h` | 在左侧新建一个窗口，并在其中新建空白 buffer |
| `SPC b N j` | 在下方新建一个窗口，并在其中新建空白 buffer |
| `SPC b N k` | 在上方新建一个窗口，并在其中新建空白 buffer |
| `SPC b N l` | 在右侧新建一个窗口，并在其中新建空白 buffer |
| `SPC b N n` | 在当前窗口新建一个空白 buffer |

#### 切换buffer

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC <Tab>` | 切换至前一缓冲区，常用于两个缓冲区来回切换 |
| `SPC b n` | 切换至下一个缓冲区，排除特殊插件的缓冲区 |
| `SPC b p` | 切换至前一个缓冲区，排除特殊插件的缓冲区 |
| `SPC b P` | 使用系统剪切板内容替换当前缓冲区 |
| `SPC b R` | 从磁盘重新读取当前缓冲区所对应的文件 |
| `SPC b s` | switch to the *scratch* buffer (create it if needed) (TODO) |
| `SPC b w` | 切换只读权限 |
| `SPC b Y` | 将整个缓冲区复制到系统剪切板 |
| `z f` | Make current function or comments visible in buffer as much as possible (TODO) |

#### 删除buffer

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC b d` | 删除当前缓冲区，但保留编辑窗口 |
| `SPC u SPC b d` | kill the current buffer and window (does not delete the visited file) (TODO) |
| `SPC b D` | 选择一个窗口，并删除其缓冲区 |
| `SPC u SPC b D` | kill a visible buffer and its window using ace-window(TODO) |
| `SPC b c` | 删除其它已保存的缓冲区 |
| `SPC b C-d` | 删除其它所有缓冲区 |
| `SPC b C-D` | kill buffers using a regular expression(TODO) |
| `SPC b e` | 清除当前缓冲区内容，需要手动确认 |
| `SPC u SPC b m` | kill all buffers and windows except the current one(TODO) |

### 文件浏览器

默认的快捷键是`F3`, SpaceVim 也提供了另外一组快捷键 `SPC f t` 和 `SPC f T` 来打开文件树

```toml
[options]
# 文件树插件可选值包括：
# - vimfiler （默认）
# - nerdtree
# - defx
filemanager = "defx"
filetree_direction = "right"
```

#### 文件浏览器内的快捷键

| 快捷键 | 功能描述 |
| --- | --- |
| `<F3>` / `SPC f t` | 切换文件树 |
| **文件树内的快捷键** |     |
| `<Left>` / `h` | 移至父目录，并关闭文件夹 |
| `<Down>` / `j` | 向下移动光标 |
| `<Up>` / `k` | 向上移动光标 |
| `<Right>` / `l` | 展开目录，或打开文件 |
| `N` | 在光标位置新建文件 |
| `y y` | 复制光标下文件路径至系统剪切板 |
| `y Y` | 复制光标下文件至系统剪切板 |
| `P` | 在光标位置黏贴文件 |
| `.` | 切换显示隐藏文件 |
| `s v` | 分屏编辑该文件 |
| `s g` | 垂直分屏编辑该文件 |
| `p` | 预览文件 |
| `i` | 切换至文件夹历史 |
| `v` | 快速查看 |
| `>` | 放大文件树窗口宽度 |
| `<` | 缩小文件树窗口宽度 |
| `g x` | 使用相关程序执行该文件 |
| `'` | 标记光标下的文件（夹） |
| `V` | 清除所有标记 |
| `Ctrl`+`r` | 刷新页面 |

#### 文件分屏操作

如果只有一个可编辑窗口，则在该窗口中打开选择的文件，否则则需要指定窗口来打开文件：

| 快捷键 | 功能描述 |
| --- | --- |
| `l` / `<Enter>` | 打开文件 |
| `sg` | 水平分屏打开文件 |
| `sv` | 垂直分屏打开文件 |

#### 文件操作

文件操作相关的快捷键都是以 `SPC f` 为前缀的：

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC f /` | 使用 `find` 或者 `fd` 命令查找文件，支持参数提示 |
| `SPC f b` | 跳至文件书签 |
| `SPC f c` | copy current file to a different location(TODO) |
| `SPC f C d` | 修改文件编码 unix -> dos |
| `SPC f C u` | 修改文件编码 dos -> unix |
| `SPC f D` | 删除文件以及 buffer，需要手动确认 |
| `SPC f E` | open a file with elevated privileges (sudo edit)(TODO) |
| `SPC f f` | 打开文件 |
| `SPC f F` | 打开光标下的文件 |
| `SPC f o` | 代开文件树，并定位到当前文件 |
| `SPC f R` | rename the current file(TODO) |
| `SPC f s` / `Ctrl-s` | 保存文件 (:w) |
| `SPC f W` | 使用管理员模式保存 |
| `SPC f S` | 保存所有文件 |
| `SPC f r` | 打开文件历史 |
| `SPC f t` | 切换侧栏文件树 |
| `SPC f T` | 打开文件树侧栏 |
| `SPC f d` | Windows 下显示/隐藏磁盘管理器 |
| `SPC f y` | 复制并显示当前文件的绝对路径 |
| `SPC f Y` | 复制并显示当前文件的远程路径 |

## SpaceVim进阶

### 工程管理

当打开一个文件时，SpaceVim 会自动切换当前目录至包含该文件的项目根目录， 项目根目录的检测依据 `project_rooter_patterns` 这一选项，其默认值为：

```toml
[options]
    project_rooter_patterns = ['.git/', '_darcs/', '.hg/', '.bzr/', '.svn/']

```

项目管理器默认自动检测最外层的项目根目录，如果需要自动检测最内层的项目根目录， 可将选项 `project_rooter_outermost` 选项改为 `false`。

```toml
[options]
    project_rooter_patterns = ['.git/', '_darcs/', '.hg/', '.bzr/', '.svn/']
    project_rooter_outermost = false

```

在自动检测项目根目录时，有时候我们需要忽略掉一些目录，可以表达式前面添加 `!`， 比如，忽略掉 `node_packages/` 文件夹：

```toml
[options]
    project_rooter_patterns = ['.git/', '_darcs/', '.hg/', '.bzr/', '.svn/', '!node_packages/']
    project_rooter_outermost = false

```

工程管理的命令以 `p` 开头：

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC p '` | 在当前工程的根目录打开 shell（需要 shell 模块） |

#### 在工程中搜索文件

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC p f` | 在当前工程中查找文件 |
| `SPC p /` | 在当前工程中搜索文本内容 |
| `SPC p k` | 关闭当前工程的所有缓冲区 |
| `SPC p p` | 显示所有工程 |

`SPC p p` 将会列出最近使用的项目清单，默认会显示最多 20 个， 这一数量可以使用 `projects_cache_num` 来修改。

为了可以夸 Vim 进程读取历史打开的项目信息，这一功能使用了缓存机制。 如果需要禁用这一缓存功能，可以将 `enable_projects_cache` 设为 `false`。

```toml
[options]
    enable_projects_cache = true
    projects_cache_num = 20

```

#### 自定义跳转文件

若要实现自定义文件跳转功能，需要在项目根目录新建一个 `.project_alt.json` 文件， 并在此文件内指定文件跳转的规则。此后可以使用名`:A`跳转到相关文件， 同时可以加上参数指定跳转类型，比如 `:A doc`。与此同时，可以在命令后加入感叹号 `:A!`， 强制根据配置文件重新分析项目跳转文件结构。若未指定跳转类型，默认的类型为 `alternate`。

配置文件示例：

```toml
{
  "autoload/SpaceVim/layers/lang/*.vim": {
    "doc": "docs/layers/lang/{}.md",
    "test": "test/layer/lang/{}.vader"
  }
}
```

### 书签管理

在浏览代码时，通常需要给指定位置添加标签，方便快速跳转，在 SpaceVim 中可以使用如下快捷键来管理标签。 这一功能需要载入 tools 模块：

```toml
[layers]
    name = "tools"

```

| 快捷键 | 功能描述 |
| --- | --- |
| `m a` | 显示书签列表 |
| `m m` | 切换当前行标签状态 |
| `m n` | 跳至下一个书签 |
| `m p` | 跳至前一个书签 |
| `m i` | 给当前行标签添加说明 |

正因为占用了以上几个快捷键，以下几个寄存器无法用来记忆当前位置了：`a`, `m`, `n`, `p`, `i`。 当然，也可以在启动函数里将 `<Leader> m` 映射为 `m` 键，如此便可使用 `<Leader> m a` 来代替 `m a`。

```toml
function! myspacevim#before() abort
    nnoremap <silent><Leader>m m
endfunction

```

### 任务管理

通过内置的任务管理系统，可以快速集成外部命令工具，类似于 vscode 的任务管理系统， 在 SpaceVim 中，目前支持的任务配置文件包括两种：

- `~/.SpaceVim.d/tasks.toml`：全局配置文件
- `.SpaceVim.d/tasks.toml`：项目局部配置文件

全局配置中定义的任务，默认会被项目局部配置文件中定义的任务覆盖掉。

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC p t e` | 编辑任务配置文件 |
| `SPC p t r` | 选定任务并执行 |
| `SPC p t l` | 列出所有任务 |

![task_manager](https://user-images.githubusercontent.com/13142418/94822603-69d0c700-0435-11eb-95a7-b0b4fef91be5.png)

#### 自定义任务

以下为一个简单的任务配置示例，异步运行 `echo hello world`，并将结果打印至输出窗口。

```toml
[my-task]
    command = 'echo'
    args = ['hello world']
```

![task hello world](https://user-images.githubusercontent.com/13142418/74582981-74049900-4ffd-11ea-9b38-7858042225b9.png)

对于不需要打印输出结果，后台运行的任务，可以设置 `isBackground` 为 `true`:

```toml
[my-task]
    command = 'echo'
    args = ['hello world']
    isBackground = true

```

任务的配置，可以设置如下关键字：

- **command**: 需要运行的命令。
- **args**: 传递给命令的参数，值为字符串数组
- **options**: 设置命令运行的一些选项，比如 `cwd`,`env` 或者 `shell`。
- **isBackground**: 可设定的值为 `true` 或者 `false`， 默认是 `false`， 设置是否需要后台运行任务
- **description**: 关于该任务的一段简短介绍

当启动一个任务时，默认会关闭前一个任务，如果需要让任务一直保持后台运行， 可以将 `isBackground` 设为 `true`。

在编辑任务配置文件时，可以使用一些预设定的变量，以下列出目前已经支持的预设定变量：

- **${workspaceFolder}**: \- 当前项目的根目录；
- **${workspaceFolderBasename}**: \- 当前项目根目录所在父目录的文件夹名称；
- **${file}**: \- 当前文件的绝对路径；
- **${relativeFile}**: \- 当前文件相对项目根目录的相对路径；
- **${relativeFileDirname}**: \- 当前文件所在的文件夹相对项目根目录的相对路径；
- **${fileBasename}**: \- 当前文件的文件名
- **${fileBasenameNoExtension}**: \- 当前文件的文件名，不包括后缀名
- **${fileDirname}**: \- 当前文件所在的目录的绝对路径
- **${fileExtname}**: \- 当前文件的后缀名
- **${lineNumber}**: \- 光标所在行号

例如：假定目前正在编辑文件 `/home/your-username/your-project/folder/file.ext` ，光标位于第十行； 该文件所在的项目根目录为 `/home/your-username/your-project`，那么任务系统的预设定变量的值为：

- **${workspaceFolder}**: \- `/home/your-username/your-project/`
- **${workspaceFolderBasename}**: \- `your-project`
- **${file}**: \- `/home/your-username/your-project/folder/file.ext`
- **${relativeFile}**: \- `folder/file.ext`
- **${relativeFileDirname}**: \- `folder/`
- **${fileBasename}**: \- `file.ext`
- **${fileBasenameNoExtension}**: \- `file`
- **${fileDirname}**: \- `/home/your-username/your-project/folder/`
- **${fileExtname}**: \- `.ext`
- **${lineNumber}**: \- `10`

#### 任务自动识别

SpaceVim 目前支持自动识别以下构建系统的任务：npm。 任务管理插件将自动读取并分析 npm 系统的文件`package.json`。 比如，克隆示例项目 [eslint-starter](https://github.com/spicydonuts/eslint-starter)， 编辑其中的任意文件，然后按下快捷键`SPC p t r`，将会显示如下任务列表：

![task-auto-detection](https://user-images.githubusercontent.com/13142418/75089003-471d2c80-558f-11ea-8aea-cbf7417191d9.png)

#### 任务提供源

任务提供源可以自动检测并新建任务。例如，一个任务提供源可以自动检测是否存在项目构建文件，比如：`package.json`， 如果存在则根据其内容创建 npm 的构建任务。

在 SpaceVim 里，如果需要新建任务提供源，需要使用启动函数，任务提供源是一个 Vim 函数，该函数返回一系列任务对象。

以下为一个简单的示例：

```vimscript
function! s:make_tasks() abort
    if filereadable('Makefile')
        let subcmd = filter(readfile('Makefile', ''), "v:val=~#'^.PHONY'")
        if !empty(subcmd)
            let commands = split(subcmd[0])[1:]
            let conf = {}
            for cmd in commands
                call extend(conf, {
                            \ cmd : {
                            \ 'command': 'make',
                            \ 'args' : [cmd],
                            \ 'isDetected' : 1,
                            \ 'detectedName' : 'make:'
                            \ }
                            \ })
            endfor
            return conf
        else
            return {}
        endif
    else
        return {}
    endif
endfunction
call SpaceVim#plugins#tasks#reg_provider(funcref('s:make_tasks'))
```

将以上内容加入启动函数，在 SpceVim 仓库内按下 `SPC p t r` 快捷键，将会展示如下任务：

![task-make](https://user-images.githubusercontent.com/13142418/75105016-084cac80-564b-11ea-9fe6-75d86a0dbb9b.png)

### ledit多光标编辑

SpaceVim 内置了 iedit 多光标模式，可快速进行多光标编辑。这一功能引入了两个新的模式：`iedit-Normal` 模式和 `iedit-Insert`。

`iedit` 模式默认的颜色是 `red`/`green`，但它也基于当前的主题。

#### Iedit 快捷键

模式转换:

前面提到 Iedit 引入了两个新的模式，在这两个新的模式以及 Vim 自身模式之间转换的快捷键如下：

| 快捷键 | From | to  |
| --- | --- | --- |
| `SPC s e` | Normal/Visual | iedit-Normal |
| `<Esc>` | iedit-Normal | Normal |

##### 在 iedit-Normal 模式中

`iedit-Normal` 模式继承自 Vim 的 Normal 模式, 下面所列举的是 `iedit-Normal` 模式专属的快捷键。

| 快捷键 | 功能描述 |
| --- | --- |
| `<Esc>` | 切换回 Normal 模式 |
| `i` | 切换至 `iedit-Insert` 模式，类似于一般模式下的 `i` |
| `a` | 切换至 `iedit-Insert` 模式，类似于一般模式下的 `a` |
| `I` | 跳至当前 occurrence 并进入 `iedit-Insert` 模式，类似于一般模式下的 `I` |
| `A` | 跳至当前 occurrence 并进入 `iedit-Insert` 模式，类似于一般模式下的 `A` |
| `<Left>` / `h` | 左移光标，类似于一般模式下的 `h` |
| `<Right>` / `l` | 右移光标，类似于一般模式下的 `l` |
| `0` / `<Home>` | 跳至当前 occurrence 的开头，类似于一般模式下的 `0` |
| `$` / `<End>` | 跳至当前 occurrence 的结尾，类似于一般模式下的 `$` |
| `C` | 删除所有 occurrences 中从光标位置开始到 occurrences 结尾的字符，类似于一般模式下的 `C` |
| `D` | 删除所有 occurrences 类似于一般模式下的 `D` |
| `s` | 删除所有 occurrences 中光标下的字符并进入 `iedit-Insert` 模式，类似于一般模式下的 `s` |
| `S` | 删除所有 occurrences 并进入 `iedit-Insert` 模式，类似于一般模式下的 `S` |
| `x` | 删除所有 occurrences 中光标下的字符，类似于一般模式下的 `x` |
| `X` | 删除所有 occurrences 中光标前的字符，类似于一般模式下的 `X` |
| `gg` | 跳至第一个 occurrence，类似于一般模式下的 `gg` |
| `G` | 跳至最后一个 occurrence，类似于一般模式下的 `G` |
| `f{char}` | 向右移动光标至字符 `{char}` 首次出现的位置 |
| `n` | 跳至下一个 occurrence |
| `N` | 跳至上一个 occurrence |
| `p` | 替换所有 occurrences 为最后复制的文本 |
| `<Tab>` | toggle current occurrence |

**In iedit-Insert mode:**

| 快捷键 | 功能描述 |
| --- | --- |
| `Ctrl-g` / `<Esc>` | 回到 `iedit-Normal` 模式 |
| `Ctrl-b` / `<Left>` | 左移光标 |
| `Ctrl-f` / `<Right>` | 右移光标 |
| `Ctrl-a` / `<Home>` | 跳至当前 occurrence 的开头 |
| `Ctrl-e` / `<End>` | 跳至当前 occurrence 的结尾 |
| `Ctrl-w` | 删除光标前的词 |
| `Ctrl-k` | 删除光标后的词 |
| `Ctrl-u` | 删除光标前所有字符 |
| `Ctrl-h` / `<BackSpace>` | 删除光标前字符 |
| `<Delete>` | 删除光标后字符 |

### 高亮光标下变量

SpaceVim 支持高亮当前光标下的变量，并且启动一个临时快捷键窗口， 提示可以通过快捷键进行修改高亮范围，以及下一步的操作。

目前支持的高亮范围包括：

- 整个缓冲区（buffer）
- 当前函数内（function）
- 可见区域（visible area）

使用快捷键 `SPC s h` 来高亮光标下的符号。

可使用如下快捷键在已高亮的变量间跳转：

| 快捷键 | 功能描述 |
| --- | --- |
| `*` | 在当前缓冲区正向搜索光标下变量 |
| `#` | 在当前缓冲区逆向搜索光标下变量 |
| `SPC s e` | 启动 iedit 模式，编辑光标下变量 |
| `SPC s h` | 使用默认的的范围高亮光标下的变量 |
| `SPC s H` | 高亮当前缓冲区下所有的光标下变量 |

在高亮临时快捷键模式下可使用如下快捷键:

| 快捷键 | 功能描述 |
| --- | --- |
| `e` | 启动 iedit 模式 |
| `n` | 跳至下一个匹配处 |
| `N` / `p` | 跳至上一个匹配处 |
| `b` | 在整个缓冲区内高亮该匹配 |
| `/` | 在整个工程内检索当前匹配 |
| `<Tab>` | 切换当前匹配高亮状态 |
| `r` | 切换匹配的范围 |
| `R` | 重置匹配的范围 |
| Any other key | 退出该临时快捷键模式 |

### 异步运行与交互编程

SpaceVim 提供了一个异步执行命令和交互式编程的插件， 在大多数语言模块中，已经为该语言定义了默认的执行命令，通常快捷键为 `SPC l r`。 如果需要添加额外的命令，可以使用启动函数。比如：添加使用 F5 按键异步编译当前项目。

```
nnoremap <silent> <F5> :call SpaceVim#plugins#runner#open('make')

```

目前，SpaceVim 支持如下特性：

- 使用默认命令一键运行当前文件
- 使用系统文件管理器选择文件并执行
- 根据文件顶部标识，选择合适解析器
- 中断代码运行
- 底部窗口异步展示运行结果
- 设置默认的运行语言
- 选择指定语言来运行
- 支持交互式编程
- 运行选择的代码片段

### 错误处理

SpaceVim 通过默认通过 [checkers](https://spacevim.org/cn/layers/checkers/) 模块来进行文件语法检查，默认在保存文件时进行错误检查。

错误管理导航键 (以 `e` 开头)：

| 快捷键 | 功能描述 |
| --- | --- |
| `SPC t s` | 切换语法检查器 |
| `SPC e c` | 清除所有错误 |
| `SPC e h` | describe a syntax checker |
| `SPC e l` | 切换显示错误/警告列表 |
| `SPC e n` | 跳至下一错误 |
| `SPC e p` | 跳至上一个错误 |
| `SPC e v` | verify syntax checker setup (useful to debug 3rd party tools configuration) |
| `SPC e .` | 错误暂态（error transient state) |

下一个/上一个错误导航键和错误暂态(error transinet state) 可用于浏览语法检查器和位置列表缓冲区的错误， 甚至可检查 Vim 位置列表的所有错误。这包括下面的例子：在已被保存的位置列表缓冲区进行搜索。 默认提示符：

| 提示符 | 描述  | 自定义选项 |
| --- | --- | --- |
| `✖` | Error | `error_symbol` |
| `➤` | warning | `warning_symbol` |
| `ⓘ` | Info | `info_symbol` |

**quickfix 列表移动：**

| 快捷键 | 功能描述 |
| --- | --- |
| `<Leader> q l` | 打开 quickfix 列表窗口 |
| `<Leader> q c` | 清除 quickfix 列表 |
| `<Leader> q n` | 跳到 quickfix 列表中下一个位置 |
| `<Leader> q p` | 跳到 quickfix 列表中上一个位置 |

### 格式规范

SpaceVim 添加了 [EditorConfig](https://editorconfig.org/) 支持，通过一个配置文件来为不同的文件格式设置对应的代码格式规范， 这一工具兼容多种文本编辑器和集成开发环境。

更多配置方式，可以阅读其官方文档：[editorconfig-vim package’s documentation](https://github.com/editorconfig/editorconfig-vim/blob/master/README.md).

### 后台服务

SpaceVim 在启动时启动了一个后台服务。无论何时，当你关闭了 Vim 窗口，该服务器就会被关闭。

**连接到 Vim 服务器**

如果你使用 Neovim, 你需要安装[neovim-remote](https://github.com/mhinz/neovim-remote)，然后增加如下配置到你的 bashrc。

```zsh
export PATH=$PATH:$HOME/.SpaceVim/bin

```

使用命令 `svc` 在一个已存在的 Vim 服务器上打开文件，使用命令 `nsvc` 在一个已存在的 Neovim 服务器上打开文件。

![server-and-client](https://user-images.githubusercontent.com/13142418/32554968-7164fe9c-c4d6-11e7-95f7-f6a6ea75e05b.gif)
