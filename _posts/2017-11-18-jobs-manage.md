---
layout: post
title: Linux作业管理
catagories: tutorial
---

# **nohup**

**nohup**命令可以将程序以忽略挂起信号的方式运行，被运行的程序的输出信息将不会显示到终端。输出将附加到当前目录的nohup.out文件中，如果当前目录的nohup.out文件不可写，输出重定向到$HOME/nohup.out文件中。

```
nohup command > result &  --> 将command命令的输出重定向到result文件，而且后台执行
```

# **jobs**

在shell终端中执行指令时，默认总是以前台(foreground)方式运行，即当前指令抢占shell终端直到其退出或终止。很多时候前台运行指令会妨碍使用shell终端执行其他操作，可以按下**Ctrl-z**将当前正在前台执行的程序挂起（暂停并交出shell终端控制权）。使用**jobs**指令可以查看当前终端中有哪些任务，使用**fg**, **bg**命令可以对作业进行前台后台切换。

```
command --> 将command命令以前台方式执行
command & --> 将command命令以后台方式执行
Ctrl-z --> 将当前正在执行的前台命令挂起
jobs --> 查看当前shell终端中有哪些作业
fg %n --> 将作业号为n的作业转换到前台运行
bg %n --> 将作业号为n的作业转换到后台运行
```

job依赖于终端存在，如果不使用**nohup**指令，关闭终端时其中的job就会终止。即使使用了**nohup**指令，再次登录时也会失去作业信息，只能查看进程号关闭进程。可以使用**tmux**使终端持久化，以解决这个问题。

# **终止job**

**jobs**指令不提供终止作业的选项，可以通过杀死进程来终止作业。

```
ps -A | grep cmd --> 查找名称包含cmd关键字的进程
pgrep cmd --> 查看名称为cmd的进程的进程号
pstree --> 以树状方式输出当前进程间的派生关系
sudo killall cmd --> 杀死所有名称为cmd的进程
sudo pkill cmd --> 杀死某个名称为cmd的进程
sudo kill -9 ps_num --> 杀死进程号为ps_num的进程
```
