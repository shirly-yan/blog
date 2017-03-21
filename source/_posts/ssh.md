---
title: ssh
date: 2016-11-15 13:37:55
categories: linux
---
debian下的ssh
<!-- more -->

<h3>开启ssh服务</h3>
默认请况下，ubuntu是不允许远程登陆的。（因为服务没有开，可以这么理解。）
想要用ssh登陆的话，要在需要登陆的系统上启动服务。即，安装ssh的服务器端
```markdown
# apt-get install openssh-server
```
然后，启动服务。
```markdown
# service ssh start
```
或者是：
```markdown
# /etc/init.d/ssh restart
```
<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
