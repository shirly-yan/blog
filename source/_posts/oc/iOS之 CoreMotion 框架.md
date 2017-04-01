---
title: iOS之 CoreMotion 框架
date: 2015-10-08 11:39:46
categories: objective-c
---
 在项目中经常会使用到重力加速度，陀螺仪，最近在一个项目中使用到了CoreMotion 框架，需求是：上下摇摆手机控制智能椅子的角度调节，该项目是使用蓝牙4.0进行通讯的，在iOS内已经封装好了CoreBluetooth 框架，不过需要iPHone4s 以上的手机，和iPadmin以上的平板电脑才有蓝牙4.0.今天在这里先记录一下CoreMotion 框架的使用.
<!-- more -->

主要的类：CMMotionManager , NSOperationQueue.
下面是一个被我封装过的一个类。
项目需要导入CoreMotion.Framework 
在.h 文件需要#import <CoreMotion/CoreMotion.h>.h文件代码:

```objc
#import <Foundation/Foundation.h>
#import <CoreMotion/CoreMotion.h>
typedef enum _MotionDirection {UNKNOWDIRECTION=0,UP,DOWN}MotionDirection;
@interface iCtrlCMManager : NSObject
@property (nonatomic,retain) CMMotionManager * ictrlCMManager;
@property (nonatomic,assign) MotionDirection motionDirection;
+(id)sharedInstance;
-(void)startMotion;
-(void)stopMotion;
@end
.m文件代码:

#import "iCtrlCMManager.h"
 
@implementation iCtrlCMManager
#pragma mark -
#pragma mark life cycle
+(id)sharedInstance
{
    static iCtrlCMManager *this = nil;
    if(!this)
    {
        this = [[iCtrlCMManager alloc] init];
         
    }
     
    return this;
     
}
 
-(id)init
{
    self = [super init];
    if(self)
    {
 
        _ictrlCMManager = [[CMMotionManager alloc] init];
         
    }
    return self;
}
 
-(void)dealloc
{
    [_ictrlCMManager release];
    [super dealloc];
}
 
 
#pragma mark 
#pragma mark instance methods
-(void)startMotion
{
    _motionDirection = UNKNOWDIRECTION;
    if(_ictrlCMManager.accelerometerAvailable)
    {
        NSOperationQueue *queue = [[[NSOperationQueue alloc] init] autorelease];
        _ictrlCMManager.accelerometerUpdateInterval = 1.0/60.0;
 
        __block float y = 0.0;
        __block int i = 0;
        [_ictrlCMManager startAccelerometerUpdatesToQueue:queue withHandler:^(CMAccelerometerData *accelerometerData, NSError *error) {
            if (error)
            {
                NSLog(@"CoreMotion Error : %@",error);
                [_ictrlCMManager stopAccelerometerUpdates];
            }
            if(i == 0)
            {
                y = accelerometerData.acceleration.y;
                 
            }
             
             
            i++;
            if(fabs(y - accelerometerData.acceleration.y) > 0.4)
            {
                
  
                if(y > accelerometerData.acceleration.y)
                {
                    NSLog(@"UP");
                    _motionDirection = UP;
                }
                else
                {
                    NSLog(@"Down");
                    _motionDirection = DOWN;
                }
                 
                y = accelerometerData.acceleration.y;
        
            }
 
             
        }];
    }
    else
    {
        NSLog(@"The accelerometer is unavailable");
    }
}
 
-(void)stopMotion
{
    _motionDirection = UNKNOWDIRECTION;
    [_ictrlCMManager stopAccelerometerUpdates];
  
}
 
@end
```

