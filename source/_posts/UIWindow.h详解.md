---
title: UIWindow.h详解
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
#import <Foundation/Foundation.h>//基础框架入口

#import <CoreGraphics/CoreGraphics.h>//绘图入口

#import <UIKit/UIView.h>//视图对象

#import <UIKit/UIApplication.h>//提供iOS程序运行期的协作和控制

#import <UIKit/UIKitDefines.h>//一些宏定义



NS_ASSUME_NONNULL_BEGIN 



typedef CGFloat UIWindowLevel;//32位则为float | 64位为double

/*

  UIEvent 触摸事件，运动事件

  UIScreen 屏幕处理

  NSUndoManager  记录撤销，修改操作的消息

  UIViewController 视图控制器

*/

@class UIEvent,UIScreen, NSUndoManager,UIViewController;



NS_CLASS_AVAILABLE_IOS(2_0)@interface UIWindow :UIView



@property(nonatomic,strong)UIScreen *screen; 

@property(nonatomic)UIWindowLevel windowLevel;            

@property(nonatomic,readonly,getter=isKeyWindow) 

BOOL keyWindow;

- (void)becomeKeyWindow;                               

- (void)resignKeyWindow;                               

- (void)makeKeyWindow;

- (void)makeKeyAndVisible;                             

@property(nullable,nonatomic,strong)UIViewController *rootViewController NS_AVAILABLE_IOS(4_0);  // default is nil



/*

 事件拦截分发到指定视图对象

 当用户发起一个事件，比如触摸屏幕或者晃动设备，系统产生一个事件，同时投递给UIApplication，而UIApplication则将这个事件传递给特定

 UIWindow进行处理(正常情况都一个程序都只有一个UIWindow)，然后由UIWindow将这个事件传递给特定的对象(即first responder)并通过响应链

 进行处理。虽然都是通过响应链对事件进行处理，但是触摸事件和运动事件在处理上有着明显的不同(主要体现在确定哪个对象才是他们的first

 responder)：

*/

- (void)sendEvent:(UIEvent *)event; 

                   

//窗口坐标系统转化

- (CGPoint)convertPoint:(CGPoint)point toWindow:(nullableUIWindow *)window;//转化当前窗口一个坐标相对另外一个窗口的坐标

- (CGPoint)convertPoint:(CGPoint)point fromWindow:(nullableUIWindow *)window;//转化另外窗口一个坐标相对于当前窗口的坐标

- (CGRect)convertRect:(CGRect)rect toWindow:(nullableUIWindow *)window;//转化当前窗口一个矩形坐标相对另外一个窗口的坐标

- (CGRect)convertRect:(CGRect)rect fromWindow:(nullableUIWindow *)window;//转化另外窗口一个矩形坐标相对于当前窗口的坐标



@end



UIKIT_EXTERN constUIWindowLevel UIWindowLevelNormal;//默认等级

UIKIT_EXTERN constUIWindowLevel UIWindowLevelAlert;//UIAlert等级

UIKIT_EXTERN constUIWindowLevel UIWindowLevelStatusBar;//状态栏等级



UIKIT_EXTERN NSString *const UIWindowDidBecomeVisibleNotification;// nil 通知对象窗口为可见

UIKIT_EXTERN NSString *const UIWindowDidBecomeHiddenNotification; // nil 通知对象窗口为隐藏

UIKIT_EXTERN NSString *const UIWindowDidBecomeKeyNotification;    // nil 通知对象窗口为重要

UIKIT_EXTERN NSString *const UIWindowDidResignKeyNotification;    // nil 通知对象窗口取消主窗



//当键盘显示或消失时，系统会发送相关的通知:

UIKIT_EXTERN NSString *const UIKeyboardWillShowNotification;//通知键盘对象视图即将加载

UIKIT_EXTERN NSString *const UIKeyboardDidShowNotification;//通知键盘对象视图完全加载

UIKIT_EXTERN NSString *const UIKeyboardWillHideNotification;//通知键盘对象视图即将隐藏

UIKIT_EXTERN NSString *const UIKeyboardDidHideNotification;//通知键盘对象视图完全隐藏



