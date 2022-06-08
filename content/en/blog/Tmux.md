---
title: Tmux
encrypt: false
enc_pwd: askding
categories:
  - Tools
date: 2020-12-07 18:14:43
top:
tags:
layout:
---

![tmux-1](../images/tmux-1.gif)



## Installation

```
$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .
```
## Key-Bindings
  - `<prefix>` <kbd>Ctrl</kbd> + <kbd>a</kbd> or <kbd>Ctrl</kbd> + <kbd>b</kbd>
  - `<prefix> c`  <kbd>Ctrl</kbd> + <kbd>a</kbd> + <kbd>c</kbd>
  - `<prefix> C-c`  <kbd>Ctrl</kbd> + <kbd>a</kbd>  + <kbd>Ctrl</kbd> + <kbd>c</kbd>
  - `<prefix> ?` manual 
  - `<prefix> d` detached the current session
  - `<ctrl> +d` kill current session
  - `<prefix> d` close current panel

This configuration uses the following bindings:

 - `<prefix> e` opens `~/.tmux.conf.local`
 - `<prefix> r` reloads `~/.tmux.conf.local`
 - `C-l` clears both the screen and the tmux history

### Session Operation:

- `<prefix> C-c` creates a new session
- `<prefix> C-f` lets you switch to another session by name
- `<prefix> $` rename session name
- `<prefix> s` list sessions

### Windows Operation:
- `<prefix> C-h` and `<prefix> C-l` let you navigate windows 
- `<prefix> c` create window
- `<prefix> w` list windows 
- `<prefix> Tab` brings you to the last active window
- `<prefix> 1,2,3` let you navigate to specific window
- `<prefix> ,` rename current window
- `<prefix> &` close current window
- `<prefix> f` find specific window by name

### Panel Operation:
 - `<prefix> /` splits the current pane vertically
 - `<prefix> -` splits the current pane horizontally
 - `<prefix> h`, `<prefix> j`, `<prefix> k` and `<prefix> l` let you navigate
 - `<prefix> o` navigate panels
 - `<prefix> H`, `<prefix> J`, `<prefix> K`, `<prefix> L` let you resize panes
 - `<prefix> <` and `<prefix> >` let you swap panes
 - `<prefix> +` maximizes the current pane to a new panel
 - `<prefix> x` kill the current panel

 - `<prefix> m` toggles mouse mode on or off
 - `<prefix> <space> ` 切换窗格布局

 - `<prefix> Enter` enters copy-mode
 - `<prefix> b` lists the paste-buffers
 - `<prefix> p` pastes from the top paste-buffer
 - `<prefix> P` lets you choose the paste-buffer to paste from

## Configuration
-------------
file: `~/.tmux.conf.local`
variables:

```
tmux_conf_theme_left_separator_main='\uE0B0'
tmux_conf_theme_left_separator_sub='\uE0B1'
tmux_conf_theme_right_separator_main='\uE0B2'
tmux_conf_theme_right_separator_sub='\uE0B3'
```
### Configuring the status line


Edit the `~/.tmux.conf.local` file (`<prefix> e`) and adjust the
`tmux_conf_theme_status_left` and `tmux_conf_theme_status_right` variables to
your own preferences.

This configuration supports the following builtin variables:

 - `#{battery_bar}`: horizontal battery charge bar
 - `#{battery_percentage}`: battery percentage
 - `#{battery_status}`: is battery charging or discharging?
 - `#{battery_vbar}`: vertical battery charge bar
 - `#{circled_session_name}`: circled session number, up to 20
 - `#{hostname}`: SSH/Mosh aware hostname information
 - `#{hostname_ssh}`: SSH/Mosh aware hostname information, blank when not
   connected to a remote server through SSH/Mosh
 - `#{loadavg}`: load average
 - `#{pairing}`: is session attached to more than one client?
 - `#{prefix}`: is prefix being depressed?
 - `#{root}`: is current user root?
 - `#{synchronized}`: are the panes synchronized?
 - `#{uptime_y}`: uptime years
 - `#{uptime_d}`: uptime days, modulo 365 when `#{uptime_y}` is used
 - `#{uptime_h}`: uptime hours
 - `#{uptime_m}`: uptime minutes
 - `#{uptime_s}`: uptime seconds
 - `#{username}`: SSH/Mosh aware username information
 - `#{username_ssh}`: SSH aware username information, blank when not connected
   to a remote server through SSH/Mosh

Beside custom variables mentioned above, the `tmux_conf_theme_status_left` and
`tmux_conf_theme_status_right` variables support usual tmux syntax, e.g. using
`#()` to call an external command that inserts weather information provided by
[wttr.in]:
```
tmux_conf_theme_status_right='#{prefix}#{pairing}#{synchronized} #(curl wttr.in?format=3) , %R , %d %b | #{username}#{root} | #{hostname} '
```

![tmux-2](../images/tmux-2.png)

[wttr.in]: https://github.com/chubin/wttr.in#one-line-output

### Accessing the macOS clipboard from within tmux sessions

[Chris Johnsen created the `reattach-to-user-namespace`
utility][reattach-to-user-namespace] that makes `pbcopy` and `pbpaste` work
again within tmux.

To install `reattach-to-user-namespace`, use either [MacPorts][] or
[Homebrew][]:

   ` $ port install tmux-pasteboard`

or

    `$ brew install reattach-to-user-namespace`

Once installed, `reattach-to-usernamespace` will be automatically detected.

[MacPorts]: http://www.macports.org/
[Homebrew]: http://brew.sh/

[tmux终端复用神器](https://www.cnblogs.com/kevingrace/p/6496899.html)
[Tmux 快捷键](https://gist.github.com/ryerh/14b7c24dfd623ef8edc7)
[多窗口管理器Tmux - 从入门到精通](https://segmentfault.com/a/1190000016283278)
[优雅地使用命令行：Tmux 终端复用 ](https://harttle.land/2015/11/06/tmux-startup.html)
[[结对编程利器：SSH和Tmux](https://segmentfault.com/a/1190000000423141)]