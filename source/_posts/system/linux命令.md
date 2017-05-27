---
title: linux命令
date: 2016-10-20 07:36:09
categories: linux
---
linux常用命令
<!-- more -->

<h2>关机与重启</h2>

<h3>关机</h3>
立即关机

```mark
shutdown -h now
```
```mark
poweroff 不推荐
```
<h3>重启</h3>
```mark
shutdown -r
```
```mark
reboot
```
后台执行5点30重启

```mark
shutdown -r 05:30 &
```
<h3>取消前一个关机命令</h3>
```mark
shutdown -c
```
<h2>init</h2>
```mark
0  关机
1  单用户
2  不完全多用户，不含NFS服务
3  完全多用户
4  未分配
5  图形界面
6  重启
```

<h3>查询系统运行级别</h3>
```mark
runlevel
```

<h3>系统默认运行级别</h3>
```mark
cat /etc/inittab
```

<h3>退出登录命令</h3>
```mark
logout 退出系统
```

<h2>压缩与解压缩</h2>
压缩格式 .zip .gz .bz2 .tar.gz .tar.bz2

<h3>安装压缩解压缩工具</h3>
```mark
sudo apt-get install zip
```

<h3>.zip</h3>
<h4>压缩.zip</h4>
压缩文件
```mark
zip 压缩文件名 压缩文件
```

压缩目录
```mark
zip -r 压缩文件名 目录名 
```

<h4>解压缩.zip</h4>
```mark
unzip 压缩文件名
```

<h3>.gz</h3>
<h4>压缩.gz</h4>
不保留源文件
```mark
gzip 压缩文件
```

保留源文件
```mark
gzip -c 源文件 > 压缩文件
```

压缩目录下的所有子文件
```mark
gzip -r 目录
```

<h4>解压缩.gz</h4>
```mark
gzip -d 压缩文件名
```
```mark
gunzip 压缩文件名
```
解压缩目录下的所有文件

```mark
gunzip -r 目录名
```

<h3>.bz2</h3>
<h4>压缩.bz2</h4>
bzip2不能压缩目录

不保留源文件

```mark
bzip2 源文件
```
保留源文件

```mark
bzip2 -k 源文件
```

<h4>解压缩.bz2</h4>
```mark
bunzip2 压缩文件
```
```mark
bzip2 -d 压缩文件
```
-k保留压缩文件

<h3>tar打包命令</h3>
```mark
tar -cvf 打包文件名 源文件
选项：
    -c: 打包
    -v: 显示过程
    -f: 指定打包后的文件名
例：
tar -cvf test.tar test
```

<h3>.tar.gz</h3>
<h4>压缩.tar.gz</h4>
```mark
tar -zcvf 压缩包名.tar.gz 源文件
```
<h4>解压缩.tar.gz</h4>
```mark
tar -zxvf 压缩包名.tar.gz 
```

<h3>.tar.bz2</h3>
<h4>压缩.tar.bz2</h4>
```mark
tar -jcvf 压缩包名.tar.bz2 源文件
```
<h4>解压缩.tar.bz2</h4>
```mark
tar -jxvf 压缩包名.tar.bz2 
```

<h2>帮助命令</h2>
<h3>man的级别</h3>

```mark
1 ：常看命令的帮助
2 ：查看可被内核调用的函数的帮助
3 ：查看函数和函数库的帮助
4 ：查看特殊文件的帮助（主要是/dev目录下的文件）
5 ：查看配置文件的帮助
6 ：查看游戏的帮助
7 ：查看其它杂项的帮助
8 ：查看系统管理员可用命令的帮助
9 ：查看和内核相关文件的帮助
```
<h3>显示命令帮助</h3>

```mark
man 命令
```
<h4>显示命令帮助等级</h4>

```mark
man -f 命令
```
相当于

```mark
whatis 命令
```
<h4>调用相应等级的帮助</h4>

```mark
man -等级 命令
```
<h4>查看和命令相关的所有帮助</h4>

```mark
man -k 命令
```
相当于

```mark
apropos 命令
```

<h3>选项帮助</h3>
```mark
命令 --help
```

<h3>shell内部命令帮助</h3>
<h4>确定是否是shell命令</h4>
```mark
whereis 命令
```
<h4>获取内部命令帮助</h4>
```mark
help shell内部命令
```

<h3>详细命令帮助info</h3>
```mark
info 命令
- 回车 : 进入子帮助页面（带有*号标记）
- u   : 进入上层页面
- n   : 进入下一个帮助小节
- p   : 进入上一个帮助小节
- q   : 退出
```


<h2>目录</h2>
<h3>建立目录</h3>
```mark
mkdir 目录名
```
递归创建

```mark
mkdir -p 目录名/目录名
```

<h3>切换目录</h3>
进入当前用户的家目录

```mark
cd ~ 
```

<h3>清屏</h3>
control + l


<h3>查看debian版本</h3>
```mark
more /etc/debian_version 
```
<h3>查看内核版本</h3>
```markdown
uname -a
```

<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
