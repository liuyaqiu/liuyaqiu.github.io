---
layout: post
title: "tmux命令与快捷键"
categories: tutorial
---

如果需要同时在命令行中执行多个任务，最简单的方式是打开多个terminal，但是多个terminal往往并不方便。`tmux`是一款经典的终端复用器，可以复用一个终端，在同一个终端中执行多个任务。与`tmux`比较相似的是`gnu screen`，但是`tmux`更加强大。

`tmux`除了能够解决终端复用的问题，还能够解决后台运行的问题。当在远端服务器主机上运行程序时，如果是执行运行时间较长的任务，必须保证terminal的长期稳定的连接，否则任务将随着terminal的中断而终止。而实际上保持持久稳定的terminal连接不方便，甚至很困难。`tmux`可以解决这个问题，当离开`tmux`窗口，甚至终止`tmux`所在的terminal后，重新从另一个terminal中运行`tmux`，仍然能够进入此前的任务运行环境，因为`tmux`中的任务是脱离terminal后台运行的。除非关闭`tmux`或者服务器主机关机，否则`tmux`任务不会被中断。在服务器上运行`tmux`，既可以通过一个terminal同时管理和运行多个任务，而且不必维持持续连接。

# **Sessions**
```
tmux new -s basic --> 启动一个名为basic的session
<prefix> C-c --> 创建一个新的session(自定义)
<prefix> C-f --> 以名称切换到某个session(自定义)
tmux ls (<prefix> s) --> 显示当前的session
tmux kill-session -t basic --> 关闭名为basic的session
tmux kill-session -a --> 关闭除了当前session以外的其他session
tmux kill-session -a -t basic --> 关闭除了名为basic的session以外的其他session
<prefix> $ --> 重命名当前session
<prefix> d --> detach当前session
tmux attach --> 切换到最近使用的上一个session
tmux attach -t basic --> 切换到名为basic的session
<prefix> ( --> 切换到上一个session
<prefix> ) --> 切换到下一个session
```

# **Windows**
```
tmux new -s basic -n mywindow --> 创建一个新的session，其默认window命名为mywindow
<prefix> c --> 创建一个新的window
<prefix> w --> 显示所有的window
<prefix> , --> 重命名当前window
<prefix> Tab --> 切换到最近使用的上一个window(自定义)
<prefix> & --> 关闭当前window
<prefix> C-h --> 切换到上一个window(自定义)
<prefix> C-l --> 切换到下一个window(自定义)
<prefix> [0..9] --> 切换到0到9某个window
```

# **Panes**
```
<prefix> ; --> 切换到最近使用的pane
<prefix> | --> 水平方向分割当前window(自定义)
<prefix> - --> 垂直方向分割当前window(自定义)
<prefix> h, j, k, l --> 以vim风格在panes间切换(自定义)
<prefix> q --> 显示各个pane的序号
<prefix> q [0..9] --> 通过序号选中pane
<prefix> z --> 全屏当前pane
<prefix> ! --> 将当前pane变成window
<prefix> H, J, K, L --> 改变当前pane的大小(自定义)
<prefix> Space --> 改变window中的pane的布局方式
<prefix> o --> 切换到下一个pane
<prefix> x --> 关闭当前pane
```

# **Others**
```
<prefix> : --> 进入tmux命令模式
<prefix> ? --> 显示tmux快捷键绑定
<prefix> e --> 打开~/.tmux.conf.local，修改配置(自定义)
<prefix> r --> 加载配置文件，刷新配置(自动义)
<prefix> m --> 启动和关闭鼠标(自定义)
```
