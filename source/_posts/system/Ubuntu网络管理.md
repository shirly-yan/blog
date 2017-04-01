---
title: linux网络管理
date: 2016-10-23 15:32:25
categories: linux
---
linux网络管理
<!-- more -->

<h2>iso/osi七层模型</h2>
```mark
iso : 国际标准化组织
osi : 开放系统互联模型
ios ：苹果操作系统
```
```mark
                                    数据单位
              应用层协议
7  应用层   <------------->  应用层   APDU
              表示层协议
6  表示层   <------------->  表示层   PPDU
              会话层协议
5  会话层   <------------->  会话层   SPDU
              传输层协议
4  传输层   <------------->  传输层   TPDU
              网络层协议
3  网络层   <------------->  网络层    报文
             数据链路层协议
2 数据链路层 <-------------> 数据链路层  帧
              物理层协议
1  物理层   <------------->  物理层    比特
```

1 2 3 4 实际进行数据传输
5 6 7   给用户提供服务的

mac地址负责局域网通信
ip地址负责外网通信

应用层：用户接口
表示层：数据的表现形式、特定功能的实现如加密
会话层：对应用会话的管理、同步
传输层：可靠与不可靠的传输、传输前的错误检测、流控
网络层：提供逻辑地址、选路
数据链路层：成帧、用mac地址访问媒介、错误监测与纠正
物理层：设备之间的比特流的传输、物理接口、电气特性等

<h2>TCP/IP协议4层模型</h2>
<h3>TCP/IP模型与OSI模型的对应</h3>
```mark
OSI7层模型  TCP/IP4层模型
 应用层    |
 表示层    }  应用层
 会话层    |
 传输层    }  传输层
 网络层    }  网络互联层
数据链路层  }  网络接口层
 物理层    |
```
<h2>IP地址</h2>
<h3>数据封装过程</h3>
<img src="/images/37.png" width="800" height="406" />

<h3>IP包头</h3>
<img src="/images/36.png" width="800" height="406" />

<h3>IP地址分类</h3>
<img src="/images/38.png" width="800" height="406" />


<h2>端口号</h2>
<h3>TCP协议包头</h3>
<img src="/images/39.png" width="800" height="406" />

<h3>UDP协议包头</h3>
<img src="/images/40.png" width="800" height="406" />

<h3>常见端口号</h3>
<img src="/images/41.png" width="800" height="406" />

<h2>DNS</h2>
DNS Domain Name System
 









<h2>查看网络信息</h2>
<h3>查看网络信息</h3>
```mark
# ifconfig
```
<h3>重启网络</h3>
```mark
sudo /etc/init.d/networking restart
```
<!--<img src="/images/6.png" width="800" height="263" />-->
<!--<font color=#FF6666></font>-->
