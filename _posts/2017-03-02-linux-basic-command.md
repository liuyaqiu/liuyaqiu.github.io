---
layout: post
titile: Linux Basic Command
categories: tutorial
---

## 一些简单的shell命令
```
uname -> 输出系统信息
date -> 输出当前日期信息
cal -> 输出本月日历
free -> 输出当前内存使用情况
df -> 输出当前磁盘使用情况
exit -> 退出当前shell
```

## man命令
Linux下的大部分命令都有详细的文档，阅读文档我们可以获得比较详细的指导。在命令行中一般使用man命令调用文档。

man文档分为8类，如果不指定查看哪一类文档，那么man命令返回找到的第一个文档。如果要指定使用哪个类别的文档，通过指定文档类型即可。

`man 5 passwd`，即查看passwd的第5类文档。

man文档类型(常用的是1, 5, 7, 8):
```
1 -> User commands
2 -> System calls
3 -> High-level Unix programming library documentation
4 -> device interface and driver information
5 -> File descriptions(system configuration files)
6 -> Games
7 -> File formats, conventions, and encodings(ASCII, suffixes and so on)
8 -> System commands and servers
```

## 目录与文件操作命令
Linux的目录结构
```
/ -> 根目录
/bin -> 包含二进制文件，一般是系统启动或运行所需要的可执行文件
/boot -> 包含Linux的内核，启动RAM镜像，以及boot loader
/boot/vmlinuz -> 存放Linux内核
/dev -> 系统连接的外设硬件被映射为文件，存放在这里
/etc -> 系统层次的配置文件，/etc/fstab是磁盘以及挂载点配置信息
/home -> 存放每个用户的各自的所有文件
/lib -> 存放系统程序所需要的所有库文件，类似于windows中的dll文件目录
/media -> 可移除设备的目录，与/dev相似，但是用于映射USB存储器、CD-ROM等可移除设备
/mnt -> 用户手动挂载可移除设备的目录，可用来挂载外接硬盘
/opt -> 存放系统上安装的商业软件
/proc -> 由Linux内核维护管理的虚拟文件系统
/root -> root用户的home目录
/sbin -> 存放系统所需的二进制文件，这些文件是系统任务所需的关键性程序
/tmp -> 临时存储目录，存放临时性文件，可以配置为每次系统重启后情况
/usr -> 用户目录，用来存放一般用户所需的相关程序
/usr/bin -> Linux发行版存放可执行程序，该目录下的文件可以在命令行中直接通过文件名启动
/usr/local -> 存放一般用户自行安装的、非Linux发行版安装的程序
/usr/sbin -> 包含系统管理程序
/usr/share -> 包含被/usr/bin共享的数据，包括配置文件、图标、声音文件等等
/usr/share/doc -> 存放程序文档
/var -> 存放系统中的动态变化文件，如数据库、用户邮件等等
/var/log -> 存放系统活动日志消息
```
Linux奉行Unix的`一切皆文件`的哲学，所以进行目录和文件操作是Linux下最基本的需求。

```
pwd -> 输出当前目录的绝对路径
ls [-op] file/dir [file/dir...] -> 输出目录下的文件
file [-op] file/dir [file/dir...] -> 输出文件的类型信息
cp file1 file2 -> 复制file1到file2，如果file2不存在，则创建，如果已经存在，替换之
cp file dir -> 复制file到目标目录dir中
mv file1 file2 -> 移动file1到file2，如果file2,存在，替换之
mv file dir -> 将file移动到目标目录dir中
mkdir dir -> 创建目录dir
rm file -> 删除文件file
ln -s target linkname -> 创建符号链接，链接名为linkname，指向target(文件或目录)
```

Linux命令行中常常使用通配符以便捷地进行批量处理.注意通配符只是命令行解析，在命令执行以前就进行了解析处理，与正则表达式不同
```
* -> 任意个字符
? -> 一个任意字符
[characters] -> characters中的任意一个，[abc]表示a，或b，或c
[!characters] -> 不是characters中的任意一个
[:alnum:] -> 任何英文字符或者数字
[:alpha:] -> 任何英文字符
[:digit:] -> 任何数字
[:lower:] -> 任何小写英文字符
[:upper:] -> 任何大写英文字符
```
例如使用通配符，`cp dir1/* dir2`表示将dir1目录下的所有文件复制到目录dir2下

