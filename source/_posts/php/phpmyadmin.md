---
title: phpmyadmin
date: 2017-05-01 19:01:09
categories: [php]
tags: [phpmyadmin,php]
---
mac下phpmyadmin的配置和使用
<!-- more -->

<h3>下载</h3>
[官网下载](http://www.phpmyadmin.net/home_page/downloads.php)

<h3>安装</h3>
将下载下来的解压放在
```bash
$ /Library/WebServer/Documents/
```
目录下，完整的目录为：
```bash
$ /Library/WebServer/Documents/phpmyadmin/
```

<h3>配置</h3>
终端进入phpmyadmin目录
```bash
$ /Library/WebServer/Documents/phpmyadmin/
```
生成配置文件
```bash
$ cp config.sample.inc.php config.inc.php  
```
修改配置文件
```bash
$ vim config.inc.php 
```  

按照下面进行修改：
```
$cfg['blowfish_secret'] = '';//用于Cookie加密，随意的长字符串  
$cfg['Servers'][$i]['host'] = '127.0.0.1';//MySQL守护程序做了IP绑定
```
  
现在可以在浏览器中输入URL:
http://localhost/phpmyadmin/



<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
