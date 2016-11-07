---
layout: post
title: "Ubuntu snapd 升级安装卡死"
categories: resource
---

在Ubuntu系统下，软件更新时很容易发生snapd升级卡死的问题，可以通过以下方式解决：

1. `E: Could not get lock /var/cache ...`: 遇到此问题一般是更新过程中强制中断引起的，运行指令`sudo rm -rf /var/cache/apt/archievs/lock`，然后`sudo apt-get update`

2. `E: Unable to lock the administration directory (/var/lib/dpkg), is another process using it?`是由于升级中断造成的，运行指令`sudo rm /var/lib/dpkg/lock`

3. 在更新snapd之前，先输入该指令：`su`, `echo "bash -c 'service snapd.boot-ok start'" | at now + 4 min`,`apt install snapd`, 若提示发生异常，运行`dpkg --configure -a`。然后等待4分钟即可，在安装过程中命令行中有进度提示条。
