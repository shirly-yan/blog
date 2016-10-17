---
title: iOS隐藏状态栏
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
状态栏： 运营商时间电量的行
#pragma mark  隐藏状态栏
-(BOOL)prefersStatusBarHidden
{
    return YES;
}
```

