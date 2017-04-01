---
title: apt
date: 2016-11-15 13:49:16
categories: linux
tags: [linux,apt]
---
apt
<!-- more -->
<h2>apt源</h2>
<h3>中科大</h3>
```markdown
deb http://ftp.cn.debian.org/debian jessie main contrib non-free
deb-src http://ftp.cn.debian.org/debian jessie main contrib non-free

deb http://ftp.cn.debian.org/debian jessie-updates main contrib non-free
deb-src http://ftp.cn.debian.org/debian jessie-updates main contrib non-free

deb http://ftp.cn.debian.org/debian-security/ jessie/updates main contrib non-free
deb-src http://ftp.cn.debian.org/debian-security/ jessie/updates main contrib non-free

deb http://security.debian.org jessie/updates main contrib non-free
```

<h3>163</h3>
```markdown
deb http://mirrors.163.com/debian jessie main non-free contrib
deb http://mirrors.163.com/debian jessie-proposed-updates main contrib non-free

deb-src http://mirrors.163.com/debian jessie main non-free contrib
deb-src http://mirrors.163.com/debian jessie-proposed-updates main contrib non-free

deb http://mirrors.163.com/debian-security jessie/updates main contrib non-free 
deb-src http://mirrors.163.com/debian-security jessie/updates main contrib non-free 

deb http://security.debian.org jessie/updates main contrib non-free
```
<h3>常用命令</h3>
apt-cache search packagename 搜索包
apt-cache show packagename 获取包的相关信息，如说明、大小、版本等
apt-get install packagename 安装包
apt-get install packagename --reinstall 重新安装包
apt-get -f install 修复安装”-f = –fix-missing”
apt-get remove packagename 删除包
apt-get remove packagename --purge 删除包，包括删除配置文件等
apt-get update 更新源
apt-get upgrade 更新已安装的包
apt-get dist-upgrade 升级系统
apt-get dselect-upgrade 使用 dselect 升级
apt-cache depends packagename 了解使用依赖
apt-cache rdepends packagename 是查看该包被哪些包依赖
apt-get build-dep packagename 安装相关的编译环境
apt-get source packagename 下载该包的源代码
apt-get clean 清理无用的包
apt-get autoclean 清理无用的包
apt-get check 检查是否有损坏的依赖


apt卸载nginx方法
卸载方法1.
# 删除nginx，保留配置文件
sudo apt-get remove nginx
#删除配置文件
rm -rf /etc/nginx

卸载方法2.
#删除nginx连带配置文件
sudo apt-get purge nginx # Removes everything.

#卸载不再需要的nginx依赖程序
sudo apt-get autoremove


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
