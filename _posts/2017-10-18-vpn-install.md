---
layout: post
title: "Ubuntu安装和配置openvpn与shadowsocks"
categories: tutorial
---

# **创建VPS**

安装openvpn或者shadowsocks用于***Fuck the GFW***,首先需要申请一个外国的私有服务器(Virtual Private Server), 常见的性价比比较高的选择有:

* [DigitalOcean](https://m.do.co/c/da113bb1ae16)
* [Vultr](https://www.vultr.com/?ref=6837046)
* [Linode](https://www.linode.com/?r=f9a3e44c3b09957f89668ba7137ae34e5ea54c5e)

DigitalOcean虽然有额定流量，但是似乎并不会因为流量超限而额外收费，如果是有比较大的流量需求，建议选择DigitalOcean。最便宜的5刀价位上，DO家的机器内存最小，只有512M，Vultr和Linode均为1G。如果想要在VPS上搭建其他服务，如博客等，那么Vultr和Linode的性价比更高。在文档支持方面，DO有比较强大的社区，有很多用户撰写高质量的文档，Vultr的文档通常比较简略，Linode的文档更新不够活跃。

# **配置VPS**

VPS推荐使用Ubuntu最新LTS版本，Ubuntu用户多，社区活跃，遇到问题，更容易找到支持和帮助。创建Ubuntu VPS后，可以参考[这篇文章](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04)做最基本的安全配置。如果没有特殊需求，一般情况下可以忽略防火墙配置。创建VPS后，推荐使用zsh代替原生的bash，并使用[oh-my-zsh](http://ohmyz.sh/)主题框架，可以减轻使用命令行的痛苦。

# **安装配置shadowsocks**

## 服务器端安装

```
sudo apt-get install python-pip
sudo pip install shadowsocks
cat > /etc/shadowsocks.json
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

可以参考[官方文档](https://github.com/liuyaqiu/shadowsocks/wiki)修改各个配置项。如果需要让shadowsocks同时监听ipv4和ipv6流量，配置`"server":"::"`。

```
sudo ssserver -c /etc/shadowsocks.json -d start --> 启动shadowsocks服务端
sudo ssserver -c /etc/shadowsocks.json -d stop --> 终止shadowsocks服务端
```

## 客户端安装

在客户端安装shadowsocks的步骤与服务器端相同，配置文件中，***server***项填写为服务器ip地址即可，ipv4或者ipv6均可，*server_port*,*password*,*method*等项需要与服务器配置相同。

```
sudo sslocal -c /path/to/shadowsocks.json -d start --> 启动shadowsocks客户端
sudo sslocal -c /path/to/shadowsocks.json -d stop --> 终止shadowsocks客户端
```

## 客户端代理配置

在客户端安装shadowsocks后，需要配置本地代理服务。

### 浏览器配置

对于Chrome浏览器，安装[SwitchyOmega](https://github.com/FelisCatus/SwitchyOmega)插件。设置本机流量和局域网流量不使用代理。

```
Protocol: socks5
Server: 127.0.0.1
Port: 1080

Bypass List:
127.0.0.1
::1
localhost
10.*
```

### 命令行配置

对于terminal，安装[proxychains](https://github.com/rofl0r/proxychains-ng)进行代理。参考官方文档安装后，`echo "socks5 127.0.0.1 1080" >> /etc/proxychains.conf`即可让proxychains使用shadowsocks进行代理。在需要使用代理的命令前添加proxychains4命令即可，例如`sudo proxychains4 apt-get update`。注意proxychains命令不可代理ping指令，但是可以代理curl指令，可以通过`proxychains4 curl www.google.com`来测试代理网络的连通性。

我们可以在客户端的`.zshrc`中写入`alias pc='proxychains4`来简化指令，但是当使用sudo命令时，仍然需要使用命令全称。

### 移动端配置

在Android移动端，可以通过[shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android)来使用shadowsocks，配置方法同上。新版本中加入了广告和不必要的新功能，推荐使用老版本[2.8.3](https://github.com/shadowsocks/shadowsocks-android/releases/tag/v2.8.3)。

# openvpn安装配置

openvpn安装配置较为复杂，配置项较多，稍有错误openvpn就可能无法正常工作。推荐使用[openvpn一键安装脚本](https://github.com/Nyr/openvpn-install)简化安装。

```
sudo systemctl enable openvpn@server --> 设置openvpn为开机自启动
sudo systemctl openvpn@server start --> 启动openvpn服务端
sudo systemctl openvpn@server sotp --> 终止openvpn服务端
```

安装完成后，脚本会把服务端配置文件写入到`/etc/openvpn/`目录下，并且在当前目录下生成`client.ovpn`配置文件，用户客户端配置。将`client.ovpn`使用ftp或者sftp复制到客户端机器的`/etc/openvpn/`目录下即可。

注意，如果需要使用让openvpn支持ipv6通道，应该在openvpn的服务端配置文件`/etc/openvpn/server.conf`和客户端配置文件`/etc/openvpn/client.ovpn`中配置`proto udp6`。默认情况下，客户端的所有流量均走vpn通道，我们可以在客户端配置文件中设置`route 10.0.0.0 255.0.0.0 net_gateway`，让局域网流量不走vpn通道。

```
sudo openvpn /etc/openvpn/client.ovpn > /dev/null & --> 运行openvpn客户端
```

在Android移动端可以使用[OpenVPN for Android](https://github.com/schwabe/ics-openvpn)来连接openvpn。配置方法是把`client.ovpn`复制到手机中，在手机中点击该文件，选择***导入OpenVPN配置***。
