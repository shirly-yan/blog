---
title: Masonry介绍与使用实践：快速上手Autolayout
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

转自：http://www.cocoachina.com/ios/20141219/10702.html
前言
	MagicNumber -> autoresizingMask -> autolayou
以上是纯手写代码所经历的关于页面布局的三个时期
在iphone1-iphone3gs时代 window的size固定为(320,480) 我们只需要简单计算一下相对位置就好了
在iphone4-iphone4s时代 苹果推出了retina屏 但是给了码农们非常大的福利:window的size不变
在iphone5-iphone5s时代 window的size变了(320,568) 这时autoresizingMask派上了用场(为啥这时候不用Autolayout? 因为还要支持ios5呗) 简单的适配一下即可
在iphone6+时代 window的width也发生了变化(相对5和5s的屏幕比例没有变化) 终于是时候抛弃autoresizingMask改用autolayout了(不用支持ios5了 相对于屏幕适配的多样性来说autoresizingMask也已经过时了)
那如何快速的上手autolayout呢? 说实话 当年ios6推出的同时新增了autolayout的特性 我看了一下官方文档和demo 就立马抛弃到一边了 因为实在过于的繁琐和啰嗦(有过经验的朋友肯定有同感)
直到iPhone6发布之后 我知道使用autolayout势在必行了 这时想起了以前在浏览Github看到过的一个第三方库Masonry 在花了几个小时的研究使用后 我就将autolayout掌握了(重点是我并没有学习任何的官方文档或者其他的关于autolayout的知识) 这就是我为什么要写下这篇文章来推荐它的原因.
**介绍**
Masonry 源码：[https://github.com/Masonry/Masonry](https://github.com/Masonry/Masonry)
Masonry是一个轻量级的布局框架 拥有自己的描述语法 采用更优雅的链式语法封装自动布局 简洁明了 并具有高可读性 而且同时支持 iOS 和 Max OS X。
我们先来看一段官方的sample code来认识一下Masonry
	[view1 mas_makeConstraints:^(MASConstraintMaker *make) {
	    make.edges.equalTo(superview).with.insets(padding);
	}];
看到block里面的那句话: make edges equalTo superview with insets
通过链式的自然语言 就把view1给autolayout好了 是不是简单易懂?
**使用**
看一下Masonry支持哪一些属性
	@property (nonatomic, strong, readonly) MASConstraint *left;
	@property (nonatomic, strong, readonly) MASConstraint *top;
	@property (nonatomic, strong, readonly) MASConstraint *right;
	@property (nonatomic, strong, readonly) MASConstraint *bottom;
	@property (nonatomic, strong, readonly) MASConstraint *leading;
	@property (nonatomic, strong, readonly) MASConstraint *trailing;
	@property (nonatomic, strong, readonly) MASConstraint *width;
	@property (nonatomic, strong, readonly) MASConstraint *height;
	@property (nonatomic, strong, readonly) MASConstraint *centerX;
	@property (nonatomic, strong, readonly) MASConstraint *centerY;
	@property (nonatomic, strong, readonly) MASConstraint *baseline;
这些属性与NSLayoutAttrubute的对照表如下


其中leading与left trailing与right 在正常情况下是等价的 但是当一些布局是从右至左时(比如阿拉伯文?没有类似的经验) 则会对调 换句话说就是基本可以不理不用 用left和right就好了
在ios8发布后 又新增了一堆奇奇怪怪的属性(有兴趣的朋友可以去瞅瞅) Masonry暂时还不支持(不过你要支持ios6,ios7 就没必要去管那么多了)
在讲实例之前 先介绍一个MACRO
1`#define WS(weakSelf)  __weak __typeof(&*self)weakSelf = self;`快速的定义一个weakSelf 当然是用于block里面啦 下面进入正题(为了方便 我们测试的superView都是一个size为(300,300)的UIView)
下面 通过一些简单的实例来简单介绍如何轻松愉快的使用Masonry:
1. [基础] 居中显示一个view
1234567891011121314151617`- (void)viewDidLoad``{``    ``[``super` `viewDidLoad];``    ``// Do any additional setup after loading the view.``    ` `    ``WS(ws);``    ` `    ``UIView *sv = [UIView ``new``];``    ``[sv showPlaceHolder];``    ``sv.backgroundColor = [UIColor blackColor];``    ``[self.view addSubview:sv];``    ``[sv mas_makeConstraints:^(MASConstraintMaker *make) {``        ``make.center.equalTo(ws.view);``        ``make.size.mas_equalTo(CGSizeMake(300, 300));``    ``}];``    ` `}`
