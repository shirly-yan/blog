---
title: NSTimer
date: 2016-10-08 11:39:43
categories: objective-c
---
NSTimer
<!-- more -->

```objc
    //创建NSTimerNSTimer*timer = [[NSTimeralloc]init];      //每1秒运行一次function方法。    timer =  [NSTimer scheduledTimerWithTimeInterval:1.0target:selfselector:@selector(function:) userInfo:nilrepeats:YES];   //TimeInterval:调用的时间间隔   //repeats:决定是否重复调NSTimer   //注意：将计数器的repeats设置为YES的时候，self的引用计数会加1。因此可能会导致self（即viewController）不能release，所以，必须在viewWillAppear的时候，将计数器timer停止，否则可能会导致内存泄露。   //停止timer的运行，但这个是永久的停止：（注意：停止后，一定要将timer赋空，否则还是没有释放。
   //取消定时器
    [timer invalidate];    timer =nil;
   //要想实现：先停止，然后再某种情况下再次开启运行timer，可以使用下面的方法：
   //暂时关闭定时器    [myTimer setFireDate:[NSDatedistantFuture]];
   //开启定时器    [myTimer setFireDate:[NSDatedistantPast]];

   //设置启动页面时间    NSThread sleepForTimeInterval:<#(NSTimeInterval)#>
/* NSTimer.hCopyright (c) 1994-2014, Apple Inc. All rights reserved.
*/

#import<Foundation/NSObject.h>
#import<Foundation/NSDate.h>

@interfaceNSTimer :NSObject

+ (NSTimer*)timerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation*)invocation repeats:(BOOL)yesOrNo;
+ (NSTimer*)scheduledTimerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation*)invocation repeats:(BOOL)yesOrNo;

+ (NSTimer*)timerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget
 selector:(SEL)aSelector userInfo:(id)userInfo
 repeats:(BOOL)yesOrNo;
+ (NSTimer*)scheduledTimerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget
 selector:(SEL)aSelector userInfo:(id)userInfo
 repeats:(BOOL)yesOrNo;

- (instancetype)initWithFireDate:(NSDate*)date interval:(NSTimeInterval)ti target:(id)t
 selector:(SEL)s userInfo:(id)ui
 repeats:(BOOL)repNS_DESIGNATED_INITIALIZER;

- (void)fire;

@property(copy)NSDate*fireDate;
@property(readonly)NSTimeIntervaltimeInterval;

// Setting a tolerance for a timer allows it to fire later than the scheduled fire date, improving the ability of the system to optimize for increased power savings and responsiveness.
 The timer may fire at any time between its scheduled fire date and the scheduled fire date plus the tolerance. The timer will not fire before the scheduled fire date. For repeating timers, the next fire date is calculated from the original fire date regardless
 of tolerance applied at individual fire times, to avoid drift. The default value is zero, which means no additional tolerance is applied. The system reserves the right to apply a small amount of tolerance to certain timers regardless of the value of this property.
// As the user of the timer, you will have the best idea of what an appropriate tolerance for a timer may be. A general rule of thumb, though, is to set the tolerance to at least 10%
 of the interval, for a repeating timer. Even a small amount of tolerance will have a significant positive impact on the power usage of your application. The system may put a maximum value of the tolerance.
@propertyNSTimeIntervaltoleranceNS_AVAILABLE(10_9,7_0);

- (void)invalidate;
@property(readonly,getter=isValid)BOOLvalid;

@property(readonly,retain)iduserInfo;

@end