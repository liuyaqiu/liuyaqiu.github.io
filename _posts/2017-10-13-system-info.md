---
layout: post
title: "Linux查看系统信息"
categories: tutorial
---

在使用Linux时，我们常常需要查看主机的一些状态信息，以确定主机运行状态，发现可能存在的问题以及原因，从而排除问题。

# **操作系统信息**

```
uname -a --> 操作系统的详细信息
uname -r --> 操作系统的发现编号
uname -v --> 操作系统的版本
uname -m, p, i --> 操作系统的机器类型，处理器类型，硬件类型（64位机器显示x86_64)
```

# **硬盘信息**

```
df -h --> 以k,m,g为单位显示系统的磁盘使用状态
fdisk -l --> 当前系统连接的各个硬盘的信息
lsblk --> 当前系统的磁盘挂载状态
du -h -d 1 dir --> 查看dir目录的文件容量
tree -L 2 --> 显示当前目录的树形结构
```

# **CPU信息**

```
grep 'physical id' /proc/cpuinfo | sort -u | wc -l --> 物理CPU个数
grep 'core id' /proc/cpuinfo | sort -u | wc -l --> CPU核心个数
grep 'processor' /proc/cpuinfo | sort -u | wc -l --> CPU线程数
dmidecode -t processor --> CPU的详细硬件信息
```

# **内存信息**

```
free --> 系统当前的内存使用量和剩余量
dmidecode -t memory --> 内存的详细硬件信息
cat /proc/meminfo --> 系统的内存信息
```

# **进程信息**

```
ps -A --> 系统的所有进程信息
top --> 动态显示系统的进程信息
```

# **网络信息**

```
ifconfig --> 系统的网络接口信息
netstat --> 系统的网络端口状态信息
```
