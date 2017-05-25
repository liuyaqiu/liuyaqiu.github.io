---
layout: post
title: "Emacs中文输入"
categories: tutorial
---

在Ubuntu中使用fcitx，搜狗输入法输入中文，默认情况下无法在emacs中使用，需要进行配置。

# **检查字符集**
`locale -a`，查看当前系统支持的字符集，如果包含`zh_CN.utf8`，则可以直接进行配置，否则安装中文字符集。

`sudo dpkg-reconfigure locales`，选择`zh_CN.utf8`即可安装中文字符集。

# **终端配置**
如果在终端中启动，需要设置环境变量，在`.bashrc`中添加`export LC_CTYPE=zh_CN.UTF-8`

# **GUI配置**
如果以GUI方式启动，需要修改`/usr/bin/emacs24`文件。

删除原有的`/usr/bin/emacs24`文件，在新建的文件中写入以下内容:

`LC_CTYPE=zh_CN.UTF-8 /usr/bin/emacs24-x`

注意，Ubuntu中fcitx与搜狗输入法有冲突，在使用shift键切换输入法时，需要长按shift键。
