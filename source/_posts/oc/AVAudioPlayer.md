---
title: AVAudioPlayer
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
#import <AVFoundation/AVFoundation.h>

    //初始化三个按钮
   
    //play
    UIButton * button = [UIButton buttonWithType:UIButtonTypeCustom];
    [self.view addSubview:button];
    button.backgroundColor = [UIColor yellowColor];
    [button setFrame:CGRectMake(100, 100 + 64, 60, 40)];
    [button setTitle:@"play" forState:UIControlStateNormal];
    [button setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [button addTarget:self action:@selector(play) forControlEvents:UIControlEventTouchUpInside];

   
   
    //pause暂停
    UIButton * button1 = [UIButton buttonWithType:UIButtonTypeCustom];
    [self.view addSubview:button1];
    button1.backgroundColor = [UIColor yellowColor];
    [button1 setFrame:CGRectMake(100, 150 + 64, 60, 40)];
    [button1 setTitle:@"pause" forState:UIControlStateNormal];
    [button1 setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [button1 addTarget:self action:@selector(pause) forControlEvents:UIControlEventTouchUpInside];

   
   
    //stop
    UIButton * button2 = [UIButton buttonWithType:UIButtonTypeCustom];
    [self.view addSubview:button1];
    button2.backgroundColor = [UIColor yellowColor];
    [button2 setFrame:CGRectMake(100, 200 + 64, 60, 40)];
    [button2 setTitle:@"stop" forState:UIControlStateNormal];
    [button2 setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
    [button2 addTarget:self action:@selector(stop) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:button2];

   
   
   
    // 从bundle路径下读取音频文件
    NSString * string = [[NSBundle mainBundle] pathForResource:@"凭什么说-刘心" ofType:@"mp3"];
    // 把音频文件转换成url格式
    NSURL * url = [NSURL fileURLWithPath:string];
    // 初始化音频类 并且添加播放文件
    self.avAudioPlayer = [[AVAudioPlayer alloc] initWithContentsOfURL:url error:nil];
    // 设置代理
    [self.avAudioPlayer setDelegate:self];
    // 设置初始音量大小
    [self.avAudioPlayer setVolume:1];
    // 设置音乐播放次数 -1为一直循环
    [self.avAudioPlayer setNumberOfLoops:-1];
    // 预播放
    [self.avAudioPlayer prepareToPlay];
   
   
   
    // 初始化一个播放进度条
    self.progressV = [[UIProgressView alloc] initWithFrame:CGRectMake(80, 100, 200, 20)];
    [self.progressV setProgressTintColor:[UIColor redColor]];
    [self.progressV setTrackTintColor:[UIColor blueColor]];
    [self.view addSubview:self.progressV];
    [self.progressV release];
   
    // 用NSTimer来监控音频播放进度
    self.timer = [NSTimer scheduledTimerWithTimeInterval:0.1 target:self selector:@selector(playProgress) userInfo:nil repeats:YES];
   
   
   
    // 初始化音量控制
    self.volumeSlider = [[UISlider alloc] initWithFrame:CGRectMake(80, 130, 200, 20)];
    [self.volumeSlider addTarget:self action:@selector(volumeChange) forControlEvents:UIControlEventValueChanged];
    // 设置最小音量
    [self.volumeSlider setMinimumValue:0.0f];
    // 设置最大音量
    [self.volumeSlider setMaximumValue:10.0f];
    // 初始化音量为多少
    [self.volumeSlider setValue:5.0f];
    [self.view addSubview:self.volumeSlider];
    [self.volumeSlider release];
   
   
    //声音开关控件(静音)
    UISwitch *swith = [[UISwitch alloc] initWithFrame:CGRectMake(100, 500, 60, 40)];
    [self.view addSubview:swith];
    [swith addTarget:self action:@selector(onOrOff:) forControlEvents:UIControlEventValueChanged];
    //默认状态为打开
    swith.on = YES;
    [swith release];

/********************AVAudioPlayer***************/
//播放
- (void)play
{
    [self.avAudioPlayer play];
}
//暂停
- (void)pause
{
    [self.avAudioPlayer pause];
}
//停止
- (void)stop
{
    self.avAudioPlayer.currentTime = 0;  //当前播放时间设置为0
    [self.avAudioPlayer stop];
}
//播放进度条
- (void)playProgress
{
    //通过音频播放时长的百分比,给progressview进行赋值;
    self.progressV.progress = self.avAudioPlayer.currentTime/self.avAudioPlayer.duration;
}
//声音开关(是否静音)
- (void)onOrOff:(UISwitch *)sender
{
    self.avAudioPlayer.volume = sender.on;
}

//播放音量控制
- (void)volumeChange
{
    self.avAudioPlayer.volume = self.volumeSlider.value;
}

//播放完成时调用的方法  (代理里的方法),需要设置代理才可以调用
- (void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag
{
    [self.timer invalidate]; //NSTimer暂停  invalidate  使...无效;
}
/********************AVAudioPlayer***************/
```

