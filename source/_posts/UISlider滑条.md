---
title: UISlider滑条
date: 2016-10-08 11:39:43
categories: objective-c
---
UIDevice提供了多种属性、类函数及状态通知，帮助我们全方位了解设备状况。从检测电池电量到定位设备与临近感应，UIDevice所做的工作就是为应用程序提供用户及设备的一些信息。UIDevice类还能够收集关于设备的各种具体细节，例如机型及iOS版本等。其中大部分属性都对开发工作具有积极的辅助作用。下面的代码简单的使用UIDevice获取手机属性。
<!-- more -->

```objc
UISlider*slider = [[UISlideralloc]initWithFrame:CGRectMake(80,400,200,40)];
    [self.view addSubview:slider];
    [slider release];
    slider.backgroundColor = [UIColor lightGrayColor];
   
    //最小滑条颜色
    slider.minimumTrackTintColor = [UIColor redColor];
    //最大滑条颜色
    slider.maximumTrackTintColor = [UIColor greenColor];
    //设置滑块最小值

    //设置滑块最大值
    slider.maximumValue = 10;
    //默认的值
    slider.value = 5;
    //核心方法
    [slider addTarget:self action:@selector(sliderAction:) forControlEvents:UIControlEventValueChanged];
   

}
-(void)sliderAction:(UISlider *)slider
{

   
    NSLog(@"value = %f",slider.value);
}
```

