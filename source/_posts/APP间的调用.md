---
title: APP间的调用
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
被调用的App：
1.在appdelegate里实现方法

-(BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options
{
    NSLog(@"%@",app);
    NSLog(@"%@",url);
    NSLog(@"%@",options);
    return  YES;
}

2.配置Info.plist文件



调用方法：

    //打开另一个APP的方法
    //a另一个APP的openurl
    //aaaaaaaaa参数
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:@"a:aaaaaaa"]];

```

