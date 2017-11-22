---
layout: post
title: "Linux字符集设置"
categories: tutorial
---

# **locale变量**

```
LC_COLLATE --> 定义环境的排序和比较规则
LC_CTYPE --> 用户使用的语言符号及其分类
LC_MONETARY --> 货币数字的显示格式
LC_MESSAGES --> 提示信息，错误信息，状态信息等的显示格式
LC_NUMERIC --> 非货币数字的显示格式
LC_TIME --> 时间的显示格式
LC_NAME --> 姓名的显示格式
LC_ADDRESS --> 地址的显示格式
LC_TELEPHONE --> 电话号码的显示格式
LC_MEASUREMENT --> 度量衡的显示格式

LANG --> 默认值，如果LC_*没有设置，使用该值
LC_ALL --> 如果设置了，覆盖所有LC_*的设置值
```

# **设置locale**

```
sudo locale-gen en_US.UTF-8 --> 生成en_US.UTF-8字符集
sudo dpkg-reconfigure locales --> 重新选择安装的字符集
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 --> 更新locale变量
```

除了可以通过以上命令更新字locale变量设置，还可以手动设置locale配置文件。

```
/etc/locale.conf --> 系统locale设置
/etc/default/locale --> locale变量设置
~/.config/locale.conf --> 用户locale设置
```
