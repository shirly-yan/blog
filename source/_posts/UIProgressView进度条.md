---
title: UIProgressView进度条
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
  //创建
    UIProgressView *progressView = [[UIProgressView alloc]initWithProgressViewStyle:UIProgressViewStyleBar];
    [self.view addSubview:progressView];
    [progressView release];
   
    progressView.frame = CGRectMake(20, 100, 300, 10);
   
    //属性progress决定值的大小
    //设置初始值
    progressView.progress = 0;

     //利用NSTimer
     //创建NSTimer
    NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(timerAction:) userInfo:nil repeats:YES];
  

-(void)timerAction:(NSTimer *)timer
{
    if (self.progressView.progress < 1) {
         self.progressView.progress += 0.01;
    }
}
```

