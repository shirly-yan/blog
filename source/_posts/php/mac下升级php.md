---
title: mac下升级php
date: 2016-10-10 14:08:56
categories: [php]
---
mac下升级php
<!-- more -->

<h3>安装</h3>
[参考链接](https://php-osx.liip.ch)

安装php5.6
```bash
curl -s http://php-osx.liip.ch/install.sh | bash -s 5.6
```

安装php7.1
```bash
$ curl -s http://php-osx.liip.ch/install.sh | bash -s 7.1
```

<h3>配置Apache支持php7</h3>
终端输入
```bash
$ sudo vim /etc/apache2/httpd.conf
```

注释掉对php5的支持
```
#LoadModule php5_module libexec/apache2/libphp5.so
```
加入对php7的支持
```
#########php7###########
LoadModule php7_module   /usr/local/php5/libphp7.so

<IfModule mod_php7.c>

    AddType application/x-httpd-php .php
    AddType application/x-httpd-php-source .phps

    <IfModule mod_dir.c>
        DirectoryIndex index.html index.php
    </IfModule>

</IfModule>
#########php7###########
```

重启Apache
```bash
$ sudo apachectl  restart
```

<h3>删除php</h3>
```bash
$ sudo rm -rf /usr/local/*php*
$ sudo rm -rf /usr/local/var/*php*
```