## 命令解析
```
type cmd -> 输出命令cmd的解析信息，例如type mkdir输出/bin/mkdir，表示mkdir命令事实上调用/bin/mkdir程序
which cmd -> 输出命令实际上执行的可执行程序，与type类似，但是输出内容形式有所区别
whatis cmd -> 输出一条指令的简要描述信息
whereis cmd -> 搜索一条命令对应的可执行文件、man文件和源代码文件
alias name='your command' -> 将一条命令命名为name，常常写在shell配置文件中，为长命令取较短的别名
```

## IO与重定向
```
cat [file ...] -> 将文件file中的内容输出到stdout中
cat [file1 ...] > file2 ->将文件file1中的内容重定向到file2中
cat -> 从stdin读取内容，然后输出到stdout中
cat > file 从stdin读取内容，然后重定向到file中
cat < file -> 将文件file的内容重定向到stdin中，然后输入到cat，最后输出到stdout

command1 | command2 -> 将command1的输出传递给command2作为输入

sort -> 对输入流做排序
uniq -> 删除输入流中连续的重复内容（以行为单位）
wc -> 统计输入流的行数，词数和字节数
grep pattern [file ...] 输出文件file中符合pattern模式的行
head -> 输出输入流的最前面部分，默认为10行
tail -> 输出输入流的最后面部分，默认为10行
tee -> 将输入流同时输出到文件和stdout
```

## Shell expansion
```
$((arithmetic expression)) -> 计算算术表达式, % 表示取余，** 表示乘方
{A, B, C} -> 依次取集合中的元素, echo exp_{A, B, C}输出：exp_A, exp_B, exp_C, {}可以嵌套
$variable -> 输出变量的值，例如USER的值为liuyaqiu, 那么echo $USER 输出liuyaqiu
$(command) -> 将command命令的输出值转换为字符串
echo "expression" -> 输出expression，解析expression中的算式或者变量
echo 'expression' -> 输出expression，但是不解析expression中的算式和变量
\ -> 转义字符，可以输出特殊字符，例如echo \$ 将会输出$
```

## 键盘指令
在shell命令行中，如果使用键盘操作移动光标，将会十分高效.以C代表Ctrl键,<C + x>表示Ctrl + x的组合键,<A + x> 表示Alt + x的组合键
```
<C + b> -> 光标向左移动
<C + f> -> 光标向右移动
<C + p> -> 历史记录中上一条指令.在zsh目录选择中，表示向上移动.
<C + n> -> 历史记录中下一条指令.在zsh目录选择中，表示向下移动.
<C + a> -> 光标移动到行首
<C + e> -> 光标移动到行尾
<C + l> -> 清空命令行
<A + f> -> 向前移动一个单词
<A + b> -> 向后移动一个单词
<C + d> -> 删除光标位置处的字符
<C + w> -> 删除光标位置的前一个单词.删除内容复制到剪切板.
<C + u> -> 删除光标位置到行首的内容.删除内容复制到剪切板.
<C + k> -> 删除光标位置到行尾的内容.删除内容复制到剪切板.
<C + y> -> 将命令行剪切板的内容复制到光标当前位置.
<C + t> -> 交换光标位置和光标前的字符
<A + t> -> 交换光标位置和光标后的字符
<A + l> -> 将光标位置到单词尾部的所有字符转换为小写
<A + u> -> 将光标位置到单词尾部的所有字符转换为大写
history -> 输出曾经使用过的历史指令
```

## 权限
```
id -> 输出用户的账户信息(用户名，群组，权限等等)
su USER -> 以USER用户身份运行shell，如果不指定USER，以root运行
sudo command -> 以root身份运行command命令
umask -> 设置文件的默认权限
chmod -> 改变文件的权限
chown -> 改变文件的拥有者
chgrp -> 改变文件的所属群组
passwd -> 改变账户密码
```

## 进程
```
ps -> 输出当前运行的进程
top -> 显示当前真正运行的进程
jobs -> 显示当前活动的job
bg -> 将一个job置于后台
fg -> 将一个job置于前台
kill process_id -> 以进程id为标识杀死进程
killall process_name -> 以名称为标识杀死进程
shutdown -> 关闭计算机
```

本文辑录了所有常见的Linux命令，参考书籍 ***The Linux Command Line***。关于各条命令的详细使用方法，可以参考该书，或者查看命令的文档。
