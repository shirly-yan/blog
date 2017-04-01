---
title: linux环境变量
date: 2017-03-21 11:26:44
categories: linux
---
linux环境变量
<!-- more -->

<h2>命令</h2>
<h3>source 命令</h3>
重新加载配置文件
```
source 配置文件
```
或
```
. 配置文件
```

+ .  代表 source
+ ./ 代表 当前目录

<h3>umask 命令</h3>
查看系统默认权限
```bash
umask
```

+ r 4
+ w 2
+ x 1

<h2>登录后的配置文件</h2>
+ /etc/profile         对所有用户起作用
+ /etc/profile.d/*.sh  对所有用户起作用
+ /etc/bashrc          对所有用户起作用
+ ~./bash_profile      对所属的用户起作用
+ ~./bashrc            对所属的用户起作用

<h2>退出登录后的配置文件</h2>
+ ~./bash_logout      对所属的用户起作用


<h2>配置文件加载顺序</h2>
<img src="/images/47.png" width="800" height="363" />

<h3>正常过程</h3>
<img src="/images/48.png" width="800" height="363" />

<h3>su切换过程</h3>
<img src="/images/49.png" width="800" height="363" />

<h2>/etc/profile 的作用</h2>
+ user变量
+ logname变量
+ mail变量
+ path变量
+ hostname变量
+ histsize变量
+ umask
+ 调用/etc/profile.d/*.sh文件

<h2>/etc/bashrc 的作用</h2>
+ ps1变量
+ umask
+ path变量
+ 调用/etc/profile.d/*.sh文件


<h2>/etc/issue</h2>
<img src="/images/50.png" width="800" height="460" />

<h2>shell中的变量</h2>
+ 脚本 中的 自定义变量
+ 环境变量 中的 局部环境变量
+ 环境变量 中的 全局环境变量

<h3>自定义变量</h3>
+ 合法字符：字符 数字 下划线
+ 字符数量<=20
+ <font color=#FF6666>区分大小写</font>

<h4>声明变量</h4>
变量名 = 变量值
```bash
 var = 2 
```
<h4>变量值得类型</h4>
自动分配 数字 字符串 日期 数组

<h4>调用方式</h4>
$+变量名 
```bash 
$var
```
<h4>作用范围</h4>
脚本生存期内
<h3>环境变量</h3>
查看所有的环境变量
```bash
env
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
