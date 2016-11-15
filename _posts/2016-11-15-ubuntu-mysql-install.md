---
layout: post
title: "Ubuntu MySQL Install"
categories: tutorial
---

在Ubuntu中安装MySQL，直接使用Ubuntu的内置源安装即可。

`sudo apt-get install mysql-server`

`sudo apt-get install mysql-client`

`sudo apt-get install libmysqlclient-dev`

安装过程中会提示设置密码，记下该密码，该密码用于登录mysql的root服务器。

检查是否安装成功：`sudo netstat -tap | grep mysql`，如果看到mysql的socket处于listen状态，说明MySQL正在运行，然后执行`mysql -u root -p`命令登录mysql的root账户，提示`Welcome to the MySQL monitor.`并显示Server Version信息，即表示安装成功。
