---
layout: post
title:  "Ubuntu系统配置"
categories: ubuntu 
---

Ubuntu系统的初始配置非常难用，一般我们需要做出一些更改，才更加好用。下面列出我的系统的一些配置，以便以后查找。

# **修改ubuntu更新源**
我一般直接使用openvpn连接外网，所以ubuntu的更新源设定为美国，更新速度更快，这样可以使得像Android Studio，Chrome等软件能够无缝更新。

使用[repogen]自动生成ubuntu的源更新列表，访问其网站，选择地区，按照其指令执行即可。

[repogen]: https://repogen.simplylinux.ch

# **取消root密码**
在linux中我们一般不使用root账户，然而许多操作需要使用到root权限，这种情况
下就需要使用sudo命令，sudo命令默认情况下需要输入当前账户的密码以验证。

可以通过修改`/etc/sudoers`文件来取消sudo密码验证

1. 一般的，`/etc/sudoers`文件的权限为440，任何用户都不能修改它。首先应当使用
    root账户身份来登录，然后改变权限：
    `chmod 777 sudoers`

2. 然后修改sudoers：
    假设当前用户为user，可以在该文件末尾添加如下命令来取消user的sudo密码
    `user ALL=(ALL) NOPASSWD:ALL`

3. 修改sudoers文件后，应当将其权限恢复为初始状态：
    `chmod 440 sudoers`

# **安装vim，和gvim**
ubuntu默认情况下仅有vi，可以按照如下方法安装和配置vim和gvim

1. 安装vim
    `sudo apt-get install vim`

2. 安装gvim
    `sudo apt-get install vim-gtk`

3. vim和gvim的配置文件路径  
    系统的vim配置文件：`/etc/vim/vimrc`  
    系统的gvim配置文件：`/etc/vim/gvimrc`  
    用户的vim配置文件：`/home/user/.vimrc`  
    在安装awesome配置后，没有显式存在的gvim配置文件

4. 安装awesome配置
    按照awesome的github项目指引完成安装即可  
    但是会出现以下问题：无法自动使用Source Code Pro字体，没有自定义的gvim主题  
    我们可以在`/home/user/.vimrc`中显式地解决这两个问题
    在.vimrc中添加如下配置：  

    `set gfn=Source\ Code\ Pro\ Medium\ 14`   #设置字体为SourceCodePro Medium 14  
    `colorscheme solarized`   #设置gvim主题为solarized  
    `set background=dark`     #使用solarized的深色背景  

5. 要使用自定义的gvim主题，还应该显式地为vim配置文件添加主题配置文件
    在.vim文件夹中创建目录`colors/`
    将下载的主题配置文件solarized.vim复制到colors目录中

# **安装Source Code Pro字体**
[Source Code Pro]是Adobe公司出品的优秀的免费开源等宽字体

[Source Code Pro]: https://github.com/adobe-fonts/source-code-pro
1. 从github上下载字体文件（使用浏览器或者wget工具都可以）

2. 安装单个字体，直接双击打开导入即可

3. 安装多个字体，可以在`/usr/share/fonts`下创建文件夹custom/  
    `chmod 755 custom`  
    将下载好的字体文件[xx].ttf全部复制到`custom/`文件夹中即可
    运行`sudo fc-cache -f -v`刷新字体缓存

# **安装文泉驿微黑字体**
1. 安装文泉驿微黑字体
    `sudo apt-get install ttf-wqy-microhei`

2. 安装界面美化工具[Ubuntu Tweak](http://ubuntu-tweak.com)，
    在软件设置选项中选择喜欢的字体

3. 在firefox浏览器中，可以在preference设定中选择content选项
    在Fonts & Colors中选择`WenQuanYi Micro Hei`

# **安装fcitx输入法框架**
1. ubuntu自带的ibus输入法很不好用，可以安装fcitx  
    `sudo apt-get install fcitx`  
    安装google拼音输入法：
    `sudo apt-get install fcitx-googlepinyin`

2. 安装fcitx后，在ubuntu顶栏会出现一个键盘图标，这就是fcitx控制框架
    点击后选择configure，选择input method选项卡
    默认情况下会存在`Keyboard-English(US)`
    点击“+”添加新的输入法，在搜索栏中输入google，就会显示出已经安装的
    `google-pinyin`

3. 点击fcitx图标，选择skin，选择dark皮肤

4. 点击fcitx图标，选择configure，选择global config
    设定如下选项：  
    `Trigger Input Method: Shift+Lshift`,  `Empty`（空格键）  
    `Extra key for trigger input method: L_SHIFT`  
    其他保持默认，不修改，
    这样设定后，左shift键为切换输入法，空格键为以英文输入。

5. 在language support中选择输入法控制器为fcitx

6. 我们还可以使用[搜狗拼音](http://pinyin.sogou.com/linux/help.php)，其使用效果比谷歌拼音更好。

# **安装Mac主题**
ubuntu默认主题非常难看，我们可以使用[noobslab](http://www.noobslab.com/2014/04/macbuntu-1404-pack-is-released.html)的Mac主题。

1. 安装主题管理工具：  
    `sudo apt-get install unity-tweak-tool`

2. 安装Mac OS X主题：  
    `sudo add-apt repository ppa:noobslab/theme`  
    `sudo apt-get update`  
    `sudo apt-get install mac-ithemes-v3`  
    `sudo apt-get install mac-icons-v3`  

3. 在unity-tweak-tool中选择Mac主题的图标，鼠标光标和窗口样式

4. 安装docky启动器：  
    `sudo add-apt-repository ppa:docky-core/ppa`  
    `sudo apt-get update`  
    `sudo apt-get instal docky`  

5. 设置docky启动器：  
    下载docky的Mac主题包  
    选择3D Background  
    选择Buyi-idock  
    在unity-tweak-tool中选择隐藏左侧边栏启动器

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
    可以在.bashrc中设定如下自定义命令：  
    `alias tpOff='xinput set-prop 11 170 0'`  
    `alias tpOn='xinput set-prop 11 170 1'`  
    那么只要在当前用户的terminal中输入tpOff和tpOn就可以禁用或者启用触摸板

# **屏幕默认亮度**
Ubuntu开机启动后，其默认亮度始终为最大亮度，这很不方便，修改如下
    在`/etc/rc.local`中添加如下命令：  
    `echo 250 > /sys/class/backlight/intel_backlight/brightness`  
    其中250为屏幕合适的亮度值。

# **命令行只显示当前目录**
默认情况下，命令行总是显示当前的完整目录，这往往不太方便，我们可以修改`.bashrc`文件，来使得其仅仅显示当前目录。修改过程可以参考这篇[blog](http://www.cnblogs.com/conservation/p/3746752.html)

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

# **Ruby on Rails**
可以参考这一篇[DigitalOcean文档](https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-14-04)

# **应用软件**
* 浏览器：Chrome
* pdf阅读器：okular
* 电子书：Calibre
* 音乐播放器：Clementine
* 视频播放器：Mplayer，ExMplayer
* 同步工具：Dropbox
* Tex：texlive
* 邮件客户端：ThunderBird
* 编辑器：Vim，Sublime
* IDE：Intellij，Android Studio，Eclipse
* 虚拟机：VirtualBox
* 图像处理：gimp
* ftp客户端：FileZilla
* BT客户端：qbittorrent
* 压缩软件：winrar
* 代理工具：openvpn，shadowsocks，SwitchyOmega
