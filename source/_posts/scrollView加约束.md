---
title: scrollView加约束
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

#尊重原创 转自：http://www.jianshu.com/p/1cfeb1eab6c6
#

#先办到能设置contentSize
- 得添加一个额外的视图,占位
- scrollView中只添加个UIView
- 其它的控件全部放在这个UIView上
- 设置UIView的高度,即为scrollView的contendSize
- 得设置水平居中
- 约束完毕

##上下滚动
- 1.添加一个UIView类型的子控件(这将是UIScrollView唯一的一个子控件)
- 2.上下左右0
- 3.高度(contentSize的高度,可滚动的高度)
- 4.水平居中

##左右滚动
- 1.添加一个UIView类型的子控件(这将是UIScrollView唯一的一个子控件)
- 2.上下左右0
- 3.宽度(contentSize的宽度,可滚动的宽度)
- 4.竖直居中

##上下左右都能滚动
- 1.添加一个UIView类型的子控件(这将是UIScrollView唯一的一个子控件)
- 2.上下左右0
- 3.宽度,高度(contentSize的宽度,可滚动的宽度,contentSize的高度,可滚动的高度)



