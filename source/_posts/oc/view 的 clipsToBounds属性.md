---
title: view 的 clipsToBounds属性
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

**取值：**BOOL（YES/NO）**作用：**决定了子视图的显示范围当取值为YES时，超出父视图范围的子视图被剪裁不显示；当取值为NO时，超出父视图范围的子视图不被剪裁，显示。默认值为NO。
如下图所示：view2是view1的子视图
**取值为NO时：**
**取值为YES时：**![](http://img.blog.csdn.net/20160525095602290?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![](http://img.blog.csdn.net/20160525095512445?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)