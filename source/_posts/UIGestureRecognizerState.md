---
title: UIGestureRecognizerState
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
typedef NS_ENUM(NSInteger, UIGestureRecognizerState) {
    UIGestureRecognizerStatePossible,   // the recognizer has not yet recognized its gesture, but may be evaluating touch events. this is the default state
   
    UIGestureRecognizerStateBegan,      // the recognizer has received touches recognized as the gesture. the action method will be called at the next turn of the run loop
    UIGestureRecognizerStateChanged,    // the recognizer has received touches recognized as a change to the gesture. the action method will be called at the next turn of the run loop
    UIGestureRecognizerStateEnded,      // the recognizer has received touches recognized as the end of the gesture. the action method will be called at the next turn of the run loop and the recognizer will be reset to UIGestureRecognizerStatePossible
    UIGestureRecognizerStateCancelled,  // the recognizer has received touches resulting in the cancellation of the gesture. the action method will be called at the next turn of the run loop. the recognizer will be reset to UIGestureRecognizerStatePossible
   
    UIGestureRecognizerStateFailed,     // the recognizer has received a touch sequence that can not be recognized as the gesture. the action method will not be called and the recognizer will be reset to UIGestureRecognizerStatePossible
   
    // Discrete Gestures – gesture recognizers that recognize a discrete event but do not report changes (for example, a tap) do not transition through the Began and Changed states and can not fail or be cancelled
    UIGestureRecognizerStateRecognized = UIGestureRecognizerStateEnded // the recognizer has received touches recognized as the gesture. the action method will be called at the next turn of the run loop and the recognizer will be reset to UIGestureRecognizerStatePossible
};

Possible: 识别器在未识别出它的手势，但可能会接收到触摸时处于这个状态。这是默认状态。
Began: 识别器接收到触摸并识别出是它的手势时处于这个状态。响应方法将在下个循环步骤中被调用。

Changed:the recognizer has received touches recognized as a change to the gesture. （不懂怎么翻译，理解上就是识别器识别出一个变化为它的手势的触摸），响应方法将在下个循环步骤中被调用。

Ended:识别器在识别到作为当前手势结束信号的触摸时处于这个状态。响应方法将在下个循环步骤中被调用 并且 识别器将重置为possible状态。

Cancelled:识别器处于取消状态.响应方法将在下个循环步骤中被调用 并且 识别器将重置为possible状态。

Failed: 识别器接收到不能识别为它的手势的一系列触摸。响应方法不会被调用 并且 识别器将重置为possible状态。

Recognized: 识别器已识别到它的手势。响应方法将在下个循环步骤中被调用 并且 识别器将重置为possible状态。

```

