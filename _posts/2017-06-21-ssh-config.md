---
layout: post
title: "SSH配置"
categories: tutorial
---

# **ssh免密码登录**
1. 生成密钥对: `ssh-keygen`
2. 将生成的密钥对放在本地的`~/.ssh`目录下
3. 在本地`~/.ssh/config`中配置远程主机的IP地址和对应的私钥
4. 将生成密钥的公钥，如`id_rsa.pub`上传到服务器的上。并将其加入到服务器中对应用户的`~/.ssh/authorized_keys`中，可使用指令`cat id_rsa.pub > ~/.ssh/authorized_keys`

# **scp免密码**
1. 在本地`~/.ssh/config`中配置远程主机的IP地址和对应的私钥
2. 与使用ssh相同，将公钥上传到服务器中，并进行相关配置

# **sftp免密码**
1. 在服务器上安装openssh-sftp-server: `sudo apt-get install openssh-sftp-server`
2. 通过以上的方式配置密钥对
3. 在命令行中可以直接使用sftp命令连接服务器，但是推荐使用fileZilla
4. 在fileZilla中`Edit --> settings --> SFTP --> Add key file`添加私钥，然后选择Interactive方式登录SFTP服务器即可

# **其他**
以上服务均基于SSH协议，这些服务可以同时使用相同的密钥对。网上有sftp的安装配置文章，但是十分复杂麻烦，如果没有特殊需求，不修改任何配置是最好的方式。
