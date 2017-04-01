---
title: debian安装sudo
date: 2016-10-20 16:31:18
categories:
---
debian安装sudo命令
<!-- more -->
这条命令在ubuntu下可是每日必备、在debian下就要自己安装了、首先切换到root用户、su命令然后输入root密码登录root身份、再执行apt-get install sudo、这个冬冬不明白为什么一定要cd1才能安装、不解不解、那就插入cd1的光碟吧、安装完成后还要修改一下sudoers的文件不然会出现"xxx is not in the sudoers file. This incident will be reported.“的提示

我们只要修改一下/etc/sudoers文件就行了。下面是修改方 法：

1）进入超级用户模式。也就是输入"su -",系统会让你输入超级用户密码，输入密码后就进入了超级用户模式。（当然，你也可以直接用root用） 
2）添加文件的写权限。也就是输入命令"chmod u+w /etc/sudoers"。 
3） 编辑/etc/sudoers文件。也就是输入命令"vi /etc/sudoers",输入"i"进入编辑模式，找到这一 行："root ALL=(ALL) ALL"在起下面添加"xxx ALL=(ALL) ALL"(这里的xxx是你的用户名)，然后保存（就是先按一 下Esc键，然后输入":wq"）退出。 
4）撤销文件的写权限。也就是输入命令"chmod u-w /etc/sudoers"。

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
