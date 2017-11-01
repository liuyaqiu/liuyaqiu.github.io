---
layout: post
title: Ubuntu包管理
catagories: tutorial
---

# apt

```
apt list [--option]--> 列出系统安装的包
apt show package --> 列出package包的详细信息
apt update --> 更新包列表信息
apt upgrade --> 升级可更新的包
apt install package --> 安装package包
apt remove package --> 移除package包
apt autoremove package --> 移除package及其关联包
```

# apt-key

```
apt-key list --> 列出apt-key列表
sudo apt-key del key --> 删除key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys the_publickey
```

# source.list

```
/etc/apt/source.list --> 官方源列表
/etc/apt/source.list.d --> 第三方ppa repository
sudo add-apt-repository ppa:<ppa to install> --> 添加ppa repo
sudo add-apt-repository -r ppa:<ppa to remove> --> 删除ppa repo
```
当通过命令删除ppa不能生效时，可以手动删除`/etc/apt/source.list.d`中对应的文件。

# 禁止自动更新
有时候我们希望禁止某个包自动更新，以防止发生不必要的意外情况。

## dpkg

```
Put a package on hold:
echo "<package-name> hold" | sudo dpkg --set-selections

Remove the hold:
echo "<package-name> install" | sudo dpkg --set-selections

Display the status of your packages:
dpkg --get-selections

Display the status of a single package:
dpkg --get-selections | grep "<package-name>"
```

## apt

```
Hold a package:
sudo apt-mark hold <package-name>

Remove the hold:
sudo apt-mark unhold <package-name>
```

## aptitude

```
Hold a package:
sudo aptitude hold <package-name>

Remove the hold:
sudo aptitude unhold <package-name>
```

# 其他包管理工具
Ubuntu除了可以使用默认的**apt**包管理工具，还可以使用**dpkg**以及**synaptic**。

## dpkg
```
dpkg -l --> 输出已经安装的包列表
sudo dpkg -i package.deb --> 安装package包
sudo dpkg -r package --> 移除package包
```

## synpatic
**synpatic**是第三方的图形化包管理工具，使用root身份运行即可。

# 更多命令
关于**apt**, **apt-key**, **apt-mark**, **dpkg**的更多其他命令，可以通过**man**命令查看其详细文档。
