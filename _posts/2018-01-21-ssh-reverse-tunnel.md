---
layout: post
title: ssh反向隧道
catagories: tutorial
---

一般内网主机没有公网IP，因此在外网无法通过IP连接登录内网主机。为解决此问题，可以使用ssh反向隧道。设公网拥有固定IP的主机为A（可以使用VPS作为公网主机），设内网主机为B, 设需要访问主机B的主机为C。

# **配置主机A**

配置主机A的***/etc/ssh/sshd_config***，添加项**`GatewayPorts yes`**，然后重启sshd服务，**`sudo systemctl restart sshd`**。

如果使用公钥验证，需要把主机B的公钥***~/.ssh/id_rsa.pub***传送到主机A上的***~/.ssh***目录下。

# **在主机B上安装autossh并设置开机运行**

**`sudo apt-get install autossh`**

设置autossh的开机启动项:

```
cat > /etc/systemd/system/autossh.service
[Unit]
Description=Auto SSH Tunnel
After=network-online.target
[Service]
User=userb
Group=userb
Type=simple
ExecStart=/usr/bin/autossh -p 22 -M 6777 -NR 6766:localhost:22 usera@ip_hostA -i /home/usera/.ssh/id_rsa
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=always
[Install]
WantedBy=multi-user.target
WantedBy=graphical.target
```

注意此处usera为A上对应的用户, 6766为主机C进行ssh连接时的访问端口。

然后运行**`sudo chmod 755 /etc/systemd/system/autossh.service`**修正权限。**`sudo systemctl enable NetworkManager-wait-online`**，使得network-online.target生效。

**`sudo systemctl enable autossh`**, 使得autossh启动项配置生效，然后执行**`sudo systemctl start autossh`**启动autossh。**`service autossh status`**即可查看autossh运行状态，running状态为正常。

# **配置主机C**

在主机C的***~/.ssh/config***目录中写入相关配置。

```
Host reverse
User userb # 主机B上的访问目标账户
Hostname ip_hostA # 主机A的ip
IdentityFile ~/.ssh/id_rsa # 访问主机B上userb用户的私钥
Port port # 主机A上的代理端口，例如本例子中为6766
DynamicForward local_proxy_port # 设置本地代理，浏览器中可以通过socks4的localhost代理，访问主机B所在的内网
```

运行**`ssh reverse`**即可访问主机B上的目标账户userb。
