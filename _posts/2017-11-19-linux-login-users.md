---
layout: post
title: Linux查看登录用户
catagories: tutorial
---

# 查看登录用户

```
w --> 查看当前登录用户的行为
who --> 查看当前有哪些用户登录
last --> 查看用户登录历史
```

# 踢出当前用户

```
ps -ef | grep pts/0 --> 查看pts/0终端的登录用户的进程
sudo kill -9 pid --> 杀死pid进程
```
