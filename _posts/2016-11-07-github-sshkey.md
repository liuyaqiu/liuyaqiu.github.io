---
layout: post
title: "使用sshkey方式连接github"
categories: tutorial
---

使用sshkey连接github不仅更加安全，而且可以免去每次push时输入密码的繁琐。

# **生成SSH key**
`ssh-keygen -t rsa`可以生成RSA类型的密钥，关于ssh-keygen的更多使用细节，可以通过man查看文档。生成的公钥默认名称为id_rsa.pub，私钥默认名称为id_rsa。

# **在github上添加公钥**
在github的[`密钥设置`](https://github.com/settings/keys)选项中添加生成密钥对的公钥，复制公钥中的内容填入即可。

# **关联本地密钥到github网站**
首先运行命令`sshadd your_raskey`将私钥添加到ssh本地密钥库中，然后进行测试`ssh -vT git@github.com`，最后提示`Hi ！You've successfully authenticated`即表明已经成功关联。

# **修改repo的remote url**
github repo的默认remote为https方式，要使用ssh连接，需要修改为ssh模式。可以使用`git remote -v`查看当前remote url。例如对于本博客，到github页面中复制ssh remote url: "git@github.com:liuyaqiu/liuyaqiu.github.io.git"，然后运行`git remote set-url origin git@github.com:liuyaqiu/liuyaqiu.github.io.git`。

现在就可以方便地使用SSH方式进行git push提交修改到github了。
