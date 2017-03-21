---
title: iOS开发拓展篇—CoreLocation简单介绍
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

**一、简介**
1.在移动互联网时代，移动app能解决用户的很多生活琐事，比如
（1）导航：去任意陌生的地方
（2）周边：找餐馆、找酒店、找银行、找电影院
 
2.在上述应用中，都用到了地图和定位功能，在iOS开发中，要想加入这2大功能，必须基于2个框架进行开发
（1）Map Kit ：用于地图展示
（2）Core Location ：用于地理定位
 
3.两个热门专业术语
（1）LBS ：Location Based Service（基于定位的服务）
（2）SoLoMo ：Social Local Mobile（索罗门）
 
**二、CoreLocation框架的使用**
1.CoreLocation框架使用前提
（1）导入框架
　　![](http://images.cnitblog.com/i/450136/201408/091531084436493.png) 
说明：在Xcode5以后，不再需要我们手动导入
（2）导入主头文件
　　#import <CoreLocation/CoreLocation.h>
 
2.CoreLocation框架使用须知
CoreLocation框架中所有数据类型的前缀都是CL
CoreLocation中使用CLLocationManager对象来做用户定位
 
**三、经纬度等地理信息扫盲**
1.示意图
**　　![](http://images.cnitblog.com/i/450136/201408/091533193501020.png)**
2.本初子午线：穿过英国伦敦格林文治天文台
往东边（右边）走，是东经（E）
往西边（左边）走，是西经（W）
东西经各180°，总共360°
 
3.赤道：零度维度
往北边（上边）走，是北纬（N）
往南边（下边）走，是南纬（S）
南北纬各90°，总共180°
 
提示：横跨经度\纬度越大（1° ≈ 111km），表示的范围就越大，在地图上看到的东西就越小
4.我国的经纬度:
（1）中国的经纬度范围
纬度范围：N 3°51′ ~  N 53°33′
经度范围：E 73°33′ ~  E 135°05′
（2）部分城市的经纬度
　　![](http://images.cnitblog.com/i/450136/201408/091536473652603.png)
 
**四、模拟位置**
说明：在对程序进行测试的时候，设置手机模拟器的模拟位置（经纬度）
![](http://images.cnitblog.com/i/450136/201408/091538586931958.png)    ![](http://images.cnitblog.com/i/450136/201408/091539108818080.png)
