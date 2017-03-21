---
title: UISwitch 圆点按钮
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
//UISwitch的初始化
    UISwitch *switchView = [[UISwitch alloc] initWithFrame:CGRectMake(54.0f, 16.0f, 100.0f, 28.0f)];
    [view addSubview:switchView];
   
   
    //设置UISwitch的初始化状态
    switchView.on = YES;//设置初始为ON的一边
   
   
    //UISwitch事件的响应
    [switchView addTarget:self action:@selector(switchAction:) forControlEvents:UIControlEventValueChanged];
  
    //开启状态的颜色
    switchView.onTintColor = [UIColor redColor];
   
    //关闭状态的颜色
    switchView.tintColor = [UIColor greenColor];

    //圆圈的颜色
    switchView.thumbTintColor = [UIColor yellowColor];
```

