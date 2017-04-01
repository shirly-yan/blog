---
title: UIDatePicker
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
self.datePicker = [[UIDatePicker alloc]initWithFrame:CGRectMake(0, 200, 375, 267)];
   
    self.datePicker.backgroundColor = [UIColor whiteColor];
   
    //设置地区
    self.datePicker.locale = [[NSLocale alloc]initWithLocaleIdentifier:@"zh_CN"];
   
   
   
    //设置DatePicker的日历，默认为当天。
    self.datePicker.calendar = [NSCalendar currentCalendar];
   
    //设置时区
    self.datePicker.timeZone = [NSTimeZone defaultTimeZone];
   
    //选择时区
//    [self.datePicker setTimeZone:[NSTimeZone timeZoneWithName:@"GMT+8"]];
   
   
    // 设置当前显示时间
   
    NSDate *currentTime  = [NSDate date];

    self.datePicker.date = currentTime;
   
    // 设置显示最大时间
   
   self.datePicker.maximumDate = currentTime;
   
    // 显示模式
    [self.datePicker setDatePickerMode:UIDatePickerModeDate];
   
    //倒计时按秒显示
    self.datePicker.countDownDuration = 600;
   
    //设置分钟间隔
    self.datePicker.minuteInterval = 10;
   
    // 回调的方法
    [self.datePicker addTarget:self action:@selector(datePickerValueChanged:) forControlEvents:UIControlEventValueChanged];
   
   
    [self.view addSubview:self.datePicker];
   
}
   
-(void)datePickerValueChanged:(id)sender
{
    NSDate *selected = [NSDate date];
    NSLog(@"date: %@", selected);
}
```

