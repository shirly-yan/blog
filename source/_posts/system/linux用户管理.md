---
title: linux用户管理
date: 2016-10-20 15:26:13
categories: linux
---
linux用户管理
<!-- more -->

<h2>查看所有的用户组</h2>
```mark
cat /etc/group
```
```mark
deploy :    x       : 1000  :
组名称  : 组密码占位符 : 组编号 : 组中用户名列表
```

<h2>查看所有的用户组组密码</h2>
```mark
cat /etc/gshadow
```
```mark
deploy :   ！  :         :
组名称  : 组密码 : 组管理者 : 组中用户名列表
```

<h2>查看所有的用户</h2>
```mark
cat /etc/passwd
```
```mark
 deploy :    x     :   1000  :  1000    :     ,,,    : /home/deploy : /bin/bash
  用户名 : 密码占位符 : 用户编号 : 用户组编号 : 用户注释信息 : 用户主目录    : shell类型
```
<h2>查看所有的用户密码</h2>
```mark
cat /etc/shadow
```
```mark
 deploy : $6$L :17095:0:99999:7:::

  用户名 : 密码 :      : :     : :::
```
<h2>添加用户组</h2>
```mark
groupadd groupname
```
指定用户组编号
```mark
groupadd -g groupnumber groupname
```

<h2>修改用户组组名</h2>
```mark
groupmod -n newgroupname oldgroupname
```

<h2>修改用户组编号</h2>
```mark
groupmod -g groupnumber groupname
```

<h2>删除用户组</h2>
```mark
groupdel groupname
```

<h2>添加用户</h2>

```mark
useradd username
adduser username 推荐
```
Ubuntu系列linux useradd命令创建的用户属于三无用户 ，不能登录图形界面，在/home/下创建用户所属者所属组的同名目录，就可登录图形界面

指定用户组

```mark
useradd -g groupname username
```
指定用户个人文件夹

```mark
useradd -d /home/xxx username
```

<h2>修改用户信息</h2>
修改用户名

```mark
usermod -l newUserName oldUserName
```
修改用户的用户组

```mark
usermod -g groupname username
```
修改用户文件夹

```mark
usermod -d /home/xxx usernaem
```
修改用户注释信息

```mark
usermod -c userinfo username
```

<h2>删除用户</h2>
只删除用户保留用户文件

```mark
userdel username
```
删除用户及用户相关文件

```mark
userdel -r username
```
<h2>禁止root用户以外的用户登录</h2>
touch /etc/nologin

<h2>锁定账户</h2>
passwd -l username

<h2>解锁账户</h2>
passwd -u username

<h2>清除账户密码</h2>
passwd -d username
可以无密码登录

<h2>设置密码</h2>
passwd username

<h2>添加附属组</h2>
gpasswd -a username groupname

<h2>删除附属组</h2>
gpasswd -d username groupname

<h2>切换组</h2>
newgrp groupname 

<h2>改变组密码</h2>
gpasswd groupname

<h2>切换用户</h2>
su username

<h2>显示当前的用户名</h2>
whoami

<h2>显示用户信息</h2>
id username

<h2>显示用户所在的所有组</h2>
groups username

<h2>修改用户详细资料</h2>
chfn username

<h2>显示用户详细资料</h2>
finger username

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
