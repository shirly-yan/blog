---
title: UIBlurEffect系统自带毛玻璃效果
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
   //需要模糊效果的view
   UIImageView*i = [[UIImageViewalloc]initWithFrame:CGRectMake(100,100,100,200)];
    [self.viewaddSubview:i];
    i.image= [UIImageimageNamed:@"1"];
   
   //创建UIBlurEffect
   UIBlurEffect*blurEffect = [UIBlurEffecteffectWithStyle:UIBlurEffectStyleLight];

   //UIVisualEffectView
   UIVisualEffectView*effectView = [[UIVisualEffectViewalloc]initWithEffect:blurEffect];   //设置模糊效果的frame    effectView.frame = i.bounds;   //添加到view上    [iaddSubview:effectView];    //设置模糊的程度    effectView.alpha =.5f;
typedefNS_ENUM(NSInteger, UIBlurEffectStyle) {    UIBlurEffectStyleExtraLight,    UIBlurEffectStyleLight,
    UIBlurEffectStyleDark}