---
title: centos与ubuntu不同
date: 2016-10-23 15:32:25
categories: linux
---
centos与ubuntu不同
<!-- more -->

1.关于登录用户
centos可以使用root登录  ubuntu不能使用root登录
centos普通用户默认不能通过sudo取得root权限执行命令， ubuntu可以，centos需要sudo时需要修改/etc/sudoers文件
2.关于网络配置
  ubuntu的网络配置文件是在/etc/network/interface文件中，所有网卡都使用一个文件就可以了
  centos的配置文件在/etc/sysconfig/network-scripts下，而且一个网卡一个配置文件，分别是ifcfg-eth0,ifcfg-eth1 .....
  ubuntu重启网络的脚本是/etc/init.d/networking [start|stop|restart],centos是 /etc/init.d/network [start|stop|restart]
3. 自动安装软件
 ubuntu使用apt-get  centos使用yum
4.关于启动项
ubuntu的启动机制分析
据说ubuntu使用upstart机制实现服务的启动，upstart是个什么玩意，我不知道，但我认真分析了一下ubuntu的各种启动文件发现的这样的一些关系
/etc/init.d/ 这个目录里放置了ubuntu下得所有启动项，与centos不同的是，这些不是脚本，而只是一个连接到了/lib/init/upstart-job下，
upstart-job脚本执行了类似 $command $JOB的命令
当用户执行 service MySQL stop时
$command 就是stop $JOB就是mysql
相当于执行了stop mysql
stop又是/sbin/initctl的一个软连接
这就相当于执行了initctl这个程序，用来启动关闭mysql
单mysql究竟去哪里找，是一个elf文件还是一个脚本，这个实在不清楚
 
 
4.关于安装mysql的区别
 centos上使用yum install mysql就可以安装mysql了
 安装后启动文件分别是：
/etc/init.d/mysqld 是服务启动脚本
/usr/bin/mysqld_safe是一个守护脚本
/usr/libexec/mysqld 是mysql的服务程序
yum install 安装后mysql初次登录不需要密吗
 
apt-get 安装mysql在安装的过程中要求输入密码
 