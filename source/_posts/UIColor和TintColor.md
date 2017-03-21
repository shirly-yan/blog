---
title: UIColor和TintColor
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

前者是指颜色，tint是指着色，色调。
通过tintColor属性可以定制UINavigationBar的背景颜色，但如果需要设定渐变色、甚至纹理来说，就需要贴图了。比较“暴力”的一种做法就是通过Category来重新实现- (void)
 drawRect:(CGRect)rect的实现，“暴力”是因为这种杀伤面很广，所有项目内的UINavigationBar都会因此改变。
