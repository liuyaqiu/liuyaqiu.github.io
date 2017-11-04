---
layout: post
title:  "Ubuntu系统配置"
categories: ubuntu 
---

# **制作安装启动盘**

Ubuntu下可以非常容易地制作U盘启动盘: `sudo dd bs=4M if=/path/to/iso of=/dev/sdX && sync`

# **修改ubuntu更新源**
将ubuntu的更新源设定为美国，更新速度更快。使用[repogen]自动生成ubuntu的源更新列表，访问其网站，选择地区，按照其指令执行即可。

[repogen]: https://repogen.simplylinux.ch

# **取消root密码**

修改**/etc/sudoers**文件取消sudo密码验证

1. 一般的，**/etc/sudoers**文件的权限为440，任何用户都不能修改它。首先应当使用
    root账户身份来登录，然后改变权限：
    `chmod 777 sudoers`

2. 然后修改sudoers：
    假设当前用户为user，可以在该文件末尾添加如下命令来取消user的sudo密码
    `user ALL=(ALL) NOPASSWD:ALL`

3. 修改sudoers文件后，应当将其权限恢复为初始状态：
    `chmod 440 sudoers`

# **安装Source Code Pro字体**
[Source Code Pro]是Adobe公司出品的优秀的免费开源等宽字体

[Source Code Pro]: https://github.com/adobe-fonts/source-code-pro
1. 从github上下载字体文件（使用浏览器或者wget工具都可以）

2. 安装单个字体，直接双击打开导入即可

3. 安装多个字体，可以在**/usr/share/fonts**下创建文件夹**custom/**

    `chmod 755 custom`  
    将下载好的字体文件[xx].ttf全部复制到**custom/**文件夹中即可
    运行`sudo fc-cache -f -v`刷新字体缓存

# **禁用触摸板**
1. 使用xinput命令查看当前设备：  
    `xinput list`  
    我的触摸板在其中显示的设备标识为UNKNOWN，id=11
    
2. 查看触摸板的设备参数：  
    `xinput list-props 11` #11为设备id  
    在显示的信息的第一行可以看到设备允许状态为：  
    `Device Enabled (170):  1`  #设备默认情况下允许位均为1,表示当前该设备可用  
    禁用触摸板：  
    `xinput set-prop 11 170 0`  
    启用触摸板：  
    `xinput set-prop 11 170 1`  
    可以在**.bashrc**中设定如下自定义命令：  
    `alias tpOff='xinput set-prop 11 170 0'`  
    `alias tpOn='xinput set-prop 11 170 1'`  
    那么只要在当前用户的terminal中输入tpOff和tpOn就可以禁用或者启用触摸板

# **屏幕默认亮度**
Ubuntu开机启动后，其默认亮度始终为最大亮度，这很不方便，修改如下在**/etc/rc.local**中添加如下命令:
`echo 250 > /sys/class/backlight/intel_backlight/brightness`其中250为屏幕合适的亮度值。

# **安装Java**
1. 添加Java安装源：  
    `sudo add-apt-repository ppa:webupd8team/java`  
    `sudo apt-get update`  

2. 安装java  
    jdk7:
    `sudo apt-get install oracle-java7-installer`  
    jdk8:
    `sudo apt-get install oracle-java8-installer`  

3. 设置默认jdk  
    jdk7:
    `sudo update-java-alternatives -s java-7-oracle`  
    jdk8:
    `sudo update-java-alternatives -s java-8-oracle`  
    使用以上两条命令，可以切换jdk的默认选项

4. 安装成功后，可以查看java版本号确认是否安装成功
    `java -version`

# **使用i3wm**

i3wm是轻量级的窗口管理器，比重量级的桌面环境更加稳定易用。

```
sudo apt install i3
sudo apt install i3blocks
```

在**~/.config/i3/config**中写入i3wm的配置即可，状态栏的配置文件是**/etc/i3blocks.conf**。

在i3wm中打开***nautilus***，默认会打开桌面，引起bug，可以通过禁用桌面来解决这个问题:

```
gsettings set org.gnome.desktop.background show-desktop-icons false
```

# **安装Mesa驱动**

***Mesa***是Linux下的开源3D驱动，非常稳定。一般情况下，无特殊需求一定不要安装***Nvidia***的驱动，否则驱动更新或者删除后，几乎一定会引起桌面环境崩溃。
***Mesa***驱动更新较快，推荐从其[官网](https://www.mesa3d.org/download.html)最新的版本编译安装。

```
sudo apt-get build-dep mesa --> 安装mesa的依赖库
sudo apt-get install mesa-utils
./configure --prefix=/path/to/install/mesa --enable-llvm --> 在目标目录安装mesa
make && sudo make install
```

在执行**configure**指令时，可能发生报错**llvm-config not found**，而**llvm**已经被安装了，实际上**llvm-config**也存在:

```
$ locate llvm-config
/usr/bin/llvm-config-4.0
/usr/include/llvm-4.0/llvm/Config/llvm-config.h
/usr/lib/llvm-4.0/bin/llvm-config
/usr/share/man/man1/llvm-config-4.0.1.gz
```

其实**llvm-config-4.0**即为所需要的文件，但是文件名不对，导致了错误发生，可以通过创建**Symbol Link**来解决这个问题。

```
sudo ln -s /usr/bin/llvm-config-4.0 /usr/bin/llvm-config
```

VMware使用3D加速，需要在虚拟机配置文件**???.vmx**中修改对应条目: `mks.gl.allowBlacklistedDrivers = "TRUE"`

## **安装spyder3**

spyder是非常强大、非常方便的开源Python IDE。推荐从**pip**安装***spyder3***，首先安装[pip](https://pip.pypa.io/en/stable/installing/)。

```
python get-pip.py --> 安装python2版本的pip
python3 get-pip.py --> 安装python3版本的pip
sudo apt-get install python3-dev python3-pyside
pip3 install --user -U pyqt5
pip3 install --user -U spyder
```

## **配置alsamixer**

***alsamixer***是命令行下的音频输出管理工具，非常适合在***i3wm***下使用。

```
$ cat /proc/asound/cards 
  0 [HDMI           ]: HDA-Intel - HDA Intel HDMI
                       HDA Intel HDMI at 0xd1610000 irq 35
  1 [PCH            ]: HDA-Intel - HDA Intel PCH
                       HDA Intel PCH at 0xd1614000 irq 34
```

外设耳机的音频输出是**PCH**，将以下内容写入配置文件**/etc/asound.conf**即可:

```
pcm.!default {
            type hw
            card 1
}
ctl.!default {
            type hw
            card 1
}
```

## **stow**

***stow***可以对文件进行**Symbol Link**管理，**/usr/local/stow**中安装各个软件。使用mesa后，不必手动向**PATH**中添加路径变量，删除软件也十分方便，解除符号映射后直接删除对应文件目录即可。
```
$ ls /usr/local/stow
hugo  mesa  proxychains  tmux
sudo stow mesa --> 对mesa进行符号映射
sudo stow -D mesa --> 删除mesa的符号映射
```

还可以**home**目录下常见的配置文件**.dotfile**进行集中管理，把他们都放到**.dotfiles**目录中

```
$ ls .dotfiles 
spacemacs  vim  zsh
```
