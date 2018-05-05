---
layout: post
title: 系统内核更新
catagories: tutorial
---

一般情况下，系统内核并不需要更新，但是有些服务可能会依赖内核版本，当内核版本过低时，无法正常运行。例如Nvidia的cuda-9.0在4.4内核下就无法正常工作，驱动无法被内核加载。

# 内核下载

在[Ubuntu官方内核目录](http://kernel.ubuntu.com/~kernel-ppa/mainline/)下载所需要的内核，例如使用4.14版本的内核，可以到[该目录](http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.14/)中下载如下文件:
> 
linux-headers-4.14.0-041400_4.14.0-041400.201711122031_all.deb  
linux-headers-4.14.0-041400-generic_4.14.0-041400.201711122031_amd64.deb  
linux-image-4.14.0-041400-generic_4.14.0-041400.201711122031_amd64.deb

# 内核安装

进入到内核更新文件的文件目录中，使用**dpkg**指令安装内核: **`sudo dpkg -i *deb`**

# 查看nvidia驱动状态

```
lsmod | grep nvidia --> 查看nvidia驱动是否正常加载
cat /proc/driver/nvidia/version --> 查看nvidia驱动版本
```
