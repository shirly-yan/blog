---
title: NSUserDefaults
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
/*NSUserDefaults通常用于存储用户偏好设置和登陆注册等信息*/

    //取值
    [[NSUserDefaults standardUserDefaults]objectForKey:@"night"];
    //存值
    [[NSUserDefaults standardUserDefaults] setObject:@"yes" forKey:@"night"];


//存储数据
    NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];
   
    [userDefault setObject:@"张三" forKey:@"name"];
    [userDefault setInteger:23 forKey:@"age"];  
  
//取出数据
    self.navigationItem.title = [[NSUserDefaults standardUserDefaults]objectForKey:@"name"];
    NSLog(@"%ld",[[NSUserDefaults standardUserDefaults]integerForKey:@"age" ]);

//修改数据
    [[NSUserDefaults standardUserDefaults]setObject:@"李四" forKey:@"name"];

 //强制存储
    /*原因：NSUserDefaults存储数据会发生延迟现象，把数据存储到本地文件中，不是马上就生效，所以会出现娶不到数据现象*/
    [[NSUserDefaults standardUserDefaults]synchronize];

```

