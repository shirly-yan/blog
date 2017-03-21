---
title: UIDevice
date: 2016-10-08 11:39:43
categories: objective-c
---
UIDevice提供了多种属性、类函数及状态通知，帮助我们全方位了解设备状况。从检测电池电量到定位设备与临近感应，UIDevice所做的工作就是为应用程序提供用户及设备的一些信息。UIDevice类还能够收集关于设备的各种具体细节，例如机型及iOS版本等。其中大部分属性都对开发工作具有积极的辅助作用。下面的代码简单的使用UIDevice获取手机属性。
<!-- more -->
 typedef NS_ENUM(NSInteger, UIDeviceOrientation) //设备方向
    {
        UIDeviceOrientationUnknown,
        UIDeviceOrientationPortrait,                   // 竖向，头向上
        UIDeviceOrientationPortraitUpsideDown,  // 竖向，头向下
        UIDeviceOrientationLandscapeLeft,         // 横向，头向左
        UIDeviceOrientationLandscapeRight,       // 横向，头向右
        UIDeviceOrientationFaceUp,                   // 平放，屏幕朝下
        UIDeviceOrientationFaceDown                // 平放，屏幕朝下
    };
   
    typedef NS_ENUM(NSInteger, UIDeviceBatteryState) //电池状态
    {
        UIDeviceBatteryStateUnknown,
        UIDeviceBatteryStateUnplugged,   // 未充电
        UIDeviceBatteryStateCharging,     // 正在充电
        UIDeviceBatteryStateFull,             // 满电
    };
   
    typedef NS_ENUM(NSInteger, UIUserInterfaceIdiom) //用户界面类型
    {
        //iOS3.2以上有效
#if __IPHONE_3_2 <= __IPHONE_OS_VERSION_MAX_ALLOWED
        UIUserInterfaceIdiomPhone,           // iPhone 和 iPod touch 风格
        UIUserInterfaceIdiomPad,              // iPad 风格
#endif
    };
   
#define UI_USER_INTERFACE_IDIOM() ([[UIDevice currentDevice] respondsToSelector:@selector(userInterfaceIdiom)] ? [[UIDevice currentDevice] userInterfaceIdiom] : UIUserInterfaceIdiomPhone)
   
#define UIDeviceOrientationIsPortrait(orientation)  ((orientation) == UIDeviceOrientationPortrait || (orientation) == UIDeviceOrientationPortraitUpsideDown)
#define UIDeviceOrientationIsLandscape(orientation) ((orientation) == UIDeviceOrientationLandscapeLeft || (orientation) == UIDeviceOrientationLandscapeRight)
   
    NS_CLASS_AVAILABLE_IOS(2_0) @interface UIDevice : NSObject {
    @private
        NSInteger _numDeviceOrientationObservers;
        float    _batteryLevel;
        struct {
            unsigned int batteryMonitoringEnabled:1;
            unsigned int proximityMonitoringEnabled:1;
            unsigned int expectsFaceContactInLandscape:1;
            unsigned int orientation:3;
            unsigned int batteryState:2;
            unsigned int proximityState:1;
        } _deviceFlags;
    }
   
    + (UIDevice *)currentDevice; // 获取当前设备
   
    @property(nonatomic,readonly,retain) NSString    *name;                // e.g. "My iPhone"
    @property(nonatomic,readonly,retain) NSString    *model;               // e.g. @"iPhone", @"iPod touch"
    @property(nonatomic,readonly,retain) NSString    *localizedModel;    // localized version of model
    @property(nonatomic,readonly,retain) NSString    *systemName;      // e.g. @"iOS"
    @property(nonatomic,readonly,retain) NSString    *systemVersion;    // e.g. @"4.0"
    @property(nonatomic,readonly) UIDeviceOrientation orientation;       // 除非正在生成设备方向的通知，否则返回UIDeviceOrientationUnknown 。
   
    @property(nonatomic,readonly,retain) NSUUID      *identifierForVendor NS_AVAILABLE_IOS(6_0);      // 可用于唯一标识该设备，同一供应商不同应用具有相同的UUID 。
   
    @property(nonatomic,readonly,getter=isGeneratingDeviceOrientationNotifications) BOOL generatesDeviceOrientationNotifications; //是否生成设备转向通知
    - (void)beginGeneratingDeviceOrientationNotifications;
    - (void)endGeneratingDeviceOrientationNotifications;
   
    @property(nonatomic,getter=isBatteryMonitoringEnabled) BOOL batteryMonitoringEnabled NS_AVAILABLE_IOS(3_0);  // 是否启动电池监控，默认为NO
    @property(nonatomic,readonly) UIDeviceBatteryState batteryState NS_AVAILABLE_IOS(3_0);  // 如果禁用电池监控，则电池状态为UIDeviceBatteryStateUnknown
    @property(nonatomic,readonly) float batteryLevel NS_AVAILABLE_IOS(3_0);  //电量百分比， 0 .. 1.0。如果电池状态为UIDeviceBatteryStateUnknown，则百分比为-1.0
   
    @property(nonatomic,getter=isProximityMonitoringEnabled) BOOL proximityMonitoringEnabled NS_AVAILABLE_IOS(3_0); // 是否启动接近监控（例如接电话时脸靠近屏幕），默认为NO
    @property(nonatomic,readonly)  BOOL proximityState NS_AVAILABLE_IOS(3_0);  // 如果设备不具备接近感应器，则总是返回NO
   
    @property(nonatomic,readonly,getter=isMultitaskingSupported) BOOL multitaskingSupported NS_AVAILABLE_IOS(4_0); // 是否支持多任务
   
    @property(nonatomic,readonly) UIUserInterfaceIdiom userInterfaceIdiom NS_AVAILABLE_IOS(3_2); // 当前用户界面模式
   
    - (void)playInputClick NS_AVAILABLE_IOS(4_2);  // 播放一个输入的声音
    @end
   
    @protocol UIInputViewAudioFeedback
    @optional
    @property (nonatomic, readonly) BOOL enableInputClicksWhenVisible; // 实现该方法，返回YES则自定义的视图能够播放输入的声音
    @end
   
    UIKIT_EXTERN NSString *const UIDeviceOrientationDidChangeNotification; // 屏幕方向变化通知
    UIKIT_EXTERN NSString *const UIDeviceBatteryStateDidChangeNotification   NS_AVAILABLE_IOS(3_0); // 电池状态变化通知
    UIKIT_EXTERN NSString *const UIDeviceBatteryLevelDidChangeNotification   NS_AVAILABLE_IOS(3_0); // 电池电量变化通知
    UIKIT_EXTERN NSString *const UIDeviceProximityStateDidChangeNotification NS_AVAILABLE_IOS(3_0); // 接近状态变化通知
```

