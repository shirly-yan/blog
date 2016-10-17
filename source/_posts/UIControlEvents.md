---
title: UIControlEvents
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
UIControlEventstypedefNS_OPTIONS(NSUInteger,
 UIControlEvents) {    UIControlEventTouchDown           =1<< 0,     // on all touch downs
    UIControlEventTouchDownRepeat     =1<< 1,     // on multiple touchdowns (tap count > 1)
    UIControlEventTouchDragInside     =1<< 2,
    UIControlEventTouchDragOutside    =1<< 3,
    UIControlEventTouchDragEnter      =1<< 4,
    UIControlEventTouchDragExit       =1<< 5,
    UIControlEventTouchUpInside       =1<< 6,
    UIControlEventTouchUpOutside      =1<< 7,
    UIControlEventTouchCancel         =1<< 8,

    UIControlEventValueChanged        =1<<12,    // sliders, etc.

    UIControlEventEditingDidBegin     =1<<16,    // UITextField
    UIControlEventEditingChanged      =1<<17,
    UIControlEventEditingDidEnd       =1<<18,
    UIControlEventEditingDidEndOnExit =1<<19,    // 'return key' ending editing

    UIControlEventAllTouchEvents      =0x00000FFF, // for touch events
    UIControlEventAllEditingEvents    =0x000F0000, // for UITextField
    UIControlEventApplicationReserved =0x0F000000, // range available for application use
    UIControlEventSystemReserved      =0xF0000000, // range reserved for internal framework use
    UIControlEventAllEvents           =0xFFFFFFFF};/*
 UIControlEventTouchDown 单点触摸按下事件：用户点触屏幕，或者又有新手指落下的时候。
 UIControlEventTouchDownRepeat 多点触摸按下事件，点触计数大于1：用户按下第二、三、或第四根手指的时候。
 UIControlEventTouchDragInside 当一次触摸在控件窗口内拖动时。
 UIControlEventTouchDragOutside 当一次触摸在控件窗口之外拖动时。
 UIControlEventTouchDragEnter 当一次触摸从控件窗口之外拖动到内部时。
 UIControlEventTouchDragExit
 当一次触摸从控件窗口内部拖动到外部时。
 
 UIControlEventTouchUpInside 所有在控件之内触摸抬起事件。
 UIControlEventTouchUpOutside 所有在控件之外触摸抬起事件(点触必须开始与控件内部才会发送通知)。
 UIControlEventTouchCancel 所有触摸取消事件，即一次触摸因为放上了太多手指而被取消，或者被上锁或者电话呼叫打断。
 UIControlEventTouchChanged 当控件的值发生改变时，发送通知。用于滑块、分段控件、以及其他取值的控件。你可以配置滑块控件何时发送通知，在滑块被放下时发送，或者在被拖动时发送。
 UIControlEventEditingDidBegin 当文本控件中开始编辑时发送通知。
 UIControlEventEditingChanged 当文本控件中的文本被改变时发送通知。
 UIControlEventEditingDidEnd 当文本控件中编辑结束时发送通知。
 UIControlEventEditingDidOnExit 当文本控件内通过按下回车键（或等价行为）结束编辑时，发送通知。
 UIControlEventAlltouchEvents 通知所有触摸事件。
 UIControlEventAllEditingEvents 通知所有关于文本编辑的事件。
 UIControlEventAllEvents
 通知所有事件。
       */
```