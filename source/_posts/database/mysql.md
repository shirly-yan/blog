---
title: mysql
date: 2017-05-09 19:10:10
categories: [mysql]
tags: [mysql]
---
mac上mysql配置和使用
<!-- more -->

<h2>安装mysql</h2>
[官网下载社区版本](https://dev.mysql.com/downloads/file/?id=469584)

<h2>完全删除mysql</h2>
执行下面全部命令才能完全卸载干净
```bash
$ sudo rm -rf /usr/local/*mysql*
$ sudo rm -rf /usr/local/var/*mysql*
$ sudo rm -rf /Library/StartupItems/*MySQL*
$ sudo rm -rf /Library/PreferencePanes/*MySQL*
$ sudo rm -rf ~/Library/PreferencePanes/*MySQL*
$ sudo rm -rf /Library/Receipts/*mysql*
$ sudo rm -rf /Library/Receipts/*MySQL*
$ sudo rm -rf /private/var/db/receipts/*mysql*
```

<h2>mysql命令</h2>
<h3>登录</h3>
```bash
$ mysql -u root -p
```
<h3>重置密码</h3>
登录进入mysql后 重置密码
```markdown
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('password');
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
