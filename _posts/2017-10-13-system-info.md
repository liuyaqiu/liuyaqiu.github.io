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
sudo blkid /dev/sda --> 输出磁盘/dev/sda的uuid
ls -l /dev/disk/by-uuid --> 输出系统所有磁盘的uuid
lsblk -f --> 输出系统磁盘的详细信息，包括uuid
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

# **GPU信息**

```
lspci -vnn | grep VGA --> 查看当前VGA设备，有的显卡可能不会显示出来
lshw -C display --> 查看显卡设备，及其相关信息
nvidia-smi --> 输出Nvidia GPU的设备运行状态信息，可以使用watch命令获取更新的状态信息
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

本文中的各种命令的详细用法，可以参见[man7.org](http://man7.org/linux/man-pages/index.html), 或者[Linux命令大全](http://man.linuxde.net/)。
