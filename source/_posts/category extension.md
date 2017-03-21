---
title: category extension
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

category和extension用来做类扩展的，可以对现有类扩展功能或者修改其功能。在iOS中category应用是非常广泛的，系统自带的很多类都有多个category扩展功能。
一般category中可以定义新的方法、重写类原来的方法和添加readonly属性
而extension可以认为是匿名的category，但是这个extension相对于category有有一个特殊功能：在extension中可以定义可写的属性，公有可读、私有可写的属性(Publicly-Readable, Privately-Writeable Properties)一般这样实现！
举例说明如下：1. 创建测试程序empty application2. 我们自定义一个UIViewController，命名为RootViewController，它的.h文件为：###[]()[代码]c#/cpp/oc代码：
`01``//``02``// 
 RootViewController.h``03``// 
 Test4``04``//``05``// 
 Created by Vincent on 13-5-29.``06``// 
 Copyright (c) 2013年 DevDiv Community. All rights reserved.``07``//``08` `09``#import
 <UIKit/UIKit.h>``10` `11``@``interface` `RootViewController
 : UIViewController``12``@end`

那么在其对应的.m中会自动生成以下代码：###[]()[代码]c#/cpp/oc代码：
`01``//``02``// 
 RootViewController.m``03``// 
 Test4``04``//``05``// 
 Created by Vincent on 13-5-29.``06``// 
 Copyright (c) 2013年 DevDiv Community. All rights reserved.``07``//``08` `09``#import
 "RootViewController.h"``10` `11``@``interface` `RootViewController
 ()``12``@end``13` `14``@implementation
 RootViewController``15` `16` `17``-
 (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil``18``{``19``    ``self
 = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];``20``    ``if` `(self)
 {``21``        ``//
 Custom initialization``22``    ``}``23``    ``return` `self;``24``}``25` `26``-
 (``void``)viewDidLoad``27``{``28``    ``[super
 viewDidLoad];``29``    ``//
 Do any additional setup after loading the view.``30``    ``self.title
 = ``@"RootController"``;``31``    ``self.navigationItem``32``}``33` `34``-
 (``void``)didReceiveMemoryWarning``35``{``36``    ``[super
 didReceiveMemoryWarning];``37``    ``//
 Dispose of any resources that can be recreated.``38``}``39` `40``@end`
3. 第2步中我们能看到###[]()[代码]c#/cpp/oc代码：
`1``@``interface` `RootViewController
 ()``2``@end`
这个就是extension了（也就是特殊类型的category）
如果我们在.h添加这样一个属性@property (readonly) float value;那么RootViewController对外就暴露一个readonly的属性，它是公开的，所以外部是不能够对它进行写操作的。这时我们可以在extension加入以下代码：@property (readwrite) float value;那么这个属性在内部就是可读写的了，如果是只读只能在构造时期对它赋值，其他类方法中是不能对其赋值的。有了这个特性支持，那么类的内部方法均可以对其进行赋值了。


