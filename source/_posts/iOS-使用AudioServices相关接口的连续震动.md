---
title: iOS-使用AudioServices相关接口的连续震动
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
转自：http://www.jianshu.com/p/dded314dd920
话不多说，先上代码！！！喜欢就点赞

主要功能函数
/*!
    @function       AudioServicesAddSystemSoundCompletion
    @abstract       Call the provided Completion Routine when provided SystemSoundID
                    finishes playing.
    @discussion     Once set, the System Sound server will send a message to the System Sound client
                    indicating which SystemSoundID has finished playing.
    @param          inSystemSoundID
                        systemSoundID 自定义的sound（1007系统默认提示音）或者kSystemSoundID_Vibrate（震动）
    @param          inRunLoop
                       没有研究 一般写NULL 有兴趣可以自己研究一下跟大家共享
    @param          inRunLoopMode
                        同上个属性
    @param          inCompletionRoutine
                        这个是指某次震动播放完成后的回调 注意是C的函数 一般我们会在回调中写播放震动的函数 来实现连续震动
    @param          inClientData
                        没有研究啦！！！NULL就行啦
*/
extern OSStatus 
AudioServicesAddSystemSoundCompletion(  SystemSoundID               inSystemSoundID,
                                     CFRunLoopRef                         inRunLoop,
                                     CFStringRef                          inRunLoopMode,
                                     AudioServicesSystemSoundCompletionProc  inCompletionRoutine,
                                     void*                                inClientData)       
首先实现上述函数中的回调函数（注意是C）
void soundCompleteCallback(SystemSoundID sound,void * clientData) {
    AudioServicesPlaySystemSound(kSystemSoundID_Vibrate);  //震动
    AudioServicesPlaySystemSound(sound);  // 播放系统声音 这里的sound是我自定义的，不要copy哈，没有的
}
实现播放声音或震动的代码
    SystemSoundID sound;
    NSString *path = [[NSBundle mainBundle] pathForResource:soundName ofType:nil];
    AudioServicesCreateSystemSoundID((__bridge CFURLRef)[NSURL fileURLWithPath:path], &_sound);

    AudioServicesAddSystemSoundCompletion(_soundID, NULL, NULL, soundCompleteCallback, NULL);
    AudioServicesPlaySystemSound(kSystemSoundID_Vibrate);
    AudioServicesPlaySystemSound(_sound);
至此，就可以顺利的播放声音和震动了，而且是连续的哦！！！
别忘了！ 怎么让他停下来
为了方便 我就写了而一个OC的方法来做了
-(void)stopAlertSoundWithSoundID:(SystemSoundID)sound {
    AudioServicesDisposeSystemSoundID(kSystemSoundID_Vibrate);
    AudioServicesDisposeSystemSoundID(sound);
    AudioServicesRemoveSystemSoundCompletion(sound);
}
这里要详细解说一下需要注意的事项：
AudioServicesAddSystemSoundCompletion(kSystemSoundID_Vibrate, NULL, NULL, systemAudioCallback, NULL);
AudioServicesRemoveSystemSoundCompletion(kSystemSoundID_Vibrate);
这两个接口的用途是绑定和取消指定soundID对应的回调方法；kSystemSoundID_Vibrate为soundID类型，其回调方法认准的也是这个soundID,在任何地方使用这个id去执行AudioServicesPlaySystemSound(xxxSoundID)都会调用到该回调方法。而一旦调用remove方法取消回调，同样的在任何地方使用这个id去执行AudioServicesPlaySystemSound(xxxSoundID)都不会调用到这个回调。说的这么绕，其实就是说这俩接口的影响是全局的，威力很大。
我们只要在回调方法里面再调用AudioServicesPlaySystemSound接口，就可以实现连续震动了；当我们想要停止震动时，调用remove接口，ok，回调方法就歇火了。

优化：（参考某大神的博客，名字太长，直接附链接）
http://blog.csdn.net/openglnewbee/article/details/8494598
经过测试发现震动之间太连续，体验不符合要求；所以我们在c回调里面通过单例（全局变量性质的指针）调用到oc的方法进行[self performSelector:@selector(triggerShake) withObject:nil afterDelay:1]（triggerShake是震动接口);在停止震动时候我们需要调用

[NSObject cancelPreviousPerformRequestsWithTarget:self selector:@selector(triggerShake)  object:nil];
停止之前可能的回调；这两个方法的成对使用既好用又简便，对于需要定时调用的场景很适合，也免去维护定时器的麻烦。

这个时候屏幕要是常亮就更好了，不用费脑子了，用这个！！！
[[UIApplication sharedApplication] setIdleTimerDisabled:YES];   // 设置播放时屏幕常亮
```