/*

通知消息 NSNotification中的 userInfo字典中包含键盘的位置和大小信息，对应的key为

  UIKeyboardFrameBeginUserInfoKey

  UIKeyboardFrameEndUserInfoKey

  UIKeyboardAnimationDurationUserInfoKey

  UIKeyboardAnimationCurveUserInfoKey

UIKeyboardFrameBeginUserInfoKey,UIKeyboardFrameEndUserInfoKey对应的Value是个NSValue对象，内部包含CGRect结构，分别为键盘起始时和终止时的位置信息。

UIKeyboardAnimationCurveUserInfoKey对应的Value是NSNumber对象，内部为UIViewAnimationCurve类型的数据，表示键盘显示或消失的动画类型。

UIKeyboardAnimationDurationUserInfoKey对应的Value也是NSNumber对象，内部为double类型的数据，表示键盘h显示或消失时动画的持续时间



 例如，在UIKeyboardWillShowNotification，UIKeyboardDidShowNotification通知中的userInfo内容为

    userInfo = {

                 UIKeyboardAnimationCurveUserInfoKey = 0;

                 UIKeyboardAnimationDurationUserInfoKey = "0.25";

                 UIKeyboardBoundsUserInfoKey = "NSRect: {{0, 0}, {320, 216}}";

                 UIKeyboardCenterBeginUserInfoKey = "NSPoint: {160, 588}";

                 UIKeyboardCenterEndUserInfoKey = "NSPoint: {160, 372}";

                 UIKeyboardFrameBeginUserInfoKey = "NSRect: {{0, 480}, {320, 216}}";

                 UIKeyboardFrameChangedByUserInteraction = 0;

                 UIKeyboardFrameEndUserInfoKey = "NSRect: {{0, 264}, {320, 216}}";

               }

    在UIKeyboardWillHideNotification,UIKeyboardDidHideNotification通知中的userInfo内容为:

   userInfo = {

                 UIKeyboardAnimationCurveUserInfoKey = 0;

                 UIKeyboardAnimationDurationUserInfoKey = "0.25";

                 UIKeyboardBoundsUserInfoKey = "NSRect: {{0, 0}, {320, 216}}";

                 UIKeyboardCenterBeginUserInfoKey = "NSPoint: {160, 372}";

                 UIKeyboardCenterEndUserInfoKey = "NSPoint: {160, 588}";

                 UIKeyboardFrameBeginUserInfoKey = "NSRect: {{0, 264}, {320, 216}}";

                 UIKeyboardFrameChangedByUserInteraction = 0;

                 UIKeyboardFrameEndUserInfoKey = "NSRect: {{0, 480}, {320, 216}}";

              }

*/

UIKIT_EXTERN NSString *const UIKeyboardFrameBeginUserInfoKey       NS_AVAILABLE_IOS(3_2);// NSValue of CGRect

UIKIT_EXTERN NSString *const UIKeyboardFrameEndUserInfoKey         NS_AVAILABLE_IOS(3_2);// NSValue of CGRect

UIKIT_EXTERN NSString *const UIKeyboardAnimationDurationUserInfoKeyNS_AVAILABLE_IOS(3_0);// NSNumber of double

UIKIT_EXTERN NSString *const UIKeyboardAnimationCurveUserInfoKey   NS_AVAILABLE_IOS(3_0);// NSNumber of NSUInteger (UIViewAnimationCurve)

UIKIT_EXTERN NSString *const UIKeyboardIsLocalUserInfoKey          NS_AVAILABLE_IOS(9_0);// NSNumber of BOOL

UIKIT_EXTERN NSString *const UIKeyboardWillChangeFrameNotification NS_AVAILABLE_IOS(5_0);//键盘即将改变布局发出通知

UIKIT_EXTERN NSString *const UIKeyboardDidChangeFrameNotification  NS_AVAILABLE_IOS(5_0);//键盘已经改变布局后发出通知

UIKIT_EXTERN NSString *const UIKeyboardCenterBeginUserInfoKey  NS_DEPRECATED_IOS(2_0,3_2);

UIKIT_EXTERN NSString *const UIKeyboardCenterEndUserInfoKey    NS_DEPRECATED_IOS(2_0,3_2);

UIKIT_EXTERN NSString *const UIKeyboardBoundsUserInfoKey       NS_DEPRECATED_IOS(2_0,3_2);



NS_ASSUME_NONNULL_END

```

