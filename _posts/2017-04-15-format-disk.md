---
layout: post
titile: Linux Format Disk Command
categories: tutorial
---

# **lsblk**
lsblk可以显示当前系统挂载了哪些硬盘

# **fdisk**
`sudo fdisk -l`: 显示当前挂载的所有硬盘的详细信息

# **mount**
`sudo mount dev dir`: 将设备dev挂载到dir目录

`sudo umount dev|dir`: 将设备dev或者dir目录解挂载

# **gdisk**
GPT fdisk: 支持GPT分区表的fdisk

# **parted**
比fdisk更加强大的替代工具

# **gparted**
parted的图形界面版

# **mkfs**
创建文件系统, `mkfs.ext4`创建ext4文件系统, `mkfs.ntfs`创建NTFS文件系统

关于硬盘分区, 需要考虑对齐和主分区逻辑分区的问题. 关于这些内容, 应该参考专门的资料.
