---
title: linux时间
date: 2017-08-20 17:36:09
categories: linux
---
linux时间的查看和设置
<!-- more -->

<h2>查看时间</h2>
```bash
date
```
<h2>以特定的格式 查看时间</h2>
```bash
date "+%Y-%m-%d"
```
<h2>根据网络 校准时间</h2>
1.  安装ntpdate工具
```bash
apt-get install ntpdate
```

2.  设置系统时间与网络时间同步
```bash
ntpdate cn.pool.ntp.org
```


<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
