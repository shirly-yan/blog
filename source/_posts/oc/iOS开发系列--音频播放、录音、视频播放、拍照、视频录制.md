---
title: iOS开发系列--音频播放、录音、视频播放、拍照、视频录制
date: 2016-10-08 11:39:43
categories: objective-c
---
在iOS中音频播放从形式上可以分为音效播放和音乐播放。前者主要指的是一些短音频播放，通常作为点缀音频，对于这类音频不需要进行进度、循环等控制。后者指的是一些较长的音频，通常是主音频，对于这些音频的播放通常需要进行精确的控制。
<!-- more -->

```objc
转自：http://blog.csdn.net/jianxin160/article/details/47753241
音频
在iOS中音频播放从形式上可以分为音效播放和音乐播放。前者主要指的是一些短音频播放，通常作为点缀音频，对于这类音频不需要进行进度、循环等控制。后者指的是一些较长的音频，通常是主音频，对于这些音频的播放通常需要进行精确的控制。在iOS中播放两类音频分别使用AudioToolbox.framework和AVFoundation.framework来完成音效和音乐播放。
音效
AudioToolbox.framework是一套基于C语言的框架，使用它来播放音效其本质是将短音频注册到系统声音服务（System Sound Service）。System Sound Service是一种简单、底层的声音播放服务，但是它本身也存在着一些限制：
音频播放时间不能超过30s
数据必须是PCM或者IMA4格式
音频文件必须打包成.caf、.aif、.wav中的一种（注意这是官方文档的说法，实际测试发现一些.mp3也可以播放）
使用System Sound Service 播放音效的步骤如下：
调用AudioServicesCreateSystemSoundID(   CFURLRef  inFileURL, SystemSoundID*   outSystemSoundID)函数获得系统声音ID。
如果需要监听播放完成操作，则使用AudioServicesAddSystemSoundCompletion(  SystemSoundID inSystemSoundID,
CFRunLoopRef  inRunLoop, CFStringRef  inRunLoopMode, AudioServicesSystemSoundCompletionProc  inCompletionRoutine, void*  inClientData)方法注册回调函数。
调用AudioServicesPlaySystemSound(SystemSoundID inSystemSoundID) 或者AudioServicesPlayAlertSound(SystemSoundID inSystemSoundID) 方法播放音效（后者带有震动效果）。
下面是一个简单的示例程序：
//
//  KCMainViewController.m
//  Audio
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//  音效播放

#import "KCMainViewController.h"
#import <AudioToolbox/AudioToolbox.h>

@interface KCMainViewController ()

@end

@implementation KCMainViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self playSoundEffect:@"videoRing.caf"];
}

/**
 *  播放完成回调函数
 *
 *  @param soundID    系统声音ID
 *  @param clientData 回调时传递的数据
 */
void soundCompleteCallback(SystemSoundID soundID,void * clientData){
    NSLog(@"播放完成...");
}

/**
 *  播放音效文件
 *
 *  @param name 音频文件名称
 */
-(void)playSoundEffect:(NSString *)name{
    NSString *audioFile=[[NSBundle mainBundle] pathForResource:name ofType:nil];
    NSURL *fileUrl=[NSURL fileURLWithPath:audioFile];
    //1.获得系统声音ID
    SystemSoundID soundID=0;
    /**
     * inFileUrl:音频文件url
     * outSystemSoundID:声音id（此函数会将音效文件加入到系统音频服务中并返回一个长整形ID）
     */
    AudioServicesCreateSystemSoundID((__bridge CFURLRef)(fileUrl), &soundID);
    //如果需要在播放完之后执行某些操作，可以调用如下方法注册一个播放完成回调函数
    AudioServicesAddSystemSoundCompletion(soundID, NULL, NULL, soundCompleteCallback, NULL);
    //2.播放音频
    AudioServicesPlaySystemSound(soundID);//播放音效
//    AudioServicesPlayAlertSound(soundID);//播放音效并震动
}

@end
音乐
如果播放较大的音频或者要对音频有精确的控制则System Sound Service可能就很难满足实际需求了，通常这种情况会选择使用AVFoundation.framework中的AVAudioPlayer来实现。AVAudioPlayer可以看成一个播放器，它支持多种音频格式，而且能够进行进度、音量、播放速度等控制。首先简单看一下AVAudioPlayer常用的属性和方法：
属性	说明
@property(readonly, getter=isPlaying) BOOL playing	是否正在播放，只读
@property(readonly) NSUInteger numberOfChannels	音频声道数，只读
@property(readonly) NSTimeInterval duration	音频时长
@property(readonly) NSURL *url	音频文件路径，只读
@property(readonly) NSData *data	音频数据，只读
@property float pan	立体声平衡，如果为-1.0则完全左声道，如果0.0则左右声道平衡，如果为1.0则完全为右声道
@property float volume	音量大小，范围0-1.0
@property BOOL enableRate	是否允许改变播放速率
@property float rate	播放速率，范围0.5-2.0，如果为1.0则正常播放，如果要修改播放速率则必须设置enableRate为YES
@property NSTimeInterval currentTime	当前播放时长
@property(readonly) NSTimeInterval deviceCurrentTime	输出设备播放音频的时间，注意如果播放中被暂停此时间也会继续累加
@property NSInteger numberOfLoops	循环播放次数，如果为0则不循环，如果小于0则无限循环，大于0则表示循环次数
@property(readonly) NSDictionary *settings	音频播放设置信息，只读
@property(getter=isMeteringEnabled) BOOL meteringEnabled	是否启用音频测量，默认为NO，一旦启用音频测量可以通过updateMeters方法更新测量值
对象方法	说明
- (instancetype)initWithContentsOfURL:(NSURL *)url error:(NSError **)outError	使用文件URL初始化播放器，注意这个URL不能是HTTP URL，AVAudioPlayer不支持加载网络媒体流，只能播放本地文件
- (instancetype)initWithData:(NSData *)data error:(NSError **)outError	使用NSData初始化播放器，注意使用此方法时必须文件格式和文件后缀一致，否则出错，所以相比此方法更推荐使用上述方法或- (instancetype)initWithData:(NSData *)data fileTypeHint:(NSString *)utiString error:(NSError **)outError方法进行初始化
- (BOOL)prepareToPlay;	加载音频文件到缓冲区，注意即使在播放之前音频文件没有加载到缓冲区程序也会隐式调用此方法。
- (BOOL)play;	播放音频文件
- (BOOL)playAtTime:(NSTimeInterval)time	在指定的时间开始播放音频
- (void)pause;	暂停播放
- (void)stop;	停止播放
- (void)updateMeters	更新音频测量值，注意如果要更新音频测量值必须设置meteringEnabled为YES，通过音频测量值可以即时获得音频分贝等信息
- (float)peakPowerForChannel:(NSUInteger)channelNumber;	获得指定声道的分贝峰值，注意如果要获得分贝峰值必须在此之前调用updateMeters方法
- (float)averagePowerForChannel:(NSUInteger)channelNumber	获得指定声道的分贝平均值，注意如果要获得分贝平均值必须在此之前调用updateMeters方法
@property(nonatomic, copy) NSArray *channelAssignments	获得或设置播放声道
代理方法	说明
- (void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag	音频播放完成
- (void)audioPlayerDecodeErrorDidOccur:(AVAudioPlayer *)player error:(NSError *)error	音频解码发生错误
AVAudioPlayer的使用比较简单：
初始化AVAudioPlayer对象，此时通常指定本地文件路径。
设置播放器属性，例如重复次数、音量大小等。
调用play方法播放。
下面就使用AVAudioPlayer实现一个简单播放器，在这个播放器中实现了播放、暂停、显示播放进度功能，当然例如调节音量、设置循环模式、甚至是声波图像（通过分析音频分贝值）等功能都可以实现，这里就不再一一演示。界面效果如下：
AudioPlayerScreen
当然由于AVAudioPlayer一次只能播放一个音频文件，所有上一曲、下一曲其实可以通过创建多个播放器对象来完成，这里暂不实现。播放进度的实现主要依靠一个定时器实时计算当前播放时长和音频总时长的比例，另外为了演示委托方法，下面的代码中也实现了播放完成委托方法，通常如果有下一曲功能的话播放完可以触发下一曲音乐播放。下面是主要代码：
//
//  ViewController.m
//  KCAVAudioPlayer
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
#define kMusicFile @"刘若英 - 原来你也在这里.mp3"
#define kMusicSinger @"刘若英"
#define kMusicTitle @"原来你也在这里"

@interface ViewController ()<AVAudioPlayerDelegate>

@property (nonatomic,strong) AVAudioPlayer *audioPlayer;//播放器
@property (weak, nonatomic) IBOutlet UILabel *controlPanel; //控制面板
@property (weak, nonatomic) IBOutlet UIProgressView *playProgress;//播放进度
@property (weak, nonatomic) IBOutlet UILabel *musicSinger; //演唱者
@property (weak, nonatomic) IBOutlet UIButton *playOrPause; //播放/暂停按钮(如果tag为0认为是暂停状态，1是播放状态)

@property (weak ,nonatomic) NSTimer *timer;//进度更新定时器

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    [self setupUI];
    
}

/**
 *  初始化UI
 */
-(void)setupUI{
    self.title=kMusicTitle;
    self.musicSinger.text=kMusicSinger;
}

-(NSTimer *)timer{
    if (!_timer) {
        _timer=[NSTimer scheduledTimerWithTimeInterval:0.5 target:self selector:@selector(updateProgress) userInfo:nil repeats:true];
    }
    return _timer;
}

/**
 *  创建播放器
 *
 *  @return 音频播放器
 */
-(AVAudioPlayer *)audioPlayer{
    if (!_audioPlayer) {
        NSString *urlStr=[[NSBundle mainBundle]pathForResource:kMusicFile ofType:nil];
        NSURL *url=[NSURL fileURLWithPath:urlStr];
        NSError *error=nil;
        //初始化播放器，注意这里的Url参数只能时文件路径，不支持HTTP Url
        _audioPlayer=[[AVAudioPlayer alloc]initWithContentsOfURL:url error:&error];
        //设置播放器属性
        _audioPlayer.numberOfLoops=0;//设置为0不循环
        _audioPlayer.delegate=self;
        [_audioPlayer prepareToPlay];//加载音频文件到缓存
        if(error){
            NSLog(@"初始化播放器过程发生错误,错误信息:%@",error.localizedDescription);
            return nil;
        }
    }
    return _audioPlayer;
}

/**
 *  播放音频
 */
-(void)play{
    if (![self.audioPlayer isPlaying]) {
        [self.audioPlayer play];
        self.timer.fireDate=[NSDate distantPast];//恢复定时器
    }
}

/**
 *  暂停播放
 */
-(void)pause{
    if ([self.audioPlayer isPlaying]) {
        [self.audioPlayer pause];
        self.timer.fireDate=[NSDate distantFuture];//暂停定时器，注意不能调用invalidate方法，此方法会取消，之后无法恢复
        
    }
}

/**
 *  点击播放/暂停按钮
 *
 *  @param sender 播放/暂停按钮
 */
- (IBAction)playClick:(UIButton *)sender {
    if(sender.tag){
        sender.tag=0;
        [sender setImage:[UIImage imageNamed:@"playing_btn_play_n"] forState:UIControlStateNormal];
        [sender setImage:[UIImage imageNamed:@"playing_btn_play_h"] forState:UIControlStateHighlighted];
        [self pause];
    }else{
        sender.tag=1;
        [sender setImage:[UIImage imageNamed:@"playing_btn_pause_n"] forState:UIControlStateNormal];
        [sender setImage:[UIImage imageNamed:@"playing_btn_pause_h"] forState:UIControlStateHighlighted];
        [self play];
    }
}

/**
 *  更新播放进度
 */
-(void)updateProgress{
    float progress= self.audioPlayer.currentTime /self.audioPlayer.duration;
    [self.playProgress setProgress:progress animated:true];
}

#pragma mark - 播放器代理方法
-(void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag{
    NSLog(@"音乐播放完成...");
}

@end
运行效果：
AVAudioPlayer
音频会话
事实上上面的播放器还存在一些问题，例如通常我们看到的播放器即使退出到后台也是可以播放的，而这个播放器如果退出到后台它会自动暂停。如果要支持后台播放需要做下面几件事情：
1.设置后台运行模式：在plist文件中添加Required background modes，并且设置item 0=App plays audio or streams audio/video using AirPlay（其实可以直接通过Xcode在Project Targets-Capabilities-Background Modes中设置）
BackgroundModes
2.设置AVAudioSession的类型为AVAudioSessionCategoryPlayback并且调用setActive::方法启动会话。
    AVAudioSession *audioSession=[AVAudioSession sharedInstance];
    [audioSession setCategory:AVAudioSessionCategoryPlayback error:nil];
    [audioSession setActive:YES error:nil];
3.为了能够让应用退到后台之后支持耳机控制，建议添加远程控制事件（这一步不是后台播放必须的）
前两步是后台播放所必须设置的，第三步主要用于接收远程事件，这部分内容之前的文章中有详细介绍，如果这一步不设置虽让也能够在后台播放，但是无法获得音频控制权（如果在使用当前应用之前使用其他播放器播放音乐的话，此时如果按耳机播放键或者控制中心的播放按钮则会播放前一个应用的音频），并且不能使用耳机进行音频控制。第一步操作相信大家都很容易理解，如果应用程序要允许运行到后台必须设置，正常情况下应用如果进入后台会被挂起，通过该设置可以上应用程序继续在后台运行。但是第二步使用的AVAudioSession有必要进行一下详细的说明。
在iOS中每个应用都有一个音频会话，这个会话就通过AVAudioSession来表示。AVAudioSession同样存在于AVFoundation框架中，它是单例模式设计，通过sharedInstance进行访问。在使用Apple设备时大家会发现有些应用只要打开其他音频播放就会终止，而有些应用却可以和其他应用同时播放，在多种音频环境中如何去控制播放的方式就是通过音频会话来完成的。下面是音频会话的几种会话模式：
会话类型	说明	是否要求输入	是否要求输出	是否遵从静音键
AVAudioSessionCategoryAmbient	混音播放，可以与其他音频应用同时播放	否	是	是
AVAudioSessionCategorySoloAmbient	独占播放	否	是	是
AVAudioSessionCategoryPlayback	后台播放，也是独占的	否	是	否
AVAudioSessionCategoryRecord	录音模式，用于录音时使用	是	否	否
AVAudioSessionCategoryPlayAndRecord	播放和录音，此时可以录音也可以播放	是	是	否
AVAudioSessionCategoryAudioProcessing	硬件解码音频，此时不能播放和录制	否	否	否
AVAudioSessionCategoryMultiRoute	多种输入输出，例如可以耳机、USB设备同时播放	是	是	否
注意：是否遵循静音键表示在播放过程中如果用户通过硬件设置为静音是否能关闭声音。
根据前面对音频会话的理解，相信大家开发出能够在后台播放的音频播放器并不难，但是注意一下，在前面的代码中也提到设置完音频会话类型之后需要调用setActive::方法将会话激活才能起作用。类似的，如果一个应用已经在播放音频，打开我们的应用之后设置了在后台播放的会话类型，此时其他应用的音频会停止而播放我们的音频，如果希望我们的程序音频播放完之后（关闭或退出到后台之后）能够继续播放其他应用的音频的话则可以调用setActive::方法关闭会话。代码如下：
//
//  ViewController.m
//  KCAVAudioPlayer
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//  AVAudioSession 音频会话

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
#define kMusicFile @"刘若英 - 原来你也在这里.mp3"
#define kMusicSinger @"刘若英"
#define kMusicTitle @"原来你也在这里"

@interface ViewController ()<AVAudioPlayerDelegate>

@property (nonatomic,strong) AVAudioPlayer *audioPlayer;//播放器
@property (weak, nonatomic) IBOutlet UILabel *controlPanel; //控制面板
@property (weak, nonatomic) IBOutlet UIProgressView *playProgress;//播放进度
@property (weak, nonatomic) IBOutlet UILabel *musicSinger; //演唱者
@property (weak, nonatomic) IBOutlet UIButton *playOrPause; //播放/暂停按钮(如果tag为0认为是暂停状态，1是播放状态)

@property (weak ,nonatomic) NSTimer *timer;//进度更新定时器

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    [self setupUI];
    
}

/**
 *  显示当面视图控制器时注册远程事件
 *
 *  @param animated 是否以动画的形式显示
 */
-(void)viewWillAppear:(BOOL)animated{
    [super viewWillAppear:animated];
    //开启远程控制
    [[UIApplication sharedApplication] beginReceivingRemoteControlEvents];
    //作为第一响应者
    //[self becomeFirstResponder];
}
/**
 *  当前控制器视图不显示时取消远程控制
 *
 *  @param animated 是否以动画的形式消失
 */
-(void)viewWillDisappear:(BOOL)animated{
    [super viewWillDisappear:animated];
    [[UIApplication sharedApplication] endReceivingRemoteControlEvents];
    //[self resignFirstResponder];
}

/**
 *  初始化UI
 */
-(void)setupUI{
    self.title=kMusicTitle;
    self.musicSinger.text=kMusicSinger;
}

-(NSTimer *)timer{
    if (!_timer) {
        _timer=[NSTimer scheduledTimerWithTimeInterval:0.5 target:self selector:@selector(updateProgress) userInfo:nil repeats:true];
    }
    return _timer;
}

/**
 *  创建播放器
 *
 *  @return 音频播放器
 */
-(AVAudioPlayer *)audioPlayer{
    if (!_audioPlayer) {
        NSString *urlStr=[[NSBundle mainBundle]pathForResource:kMusicFile ofType:nil];
        NSURL *url=[NSURL fileURLWithPath:urlStr];
        NSError *error=nil;
        //初始化播放器，注意这里的Url参数只能时文件路径，不支持HTTP Url
        _audioPlayer=[[AVAudioPlayer alloc]initWithContentsOfURL:url error:&error];
        //设置播放器属性
        _audioPlayer.numberOfLoops=0;//设置为0不循环
        _audioPlayer.delegate=self;
        [_audioPlayer prepareToPlay];//加载音频文件到缓存
        if(error){
            NSLog(@"初始化播放器过程发生错误,错误信息:%@",error.localizedDescription);
            return nil;
        }
        //设置后台播放模式
        AVAudioSession *audioSession=[AVAudioSession sharedInstance];
        [audioSession setCategory:AVAudioSessionCategoryPlayback error:nil];
//        [audioSession setCategory:AVAudioSessionCategoryPlayback withOptions:AVAudioSessionCategoryOptionAllowBluetooth error:nil];
        [audioSession setActive:YES error:nil];
        //添加通知，拔出耳机后暂停播放
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(routeChange:) name:AVAudioSessionRouteChangeNotification object:nil];
    }
    return _audioPlayer;
}

/**
 *  播放音频
 */
-(void)play{
    if (![self.audioPlayer isPlaying]) {
        [self.audioPlayer play];
        self.timer.fireDate=[NSDate distantPast];//恢复定时器
    }
}

/**
 *  暂停播放
 */
-(void)pause{
    if ([self.audioPlayer isPlaying]) {
        [self.audioPlayer pause];
        self.timer.fireDate=[NSDate distantFuture];//暂停定时器，注意不能调用invalidate方法，此方法会取消，之后无法恢复
        
    }
}

/**
 *  点击播放/暂停按钮
 *
 *  @param sender 播放/暂停按钮
 */
- (IBAction)playClick:(UIButton *)sender {
    if(sender.tag){
        sender.tag=0;
        [sender setImage:[UIImage imageNamed:@"playing_btn_play_n"] forState:UIControlStateNormal];
        [sender setImage:[UIImage imageNamed:@"playing_btn_play_h"] forState:UIControlStateHighlighted];
        [self pause];
    }else{
        sender.tag=1;
        [sender setImage:[UIImage imageNamed:@"playing_btn_pause_n"] forState:UIControlStateNormal];
        [sender setImage:[UIImage imageNamed:@"playing_btn_pause_h"] forState:UIControlStateHighlighted];
        [self play];
    }
}

/**
 *  更新播放进度
 */
-(void)updateProgress{
    float progress= self.audioPlayer.currentTime /self.audioPlayer.duration;
    [self.playProgress setProgress:progress animated:true];
}

/**
 *  一旦输出改变则执行此方法
 *
 *  @param notification 输出改变通知对象
 */
-(void)routeChange:(NSNotification *)notification{
    NSDictionary *dic=notification.userInfo;
    int changeReason= [dic[AVAudioSessionRouteChangeReasonKey] intValue];
    //等于AVAudioSessionRouteChangeReasonOldDeviceUnavailable表示旧输出不可用
    if (changeReason==AVAudioSessionRouteChangeReasonOldDeviceUnavailable) {
        AVAudioSessionRouteDescription *routeDescription=dic[AVAudioSessionRouteChangePreviousRouteKey];
        AVAudioSessionPortDescription *portDescription= [routeDescription.outputs firstObject];
        //原设备为耳机则暂停
        if ([portDescription.portType isEqualToString:@"Headphones"]) {
            [self pause];
        }
    }
    
//    [dic enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
//        NSLog(@"%@:%@",key,obj);
//    }];
}

-(void)dealloc{
    [[NSNotificationCenter defaultCenter] removeObserver:self name:AVAudioSessionRouteChangeNotification object:nil];
}

#pragma mark - 播放器代理方法
-(void)audioPlayerDidFinishPlaying:(AVAudioPlayer *)player successfully:(BOOL)flag{
    NSLog(@"音乐播放完成...");
    //根据实际情况播放完成可以将会话关闭，其他音频应用继续播放
    [[AVAudioSession sharedInstance]setActive:NO error:nil];
}

@end
在上面的代码中还实现了拔出耳机暂停音乐播放的功能，这也是一个比较常见的功能。在iOS7及以后的版本中可以通过通知获得输出改变的通知，然后拿到通知对象后根据userInfo获得是何种改变类型，进而根据情况对音乐进行暂停操作。
扩展--播放音乐库中的音乐
众所周知音乐是iOS的重要组成播放，无论是iPod、iTouch、iPhone还是iPad都可以在iTunes购买音乐或添加本地音乐到音乐库中同步到你的iOS设备。在MediaPlayer.frameowork中有一个MPMusicPlayerController用于播放音乐库中的音乐。
下面先来看一下MPMusicPlayerController的常用属性和方法：
属性	说明
@property (nonatomic, readonly) MPMusicPlaybackState playbackState	播放器状态，枚举类型：
MPMusicPlaybackStateStopped：停止播放 MPMusicPlaybackStatePlaying：正在播放
MPMusicPlaybackStatePaused：暂停播放
MPMusicPlaybackStateInterrupted：播放中断
MPMusicPlaybackStateSeekingForward：向前查找
MPMusicPlaybackStateSeekingBackward：向后查找
@property (nonatomic) MPMusicRepeatMode repeatMode	重复模式，枚举类型：
MPMusicRepeatModeDefault：默认模式，使用用户的首选项（系统音乐程序设置）
MPMusicRepeatModeNone：不重复
MPMusicRepeatModeOne：单曲循环
MPMusicRepeatModeAll：在当前列表内循环
@property (nonatomic) MPMusicShuffleMode shuffleMode	随机播放模式，枚举类型：
MPMusicShuffleModeDefault：默认模式，使用用户首选项（系统音乐程序设置）
MPMusicShuffleModeOff：不随机播放
MPMusicShuffleModeSongs：按歌曲随机播放
MPMusicShuffleModeAlbums：按专辑随机播放
@property (nonatomic, copy) MPMediaItem *nowPlayingItem	正在播放的音乐项
@property (nonatomic, readonly) NSUInteger indexOfNowPlayingItem	当前正在播放的音乐在播放队列中的索引
@property(nonatomic, readonly) BOOL isPreparedToPlay	是否准好播放准备
@property(nonatomic) NSTimeInterval currentPlaybackTime	当前已播放时间，单位：秒
@property(nonatomic) float currentPlaybackRate	当前播放速度，是一个播放速度倍率，0表示暂停播放，1代表正常速度
类方法	说明
+ (MPMusicPlayerController *)applicationMusicPlayer;	获取应用播放器，注意此类播放器无法在后台播放
+ (MPMusicPlayerController *)systemMusicPlayer	获取系统播放器，支持后台播放
对象方法	说明
- (void)setQueueWithQuery:(MPMediaQuery *)query	使用媒体队列设置播放源媒体队列
- (void)setQueueWithItemCollection:(MPMediaItemCollection *)itemCollection	使用媒体项集合设置播放源媒体队列
- (void)skipToNextItem	下一曲
- (void)skipToBeginning	从起始位置播放
- (void)skipToPreviousItem	上一曲
- (void)beginGeneratingPlaybackNotifications	开启播放通知，注意不同于其他播放器，MPMusicPlayerController要想获得通知必须首先开启，默认情况无法获得通知
- (void)endGeneratingPlaybackNotifications	关闭播放通知
- (void)prepareToPlay	做好播放准备（加载音频到缓冲区），在使用play方法播放时如果没有做好准备回自动调用该方法
- (void)play	开始播放
- (void)pause	暂停播放
- (void)stop	停止播放
- (void)beginSeekingForward	开始向前查找（快进）
- (void)beginSeekingBackward	开始向后查找（快退）
- (void)endSeeking	结束查找
通知	说明
（注意：要想获得MPMusicPlayerController通知必须首先调用beginGeneratingPlaybackNotifications开启通知）
MPMusicPlayerControllerPlaybackStateDidChangeNotification	播放状态改变
MPMusicPlayerControllerNowPlayingItemDidChangeNotification	当前播放音频改变
MPMusicPlayerControllerVolumeDidChangeNotification	声音大小改变
MPMediaPlaybackIsPreparedToPlayDidChangeNotification	准备好播放
MPMusicPlayerController有两种播放器：applicationMusicPlayer和systemMusicPlayer，前者在应用退出后音乐播放会自动停止，后者在应用停止后不会退出播放状态。
MPMusicPlayerController加载音乐不同于前面的AVAudioPlayer是通过一个文件路径来加载，而是需要一个播放队列。在MPMusicPlayerController中提供了两个方法来加载播放队列：- (void)setQueueWithQuery:(MPMediaQuery *)query和- (void)setQueueWithItemCollection:(MPMediaItemCollection *)itemCollection，正是由于它的播放音频来源是一个队列，因此MPMusicPlayerController支持上一曲、下一曲等操作。
那么接下来的问题就是如何获取MPMediaQueue或者MPMediaItemCollection？MPMediaQueue对象有一系列的类方法来获得媒体队列：
+ (MPMediaQuery *)albumsQuery;
+ (MPMediaQuery *)artistsQuery;
+ (MPMediaQuery *)songsQuery;
+ (MPMediaQuery *)playlistsQuery;
+ (MPMediaQuery *)podcastsQuery;
+ (MPMediaQuery *)audiobooksQuery;
+ (MPMediaQuery *)compilationsQuery;
+ (MPMediaQuery *)composersQuery;
+ (MPMediaQuery *)genresQuery;
有了这些方法，就可以很容易获到歌曲、播放列表、专辑媒体等媒体队列了，这样就可以通过：- (void)setQueueWithQuery:(MPMediaQuery *)query方法设置音乐来源了。又或者得到MPMediaQueue之后创建MPMediaItemCollection，使用- (void)setQueueWithItemCollection:(MPMediaItemCollection *)itemCollection设置音乐来源。
有时候可能希望用户自己来选择要播放的音乐，这时可以使用MPMediaPickerController，它是一个视图控制器，类似于UIImagePickerController，选择完播放来源后可以在其代理方法中获得MPMediaItemCollection对象。
无论是通过哪种方式获得MPMusicPlayerController的媒体源，可能都希望将每个媒体的信息显示出来，这时候可以通过MPMediaItem对象获得。一个MPMediaItem代表一个媒体文件，通过它可以访问媒体标题、专辑名称、专辑封面、音乐时长等等。无论是MPMediaQueue还是MPMediaItemCollection都有一个items属性，它是MPMediaItem数组，通过这个属性可以获得MPMediaItem对象。
下面就简单看一下MPMusicPlayerController的使用，在下面的例子中简单演示了音乐的选择、播放、暂停、通知、下一曲、上一曲功能，相信有了上面的概念，代码读起来并不复杂（示例中是直接通过MPMeidaPicker进行音乐选择的，但是仍然提供了两个方法getLocalMediaQuery和getLocalMediaItemCollection来演示如何直接通过MPMediaQueue获得媒体队列或媒体集合）：
//
//  ViewController.m
//  MPMusicPlayerController
//
//  Created by Kenshin Cui 14/03/30
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <MediaPlayer/MediaPlayer.h>

@interface ViewController ()<MPMediaPickerControllerDelegate>

@property (nonatomic,strong) MPMediaPickerController *mediaPicker;//媒体选择控制器
@property (nonatomic,strong) MPMusicPlayerController *musicPlayer; //音乐播放器

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
}

-(void)dealloc{
    [self.musicPlayer endGeneratingPlaybackNotifications];
}

/**
 *  获得音乐播放器
 *
 *  @return 音乐播放器
 */
-(MPMusicPlayerController *)musicPlayer{
    if (!_musicPlayer) {
        _musicPlayer=[MPMusicPlayerController systemMusicPlayer];
        [_musicPlayer beginGeneratingPlaybackNotifications];//开启通知，否则监控不到MPMusicPlayerController的通知
        [self addNotification];//添加通知
        //如果不使用MPMediaPickerController可以使用如下方法获得音乐库媒体队列
        //[_musicPlayer setQueueWithItemCollection:[self getLocalMediaItemCollection]];
    }
    return _musicPlayer;
}

/**
 *  创建媒体选择器
 *
 *  @return 媒体选择器
 */
-(MPMediaPickerController *)mediaPicker{
    if (!_mediaPicker) {
        //初始化媒体选择器，这里设置媒体类型为音乐，其实这里也可以选择视频、广播等
//        _mediaPicker=[[MPMediaPickerController alloc]initWithMediaTypes:MPMediaTypeMusic];
        _mediaPicker=[[MPMediaPickerController alloc]initWithMediaTypes:MPMediaTypeAny];
        _mediaPicker.allowsPickingMultipleItems=YES;//允许多选
//        _mediaPicker.showsCloudItems=YES;//显示icloud选项
        _mediaPicker.prompt=@"请选择要播放的音乐";
        _mediaPicker.delegate=self;//设置选择器代理
    }
    return _mediaPicker;
}

/**
 *  取得媒体队列
 *
 *  @return 媒体队列
 */
-(MPMediaQuery *)getLocalMediaQuery{
    MPMediaQuery *mediaQueue=[MPMediaQuery songsQuery];
    for (MPMediaItem *item in mediaQueue.items) {
        NSLog(@"标题：%@,%@",item.title,item.albumTitle);
    }
    return mediaQueue;
}

/**
 *  取得媒体集合
 *
 *  @return 媒体集合
 */
-(MPMediaItemCollection *)getLocalMediaItemCollection{
    MPMediaQuery *mediaQueue=[MPMediaQuery songsQuery];
    NSMutableArray *array=[NSMutableArray array];
    for (MPMediaItem *item in mediaQueue.items) {
        [array addObject:item];
        NSLog(@"标题：%@,%@",item.title,item.albumTitle);
    }
    MPMediaItemCollection *mediaItemCollection=[[MPMediaItemCollection alloc]initWithItems:[array copy]];
    return mediaItemCollection;
}

#pragma mark - MPMediaPickerController代理方法
//选择完成
-(void)mediaPicker:(MPMediaPickerController *)mediaPicker didPickMediaItems:(MPMediaItemCollection *)mediaItemCollection{
    MPMediaItem *mediaItem=[mediaItemCollection.items firstObject];//第一个播放音乐
    //注意很多音乐信息如标题、专辑、表演者、封面、时长等信息都可以通过MPMediaItem的valueForKey:方法得到,但是从iOS7开始都有对应的属性可以直接访问
//    NSString *title= [mediaItem valueForKey:MPMediaItemPropertyAlbumTitle];
//    NSString *artist= [mediaItem valueForKey:MPMediaItemPropertyAlbumArtist];
//    MPMediaItemArtwork *artwork= [mediaItem valueForKey:MPMediaItemPropertyArtwork];
    //UIImage *image=[artwork imageWithSize:CGSizeMake(100, 100)];//专辑图片
    NSLog(@"标题：%@,表演者：%@,专辑：%@",mediaItem.title ,mediaItem.artist,mediaItem.albumTitle);
    [self.musicPlayer setQueueWithItemCollection:mediaItemCollection];
    [self dismissViewControllerAnimated:YES completion:nil];
}
//取消选择
-(void)mediaPickerDidCancel:(MPMediaPickerController *)mediaPicker{
    [self dismissViewControllerAnimated:YES completion:nil];
}

#pragma mark - 通知
/**
 *  添加通知
 */
-(void)addNotification{
    NSNotificationCenter *notificationCenter=[NSNotificationCenter defaultCenter];
    [notificationCenter addObserver:self selector:@selector(playbackStateChange:) name:MPMusicPlayerControllerPlaybackStateDidChangeNotification object:self.musicPlayer];
}

/**
 *  播放状态改变通知
 *
 *  @param notification 通知对象
 */
-(void)playbackStateChange:(NSNotification *)notification{
    switch (self.musicPlayer.playbackState) {
        case MPMusicPlaybackStatePlaying:
            NSLog(@"正在播放...");
            break;
        case MPMusicPlaybackStatePaused:
            NSLog(@"播放暂停.");
            break;
        case MPMusicPlaybackStateStopped:
            NSLog(@"播放停止.");
            break;
        default:
            break;
    }
}

#pragma mark - UI事件
- (IBAction)selectClick:(UIButton *)sender {
    [self presentViewController:self.mediaPicker animated:YES completion:nil];
}

- (IBAction)playClick:(UIButton *)sender {
    [self.musicPlayer play];
}

- (IBAction)puaseClick:(UIButton *)sender {
    [self.musicPlayer pause];
}

- (IBAction)stopClick:(UIButton *)sender {
    [self.musicPlayer stop];
}

- (IBAction)nextClick:(UIButton *)sender {
    [self.musicPlayer skipToNextItem];
}

- (IBAction)prevClick:(UIButton *)sender {
    [self.musicPlayer skipToPreviousItem];
}

@end
录音
除了上面说的，在AVFoundation框架中还要一个AVAudioRecorder类专门处理录音操作，它同样支持多种音频格式。与AVAudioPlayer类似，你完全可以将它看成是一个录音机控制类，下面是常用的属性和方法：
属性	说明
@property(readonly, getter=isRecording) BOOL recording;	是否正在录音，只读
@property(readonly) NSURL *url	录音文件地址，只读
@property(readonly) NSDictionary *settings	录音文件设置，只读
@property(readonly) NSTimeInterval currentTime	录音时长，只读，注意仅仅在录音状态可用
@property(readonly) NSTimeInterval deviceCurrentTime	输入设置的时间长度，只读，注意此属性一直可访问
@property(getter=isMeteringEnabled) BOOL meteringEnabled;	是否启用录音测量，如果启用录音测量可以获得录音分贝等数据信息
@property(nonatomic, copy) NSArray *channelAssignments	当前录音的通道
对象方法	说明
- (instancetype)initWithURL:(NSURL *)url settings:(NSDictionary *)settings error:(NSError **)outError	录音机对象初始化方法，注意其中的url必须是本地文件url，settings是录音格式、编码等设置
- (BOOL)prepareToRecord	准备录音，主要用于创建缓冲区，如果不手动调用，在调用record录音时也会自动调用
- (BOOL)record	开始录音
- (BOOL)recordAtTime:(NSTimeInterval)time	在指定的时间开始录音，一般用于录音暂停再恢复录音
- (BOOL)recordForDuration:(NSTimeInterval) duration	按指定的时长开始录音
- (BOOL)recordAtTime:(NSTimeInterval)time forDuration:(NSTimeInterval) duration	在指定的时间开始录音，并指定录音时长
- (void)pause;	暂停录音
- (void)stop;	停止录音
- (BOOL)deleteRecording;	删除录音，注意要删除录音此时录音机必须处于停止状态
- (void)updateMeters;	更新测量数据，注意只有meteringEnabled为YES此方法才可用
- (float)peakPowerForChannel:(NSUInteger)channelNumber;	指定通道的测量峰值，注意只有调用完updateMeters才有值
- (float)averagePowerForChannel:(NSUInteger)channelNumber	指定通道的测量平均值，注意只有调用完updateMeters才有值
代理方法	说明
- (void)audioRecorderDidFinishRecording:(AVAudioRecorder *)recorder successfully:(BOOL)flag	完成录音
- (void)audioRecorderEncodeErrorDidOccur:(AVAudioRecorder *)recorder error:(NSError *)error	录音编码发生错误
AVAudioRecorder很多属性和方法跟AVAudioPlayer都是类似的,但是它的创建有所不同，在创建录音机时除了指定路径外还必须指定录音设置信息，因为录音机必须知道录音文件的格式、采样率、通道数、每个采样点的位数等信息，但是也并不是所有的信息都必须设置，通常只需要几个常用设置。关于录音设置详见帮助文档中的“AV Foundation Audio Settings Constants”。
下面就使用AVAudioRecorder创建一个录音机，实现了录音、暂停、停止、播放等功能，实现效果大致如下：
AVAudioRecorderSnapshot
在这个示例中将实行一个完整的录音控制，包括录音、暂停、恢复、停止，同时还会实时展示用户录音的声音波动，当用户点击完停止按钮还会自动播放录音文件。程序的构建主要分为以下几步：
设置音频会话类型为AVAudioSessionCategoryPlayAndRecord，因为程序中牵扯到录音和播放操作。
创建录音机AVAudioRecorder，指定录音保存的路径并且设置录音属性，注意对于一般的录音文件要求的采样率、位数并不高，需要适当设置以保证录音文件的大小和效果。
设置录音机代理以便在录音完成后播放录音，打开录音测量保证能够实时获得录音时的声音强度。（注意声音强度范围-160到0,0代表最大输入）
创建音频播放器AVAudioPlayer，用于在录音完成之后播放录音。
创建一个定时器以便实时刷新录音测量值并更新录音强度到UIProgressView中显示。
添加录音、暂停、恢复、停止操作，需要注意录音的恢复操作其实是有音频会话管理的，恢复时只要再次调用record方法即可，无需手动管理恢复时间等。
下面是主要代码：
//
//  ViewController.m
//  AVAudioRecorder
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
#define kRecordAudioFile @"myRecord.caf"

@interface ViewController ()<AVAudioRecorderDelegate>

@property (nonatomic,strong) AVAudioRecorder *audioRecorder;//音频录音机
@property (nonatomic,strong) AVAudioPlayer *audioPlayer;//音频播放器，用于播放录音文件
@property (nonatomic,strong) NSTimer *timer;//录音声波监控（注意这里暂时不对播放进行监控）

@property (weak, nonatomic) IBOutlet UIButton *record;//开始录音
@property (weak, nonatomic) IBOutlet UIButton *pause;//暂停录音
@property (weak, nonatomic) IBOutlet UIButton *resume;//恢复录音
@property (weak, nonatomic) IBOutlet UIButton *stop;//停止录音
@property (weak, nonatomic) IBOutlet UIProgressView *audioPower;//音频波动

@end

@implementation ViewController

#pragma mark - 控制器视图方法
- (void)viewDidLoad {
    [super viewDidLoad];
    
    [self setAudioSession];
}

#pragma mark - 私有方法
/**
 *  设置音频会话
 */
-(void)setAudioSession{
    AVAudioSession *audioSession=[AVAudioSession sharedInstance];
    //设置为播放和录音状态，以便可以在录制完之后播放录音
    [audioSession setCategory:AVAudioSessionCategoryPlayAndRecord error:nil];
    [audioSession setActive:YES error:nil];
}

/**
 *  取得录音文件保存路径
 *
 *  @return 录音文件路径
 */
-(NSURL *)getSavePath{
    NSString *urlStr=[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
    urlStr=[urlStr stringByAppendingPathComponent:kRecordAudioFile];
    NSLog(@"file path:%@",urlStr);
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}

/**
 *  取得录音文件设置
 *
 *  @return 录音设置
 */
-(NSDictionary *)getAudioSetting{
    NSMutableDictionary *dicM=[NSMutableDictionary dictionary];
    //设置录音格式
    [dicM setObject:@(kAudioFormatLinearPCM) forKey:AVFormatIDKey];
    //设置录音采样率，8000是电话采样率，对于一般录音已经够了
    [dicM setObject:@(8000) forKey:AVSampleRateKey];
    //设置通道,这里采用单声道
    [dicM setObject:@(1) forKey:AVNumberOfChannelsKey];
    //每个采样点位数,分为8、16、24、32
    [dicM setObject:@(8) forKey:AVLinearPCMBitDepthKey];
    //是否使用浮点数采样
    [dicM setObject:@(YES) forKey:AVLinearPCMIsFloatKey];
    //....其他设置等
    return dicM;
}

/**
 *  获得录音机对象
 *
 *  @return 录音机对象
 */
-(AVAudioRecorder *)audioRecorder{
    if (!_audioRecorder) {
        //创建录音文件保存路径
        NSURL *url=[self getSavePath];
        //创建录音格式设置
        NSDictionary *setting=[self getAudioSetting];
        //创建录音机
        NSError *error=nil;
        _audioRecorder=[[AVAudioRecorder alloc]initWithURL:url settings:setting error:&error];
        _audioRecorder.delegate=self;
        _audioRecorder.meteringEnabled=YES;//如果要监控声波则必须设置为YES
        if (error) {
            NSLog(@"创建录音机对象时发生错误，错误信息：%@",error.localizedDescription);
            return nil;
        }
    }
    return _audioRecorder;
}

/**
 *  创建播放器
 *
 *  @return 播放器
 */
-(AVAudioPlayer *)audioPlayer{
    if (!_audioPlayer) {
        NSURL *url=[self getSavePath];
        NSError *error=nil;
        _audioPlayer=[[AVAudioPlayer alloc]initWithContentsOfURL:url error:&error];
        _audioPlayer.numberOfLoops=0;
        [_audioPlayer prepareToPlay];
        if (error) {
            NSLog(@"创建播放器过程中发生错误，错误信息：%@",error.localizedDescription);
            return nil;
        }
    }
    return _audioPlayer;
}

/**
 *  录音声波监控定制器
 *
 *  @return 定时器
 */
-(NSTimer *)timer{
    if (!_timer) {
        _timer=[NSTimer scheduledTimerWithTimeInterval:0.1f target:self selector:@selector(audioPowerChange) userInfo:nil repeats:YES];
    }
    return _timer;
}

/**
 *  录音声波状态设置
 */
-(void)audioPowerChange{
    [self.audioRecorder updateMeters];//更新测量值
    float power= [self.audioRecorder averagePowerForChannel:0];//取得第一个通道的音频，注意音频强度范围时-160到0
    CGFloat progress=(1.0/160.0)*(power+160.0);
    [self.audioPower setProgress:progress];
}
#pragma mark - UI事件
/**
 *  点击录音按钮
 *
 *  @param sender 录音按钮
 */
- (IBAction)recordClick:(UIButton *)sender {
    if (![self.audioRecorder isRecording]) {
        [self.audioRecorder record];//首次使用应用时如果调用record方法会询问用户是否允许使用麦克风
        self.timer.fireDate=[NSDate distantPast];
    }
}

/**
 *  点击暂定按钮
 *
 *  @param sender 暂停按钮
 */
- (IBAction)pauseClick:(UIButton *)sender {
    if ([self.audioRecorder isRecording]) {
        [self.audioRecorder pause];
        self.timer.fireDate=[NSDate distantFuture];
    }
}

/**
 *  点击恢复按钮
 *  恢复录音只需要再次调用record，AVAudioSession会帮助你记录上次录音位置并追加录音
 *
 *  @param sender 恢复按钮
 */
- (IBAction)resumeClick:(UIButton *)sender {
    [self recordClick:sender];
}

/**
 *  点击停止按钮
 *
 *  @param sender 停止按钮
 */
- (IBAction)stopClick:(UIButton *)sender {
    [self.audioRecorder stop];
    self.timer.fireDate=[NSDate distantFuture];
    self.audioPower.progress=0.0;
}

#pragma mark - 录音机代理方法
/**
 *  录音完成，录音完成后播放录音
 *
 *  @param recorder 录音机对象
 *  @param flag     是否成功
 */
-(void)audioRecorderDidFinishRecording:(AVAudioRecorder *)recorder successfully:(BOOL)flag{
    if (![self.audioPlayer isPlaying]) {
        [self.audioPlayer play];
    }
    NSLog(@"录音完成!");
}

@end
运行效果：
AVAudioRecorder
音频队列服务
大家应该已经注意到了，无论是前面的录音还是音频播放均不支持网络流媒体播放，当然对于录音来说这种需求可能不大，但是对于音频播放来说有时候就很有必要了。AVAudioPlayer只能播放本地文件，并且是一次性加载所以音频数据，初始化AVAudioPlayer时指定的URL也只能是File URL而不能是HTTP URL。当然，将音频文件下载到本地然后再调用AVAudioPlayer来播放也是一种播放网络音频的办法，但是这种方式最大的弊端就是必须等到整个音频播放完成才能播放，而不能使用流式播放，这往往在实际开发中是不切实际的。那么在iOS中如何播放网络流媒体呢？就是使用AudioToolbox框架中的音频队列服务Audio Queue Services。
使用音频队列服务完全可以做到音频播放和录制，首先看一下录音音频服务队列：
recording_architecture_2x
一个音频服务队列Audio Queue有三部分组成：
三个缓冲器Buffers:每个缓冲器都是一个存储音频数据的临时仓库。
一个缓冲队列Buffer Queue:一个包含音频缓冲器的有序队列。
一个回调Callback:一个自定义的队列回调函数。
声音通过输入设备进入缓冲队列中，首先填充第一个缓冲器；当第一个缓冲器填充满之后自动填充下一个缓冲器，同时会调用回调函数；在回调函数中需要将缓冲器中的音频数据写入磁盘，同时将缓冲器放回到缓冲队列中以便重用。下面是Apple官方关于音频队列服务的流程示意图：
recording_callback_function_2x
类似的，看一下音频播放缓冲队列，其组成部分和录音缓冲队列类似。
playback_architecture_2x
但是在音频播放缓冲队列中，回调函数调用的时机不同于音频录制缓冲队列，流程刚好相反。将音频读取到缓冲器中，一旦一个缓冲器填充满之后就放到缓冲队列中，然后继续填充其他缓冲器；当开始播放时，则从第一个缓冲器中读取音频进行播放；一旦播放完之后就会触发回调函数，开始播放下一个缓冲器中的音频，同时填充第一个缓冲器放；填充满之后再次放回到缓冲队列。下面是详细的流程：
playback_callback_function_2x
当然，要明白音频队列服务的原理并不难，问题是如何实现这个自定义的回调函数，这其中我们有大量的工作要做，控制播放状态、处理异常中断、进行音频编码等等。由于牵扯内容过多，而且不是本文目的，如果以后有时间将另开一篇文章重点介绍，目前有很多第三方优秀框架可以直接使用，例如AudioStreamer、FreeStreamer。由于前者当前只有非ARC版本，所以下面不妨使用FreeStreamer来简单演示在线音频播放的过程，当然在使用之前要做如下准备工作：
1.拷贝FreeStreamer中的Reachability.h、Reachability.m和Common、astreamer两个文件夹中的内容到项目中。
2.添加FreeStreamer使用的类库：CFNetwork.framework、AudioToolbox.framework、AVFoundation.framework
、libxml2.dylib、MediaPlayer.framework。
3.如果引用libxml2.dylib编译不通过，需要在Xcode的Targets-Build Settings-Header Build Path中添加$(SDKROOT)/usr/include/libxml2。
4.将FreeStreamer中的FreeStreamerMobile-Prefix.pch文件添加到项目中并将Targets-Build Settings-Precompile Prefix Header设置为YES，在Targets-Build Settings-Prefix Header设置为$(SRCROOT)/项目名称/FreeStreamerMobile-Prefix.pch（因为Xcode6默认没有pch文件）
然后就可以编写代码播放网络音频了：
//
//  ViewController.m
//  AudioQueueServices
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//  使用FreeStreamer实现网络音频播放

#import "ViewController.h"
#import "FSAudioStream.h"

@interface ViewController ()

@property (nonatomic,strong) FSAudioStream *audioStream;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];

    [self.audioStream play];
}

/**
 *  取得本地文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getFileUrl{
    NSString *urlStr=[[NSBundle mainBundle]pathForResource:@"刘若英 - 原来你也在这里.mp3" ofType:nil];
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}
-(NSURL *)getNetworkUrl{
    NSString *urlStr=@"http://192.168.1.102/liu.mp3";
    NSURL *url=[NSURL URLWithString:urlStr];
    return url;
}

/**
 *  创建FSAudioStream对象
 *
 *  @return FSAudioStream对象
 */
-(FSAudioStream *)audioStream{
    if (!_audioStream) {
        NSURL *url=[self getNetworkUrl];
        //创建FSAudioStream对象
        _audioStream=[[FSAudioStream alloc]initWithUrl:url];
        _audioStream.onFailure=^(FSAudioStreamError error,NSString *description){
            NSLog(@"播放过程中发生错误，错误信息：%@",description);
        };
        _audioStream.onCompletion=^(){
            NSLog(@"播放完成!");
        };
        [_audioStream setVolume:0.5];//设置声音
    }
    return _audioStream;
}

@end
其实FreeStreamer的功能很强大，不仅仅是播放本地、网络音频那么简单，它还支持播放列表、检查包内容、RSS订阅、播放中断等很多强大的功能，甚至还包含了一个音频分析器，有兴趣的朋友可以访问官网查看详细用法
视频
MPMoviePlayerController
在iOS中播放视频可以使用MediaPlayer.framework种的MPMoviePlayerController类来完成，它支持本地视频和网络视频播放。这个类实现了MPMediaPlayback协议，因此具备一般的播放器控制功能，例如播放、暂停、停止等。但是MPMediaPlayerController自身并不是一个完整的视图控制器，如果要在UI中展示视频需要将view属性添加到界面中。下面列出了MPMoviePlayerController的常用属性和方法：
属性	说明
@property (nonatomic, copy) NSURL *contentURL	播放媒体URL，这个URL可以是本地路径，也可以是网络路径
@property (nonatomic, readonly) UIView *view	播放器视图，如果要显示视频必须将此视图添加到控制器视图中
@property (nonatomic, readonly) UIView *backgroundView	播放器背景视图
@property (nonatomic, readonly) MPMoviePlaybackState playbackState	媒体播放状态，枚举类型：
MPMoviePlaybackStateStopped：停止播放
MPMoviePlaybackStatePlaying：正在播放
MPMoviePlaybackStatePaused：暂停
MPMoviePlaybackStateInterrupted：中断
MPMoviePlaybackStateSeekingForward：向前定位
MPMoviePlaybackStateSeekingBackward：向后定位
@property (nonatomic, readonly) MPMovieLoadState loadState	网络媒体加载状态，枚举类型：
MPMovieLoadStateUnknown：位置类型
MPMovieLoadStatePlayable：
MPMovieLoadStatePlaythroughOK：这种状态如果shouldAutoPlay为YES将自动播放
MPMovieLoadStateStalled：停滞状态
@property (nonatomic) MPMovieControlStyle controlStyle	控制面板风格，枚举类型：
MPMovieControlStyleNone：无控制面板 
MPMovieControlStyleEmbedded：嵌入视频风格 
MPMovieControlStyleFullscreen：全屏 
MPMovieControlStyleDefault：默认风格
@property (nonatomic) MPMovieRepeatMode repeatMode;	重复播放模式，枚举类型:
MPMovieRepeatModeNone:不重复，默认值
MPMovieRepeatModeOne:重复播放
@property (nonatomic) BOOL shouldAutoplay	当网络媒体缓存到一定数据时是否自动播放，默认为YES
@property (nonatomic, getter=isFullscreen) BOOL fullscreen	是否全屏展示，默认为NO，注意如果要通过此属性设置全屏必须在视图显示完成后设置，否则无效
@property (nonatomic) MPMovieScalingMode scalingMode	视频缩放填充模式，枚举类型：
MPMovieScalingModeNone：不进行任何缩放
MPMovieScalingModeAspectFit：固定缩放比例并且尽量全部展示视频，不会裁切视频
MPMovieScalingModeAspectFill：固定缩放比例并填充满整个视图展示，可能会裁切视频
MPMovieScalingModeFill：不固定缩放比例压缩填充整个视图，视频不会被裁切但是比例失衡
@property (nonatomic, readonly) BOOL readyForDisplay	是否有相关媒体被播放
@property (nonatomic, readonly) MPMovieMediaTypeMask movieMediaTypes	媒体类别，枚举类型：
MPMovieMediaTypeMaskNone：未知类型
MPMovieMediaTypeMaskVideo：视频
MPMovieMediaTypeMaskAudio：音频
@property (nonatomic) MPMovieSourceType movieSourceType	媒体源，枚举类型：
MPMovieSourceTypeUnknown：未知来源
MPMovieSourceTypeFile：本地文件
MPMovieSourceTypeStreaming：流媒体（直播或点播）
@property (nonatomic, readonly) NSTimeInterval duration	媒体时长，如果未知则返回0
@property (nonatomic, readonly) NSTimeInterval playableDuration	媒体可播放时长，主要用于表示网络媒体已下载视频时长
@property (nonatomic, readonly) CGSize naturalSize	视频实际尺寸，如果未知则返回CGSizeZero
@property (nonatomic) NSTimeInterval initialPlaybackTime	起始播放时间
@property (nonatomic) NSTimeInterval endPlaybackTime	终止播放时间
@property (nonatomic) BOOL allowsAirPlay	是否允许无线播放，默认为YES
@property (nonatomic, readonly, getter=isAirPlayVideoActive) BOOL airPlayVideoActive	当前媒体是否正在通过AirPlay播放
@property(nonatomic, readonly) BOOL isPreparedToPlay	是否准备好播放
@property(nonatomic) NSTimeInterval currentPlaybackTime	当前播放时间，单位：秒
@property(nonatomic) float currentPlaybackRate	当前播放速度，如果暂停则为0，正常速度为1.0，非0数据表示倍率
对象方法	说明
- (instancetype)initWithContentURL:(NSURL *)url	使用指定的URL初始化媒体播放控制器对象
- (void)setFullscreen:(BOOL)fullscreen animated:(BOOL)animated	设置视频全屏，注意如果要通过此方法设置全屏则必须在其视图显示之后设置，否则无效
- (void)requestThumbnailImagesAtTimes:(NSArray *)playbackTimes timeOption:(MPMovieTimeOption)option	获取在指定播放时间的视频缩略图，第一个参数是获取缩略图的时间点数组；第二个参数代表时间点精度，枚举类型：
MPMovieTimeOptionNearestKeyFrame：时间点附近
MPMovieTimeOptionExact：准确时间
- (void)cancelAllThumbnailImageRequests	取消所有缩略图获取请求
- (void)prepareToPlay	准备播放，加载视频数据到缓存，当调用play方法时如果没有准备好会自动调用此方法
- (void)play	开始播放
- (void)pause	暂停播放
- (void)stop	停止播放
- (void)beginSeekingForward	向前定位
- (void)beginSeekingBackward	向后定位
- (void)endSeeking	停止快进/快退
通知	说明
MPMoviePlayerScalingModeDidChangeNotification	视频缩放填充模式发生改变
MPMoviePlayerPlaybackDidFinishNotification	媒体播放完成或用户手动退出，具体完成原因可以通过通知userInfo中的key为MPMoviePlayerPlaybackDidFinishReasonUserInfoKey的对象获取
MPMoviePlayerPlaybackStateDidChangeNotification	播放状态改变，可配合playbakcState属性获取具体状态
MPMoviePlayerLoadStateDidChangeNotification	媒体网络加载状态改变
MPMoviePlayerNowPlayingMovieDidChangeNotification	当前播放的媒体内容发生改变
MPMoviePlayerWillEnterFullscreenNotification	将要进入全屏
MPMoviePlayerDidEnterFullscreenNotification	进入全屏后
MPMoviePlayerWillExitFullscreenNotification	将要退出全屏
MPMoviePlayerDidExitFullscreenNotification	退出全屏后
MPMoviePlayerIsAirPlayVideoActiveDidChangeNotification	当媒体开始通过AirPlay播放或者结束AirPlay播放
MPMoviePlayerReadyForDisplayDidChangeNotification	视频显示状态改变
MPMovieMediaTypesAvailableNotification	确定了媒体可用类型后
MPMovieSourceTypeAvailableNotification	确定了媒体来源后
MPMovieDurationAvailableNotification	确定了媒体播放时长后
MPMovieNaturalSizeAvailableNotification	确定了媒体的实际尺寸后
MPMoviePlayerThumbnailImageRequestDidFinishNotification	缩略图请求完成之后
MPMediaPlaybackIsPreparedToPlayDidChangeNotification	做好播放准备后
注意MPMediaPlayerController的状态等信息并不是通过代理来和外界交互的，而是通过通知中心，因此从上面的列表中可以看到常用的一些通知。由于MPMoviePlayerController本身对于媒体播放做了深度的封装，使用起来就相当简单：创建MPMoviePlayerController对象，设置frame属性，将MPMoviePlayerController的view添加到控制器视图中。下面的示例中将创建一个播放控制器并添加播放状态改变及播放完成的通知：
//
//  ViewController.m
//  MPMoviePlayerController
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <MediaPlayer/MediaPlayer.h>

@interface ViewController ()

@property (nonatomic,strong) MPMoviePlayerController *moviePlayer;//视频播放控制器

@end

@implementation ViewController

#pragma mark - 控制器视图方法
- (void)viewDidLoad {
    [super viewDidLoad];
    
    //播放
    [self.moviePlayer play];
    
    //添加通知
    [self addNotification];
    
}

-(void)dealloc{
    //移除所有通知监控
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}


#pragma mark - 私有方法
/**
 *  取得本地文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getFileUrl{
    NSString *urlStr=[[NSBundle mainBundle] pathForResource:@"The New Look of OS X Yosemite.mp4" ofType:nil];
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}

/**
 *  取得网络文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getNetworkUrl{
    NSString *urlStr=@"http://192.168.1.161/The New Look of OS X Yosemite.mp4";
    urlStr=[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSURL *url=[NSURL URLWithString:urlStr];
    return url;
}

/**
 *  创建媒体播放控制器
 *
 *  @return 媒体播放控制器
 */
-(MPMoviePlayerController *)moviePlayer{
    if (!_moviePlayer) {
        NSURL *url=[self getNetworkUrl];
        _moviePlayer=[[MPMoviePlayerController alloc]initWithContentURL:url];
        _moviePlayer.view.frame=self.view.bounds;
        _moviePlayer.view.autoresizingMask=UIViewAutoresizingFlexibleWidth|UIViewAutoresizingFlexibleHeight;
        [self.view addSubview:_moviePlayer.view];
    }
    return _moviePlayer;
}

/**
 *  添加通知监控媒体播放控制器状态
 */
-(void)addNotification{
    NSNotificationCenter *notificationCenter=[NSNotificationCenter defaultCenter];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackStateChange:) name:MPMoviePlayerPlaybackStateDidChangeNotification object:self.moviePlayer];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackFinished:) name:MPMoviePlayerPlaybackDidFinishNotification object:self.moviePlayer];
    
}

/**
 *  播放状态改变，注意播放完成时的状态是暂停
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackStateChange:(NSNotification *)notification{
    switch (self.moviePlayer.playbackState) {
        case MPMoviePlaybackStatePlaying:
            NSLog(@"正在播放...");
            break;
        case MPMoviePlaybackStatePaused:
            NSLog(@"暂停播放.");
            break;
        case MPMoviePlaybackStateStopped:
            NSLog(@"停止播放.");
            break;
        default:
            NSLog(@"播放状态:%li",self.moviePlayer.playbackState);
            break;
    }
}

/**
 *  播放完成
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackFinished:(NSNotification *)notification{
    NSLog(@"播放完成.%li",self.moviePlayer.playbackState);
}


@end
运行效果：
MPMoviePlayerController
从上面的API大家也不难看出其实MPMoviePlayerController功能相当强大，日常开发中作为一般的媒体播放器也完全没有问题。MPMoviePlayerController除了一般的视频播放和控制外还有一些强大的功能，例如截取视频缩略图。请求视频缩略图时只要调用- (void)requestThumbnailImagesAtTimes:(NSArray *)playbackTimes timeOption:(MPMovieTimeOption)option方法指定获得缩略图的时间点，然后监控MPMoviePlayerThumbnailImageRequestDidFinishNotification通知，每个时间点的缩略图请求完成就会调用通知，在通知调用方法中可以通过MPMoviePlayerThumbnailImageKey获得UIImage对象处理即可。例如下面的程序演示了在程序启动后获得两个时间点的缩略图的过程，截图成功后保存到相册：
//
//  ViewController.m
//  MPMoviePlayerController
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//  视频截图

#import "ViewController.h"
#import <MediaPlayer/MediaPlayer.h>

@interface ViewController ()

@property (nonatomic,strong) MPMoviePlayerController *moviePlayer;//视频播放控制器

@end

@implementation ViewController

#pragma mark - 控制器视图方法
- (void)viewDidLoad {
    [super viewDidLoad];
    
    //播放
    [self.moviePlayer play];
    
    //添加通知
    [self addNotification];
    
    //获取缩略图
    [self thumbnailImageRequest];
}

-(void)dealloc{
    //移除所有通知监控
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}


#pragma mark - 私有方法
/**
 *  取得本地文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getFileUrl{
    NSString *urlStr=[[NSBundle mainBundle] pathForResource:@"The New Look of OS X Yosemite.mp4" ofType:nil];
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}

/**
 *  取得网络文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getNetworkUrl{
    NSString *urlStr=@"http://192.168.1.161/The New Look of OS X Yosemite.mp4";
    urlStr=[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSURL *url=[NSURL URLWithString:urlStr];
    return url;
}

/**
 *  创建媒体播放控制器
 *
 *  @return 媒体播放控制器
 */
-(MPMoviePlayerController *)moviePlayer{
    if (!_moviePlayer) {
        NSURL *url=[self getNetworkUrl];
        _moviePlayer=[[MPMoviePlayerController alloc]initWithContentURL:url];
        _moviePlayer.view.frame=self.view.bounds;
        _moviePlayer.view.autoresizingMask=UIViewAutoresizingFlexibleWidth|UIViewAutoresizingFlexibleHeight;
        [self.view addSubview:_moviePlayer.view];
    }
    return _moviePlayer;
}

/**
 *  获取视频缩略图
 */
-(void)thumbnailImageRequest{
    //获取13.0s、21.5s的缩略图
    [self.moviePlayer requestThumbnailImagesAtTimes:@[@13.0,@21.5] timeOption:MPMovieTimeOptionNearestKeyFrame];
}

#pragma mark - 控制器通知
/**
 *  添加通知监控媒体播放控制器状态
 */
-(void)addNotification{
    NSNotificationCenter *notificationCenter=[NSNotificationCenter defaultCenter];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackStateChange:) name:MPMoviePlayerPlaybackStateDidChangeNotification object:self.moviePlayer];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackFinished:) name:MPMoviePlayerPlaybackDidFinishNotification object:self.moviePlayer];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerThumbnailRequestFinished:) name:MPMoviePlayerThumbnailImageRequestDidFinishNotification object:self.moviePlayer];
    
}

/**
 *  播放状态改变，注意播放完成时的状态是暂停
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackStateChange:(NSNotification *)notification{
    switch (self.moviePlayer.playbackState) {
        case MPMoviePlaybackStatePlaying:
            NSLog(@"正在播放...");
            break;
        case MPMoviePlaybackStatePaused:
            NSLog(@"暂停播放.");
            break;
        case MPMoviePlaybackStateStopped:
            NSLog(@"停止播放.");
            break;
        default:
            NSLog(@"播放状态:%li",self.moviePlayer.playbackState);
            break;
    }
}

/**
 *  播放完成
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackFinished:(NSNotification *)notification{
    NSLog(@"播放完成.%li",self.moviePlayer.playbackState);
}

/**
 *  缩略图请求完成,此方法每次截图成功都会调用一次
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerThumbnailRequestFinished:(NSNotification *)notification{
    NSLog(@"视频截图完成.");
    UIImage *image=notification.userInfo[MPMoviePlayerThumbnailImageKey];
    //保存图片到相册(首次调用会请求用户获得访问相册权限)
    UIImageWriteToSavedPhotosAlbum(image, nil, nil, nil);
}

@end
截图效果：
MPMoviePlayerController_Thumbnail1     MPMoviePlayerController_Thumbnail2
扩展--使用AVFoundation生成缩略图
通过前面的方法大家应该已经看到，使用MPMoviePlayerController来生成缩略图足够简单，但是如果仅仅是是为了生成缩略图而不进行视频播放的话，此刻使用MPMoviePlayerController就有点大材小用了。其实使用AVFundation框架中的AVAssetImageGenerator就可以获取视频缩略图。使用AVAssetImageGenerator获取缩略图大致分为三个步骤：
创建AVURLAsset对象（此类主要用于获取媒体信息，包括视频、声音等）。
根据AVURLAsset创建AVAssetImageGenerator对象。
使用AVAssetImageGenerator的copyCGImageAtTime::方法获得指定时间点的截图。
//
//  ViewController.m
//  AVAssetImageGenerator
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    //获取第13.0s的缩略图
    [self thumbnailImageRequest:13.0];
}

#pragma mark - 私有方法
/**
 *  取得本地文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getFileUrl{
    NSString *urlStr=[[NSBundle mainBundle] pathForResource:@"The New Look of OS X Yosemite.mp4" ofType:nil];
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}

/**
 *  取得网络文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getNetworkUrl{
    NSString *urlStr=@"http://192.168.1.161/The New Look of OS X Yosemite.mp4";
    urlStr=[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSURL *url=[NSURL URLWithString:urlStr];
    return url;
}

/**
 *  截取指定时间的视频缩略图
 *
 *  @param timeBySecond 时间点
 */
-(void)thumbnailImageRequest:(CGFloat )timeBySecond{
    //创建URL
    NSURL *url=[self getNetworkUrl];
    //根据url创建AVURLAsset
    AVURLAsset *urlAsset=[AVURLAsset assetWithURL:url];
    //根据AVURLAsset创建AVAssetImageGenerator
    AVAssetImageGenerator *imageGenerator=[AVAssetImageGenerator assetImageGeneratorWithAsset:urlAsset];
    /*截图
     * requestTime:缩略图创建时间
     * actualTime:缩略图实际生成的时间
     */
    NSError *error=nil;
    CMTime time=CMTimeMakeWithSeconds(timeBySecond, 10);//CMTime是表示电影时间信息的结构体，第一个参数表示是视频第几秒，第二个参数表示每秒帧数.(如果要活的某一秒的第几帧可以使用CMTimeMake方法)
    CMTime actualTime;
    CGImageRef cgImage= [imageGenerator copyCGImageAtTime:time actualTime:&actualTime error:&error];
    if(error){
        NSLog(@"截取视频缩略图时发生错误，错误信息：%@",error.localizedDescription);
        return;
    }
    CMTimeShow(actualTime);
    UIImage *image=[UIImage imageWithCGImage:cgImage];//转化为UIImage
    //保存到相册
    UIImageWriteToSavedPhotosAlbum(image,nil, nil, nil);
    CGImageRelease(cgImage);
}

@end
生成的缩略图效果：
AVAssetImageGenerator_Thumbnail
MPMoviePlayerViewController
其实MPMoviePlayerController如果不作为嵌入视频来播放（例如在新闻中嵌入一个视频），通常在播放时都是占满一个屏幕的，特别是在iPhone、iTouch上。因此从iOS3.2以后苹果也在思考既然MPMoviePlayerController在使用时通常都是将其视图view添加到另外一个视图控制器中作为子视图，那么何不直接创建一个控制器视图内部创建一个MPMoviePlayerController属性并且默认全屏播放，开发者在开发的时候直接使用这个视图控制器。这个内部有一个MPMoviePlayerController的视图控制器就是MPMoviePlayerViewController，它继承于UIViewController。MPMoviePlayerViewController内部多了一个moviePlayer属性和一个带有url的初始化方法，同时它内部实现了一些作为模态视图展示所特有的功能，例如默认是全屏模式展示、弹出后自动播放、作为模态窗口展示时如果点击“Done”按钮会自动退出模态窗口等。在下面的示例中就不直接将播放器放到主视图控制器，而是放到一个模态视图控制器中，简单演示MPMoviePlayerViewController的使用。
//
//  ViewController.m
//  MPMoviePlayerViewController
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//  MPMoviePlayerViewController使用

#import "ViewController.h"
#import <MediaPlayer/MediaPlayer.h>

@interface ViewController ()

//播放器视图控制器
@property (nonatomic,strong) MPMoviePlayerViewController *moviePlayerViewController;

@end

@implementation ViewController

#pragma mark - 控制器视图方法
- (void)viewDidLoad {
    [super viewDidLoad];

}

-(void)dealloc{
    //移除所有通知监控
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}


#pragma mark - 私有方法
/**
 *  取得本地文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getFileUrl{
    NSString *urlStr=[[NSBundle mainBundle] pathForResource:@"The New Look of OS X Yosemite.mp4" ofType:nil];
    NSURL *url=[NSURL fileURLWithPath:urlStr];
    return url;
}

/**
 *  取得网络文件路径
 *
 *  @return 文件路径
 */
-(NSURL *)getNetworkUrl{
    NSString *urlStr=@"http://192.168.1.161/The New Look of OS X Yosemite.mp4";
    urlStr=[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSURL *url=[NSURL URLWithString:urlStr];
    return url;
}

-(MPMoviePlayerViewController *)moviePlayerViewController{
    if (!_moviePlayerViewController) {
        NSURL *url=[self getNetworkUrl];
        _moviePlayerViewController=[[MPMoviePlayerViewController alloc]initWithContentURL:url];
        [self addNotification];
    }
    return _moviePlayerViewController;
}
#pragma mark - UI事件
- (IBAction)playClick:(UIButton *)sender {
    self.moviePlayerViewController=nil;//保证每次点击都重新创建视频播放控制器视图，避免再次点击时由于不播放的问题
//    [self presentViewController:self.moviePlayerViewController animated:YES completion:nil];
    //注意，在MPMoviePlayerViewController.h中对UIViewController扩展两个用于模态展示和关闭MPMoviePlayerViewController的方法，增加了一种下拉展示动画效果
    [self presentMoviePlayerViewControllerAnimated:self.moviePlayerViewController];
}

#pragma mark - 控制器通知
/**
 *  添加通知监控媒体播放控制器状态
 */
-(void)addNotification{
    NSNotificationCenter *notificationCenter=[NSNotificationCenter defaultCenter];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackStateChange:) name:MPMoviePlayerPlaybackStateDidChangeNotification object:self.moviePlayerViewController.moviePlayer];
    [notificationCenter addObserver:self selector:@selector(mediaPlayerPlaybackFinished:) name:MPMoviePlayerPlaybackDidFinishNotification object:self.moviePlayerViewController.moviePlayer];
    
}

/**
 *  播放状态改变，注意播放完成时的状态是暂停
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackStateChange:(NSNotification *)notification{
    switch (self.moviePlayerViewController.moviePlayer.playbackState) {
        case MPMoviePlaybackStatePlaying:
            NSLog(@"正在播放...");
            break;
        case MPMoviePlaybackStatePaused:
            NSLog(@"暂停播放.");
            break;
        case MPMoviePlaybackStateStopped:
            NSLog(@"停止播放.");
            break;
        default:
            NSLog(@"播放状态:%li",self.moviePlayerViewController.moviePlayer.playbackState);
            break;
    }
}

/**
 *  播放完成
 *
 *  @param notification 通知对象
 */
-(void)mediaPlayerPlaybackFinished:(NSNotification *)notification{
    NSLog(@"播放完成.%li",self.moviePlayerViewController.moviePlayer.playbackState);
}

@end
运行效果：
MPMoviePlayerViewController
这里需要强调一下，由于MPMoviePlayerViewController的初始化方法做了大量工作（例如设置URL、自动播放、添加点击Done完成的监控等），所以当再次点击播放弹出新的模态窗口的时如果不销毁之前的MPMoviePlayerViewController，那么新的对象就无法完成初始化，这样也就不能再次进行播放。
AVPlayer
MPMoviePlayerController足够强大，几乎不用写几行代码就能完成一个播放器，但是正是由于它的高度封装使得要自定义这个播放器变得很复杂，甚至是不可能完成。例如有些时候需要自定义播放器的样式，那么如果要使用MPMoviePlayerController就不合适了，如果要对视频有自由的控制则可以使用AVPlayer。AVPlayer存在于AVFoundation中，它更加接近于底层，所以灵活性也更强：
AVFoundation_Framework
AVPlayer本身并不能显示视频，而且它也不像MPMoviePlayerController有一个view属性。如果AVPlayer要显示必须创建一个播放器层AVPlayerLayer用于展示，播放器层继承于CALayer，有了AVPlayerLayer之添加到控制器视图的layer中即可。要使用AVPlayer首先了解一下几个常用的类：
AVAsset：主要用于获取多媒体信息，是一个抽象类，不能直接使用。
AVURLAsset：AVAsset的子类，可以根据一个URL路径创建一个包含媒体信息的AVURLAsset对象。
AVPlayerItem：一个媒体资源管理对象，管理者视频的一些基本信息和状态，一个AVPlayerItem对应着一个视频资源。
下面简单通过一个播放器来演示AVPlayer的使用，播放器的效果如下：
AVPlayer_Thumbnail
在这个自定义的播放器中实现了视频播放、暂停、进度展示和视频列表功能，下面将对这些功能一一介绍。
首先说一下视频的播放、暂停功能，这也是最基本的功能，AVPlayer对应着两个方法play、pause来实现。但是关键问题是如何判断当前视频是否在播放，在前面的内容中无论是音频播放器还是视频播放器都有对应的状态来判断，但是AVPlayer却没有这样的状态属性，通常情况下可以通过判断播放器的播放速度来获得播放状态。如果rate为0说明是停止状态，1是则是正常播放状态。
其次要展示播放进度就没有其他播放器那么简单了。在前面的播放器中通常是使用通知来获得播放器的状态，媒体加载状态等，但是无论是AVPlayer还是AVPlayerItem（AVPlayer有一个属性currentItem是AVPlayerItem类型，表示当前播放的视频对象）都无法获得这些信息。当然AVPlayerItem是有通知的，但是对于获得播放状态和加载状态有用的通知只有一个：播放完成通知AVPlayerItemDidPlayToEndTimeNotification。在播放视频时，特别是播放网络视频往往需要知道视频加载情况、缓冲情况、播放情况，这些信息可以通过KVO监控AVPlayerItem的status、loadedTimeRanges属性来获得。当AVPlayerItem的status属性为AVPlayerStatusReadyToPlay是说明正在播放，只有处于这个状态时才能获得视频时长等信息；当loadedTimeRanges的改变时（每缓冲一部分数据就会更新此属性）可以获得本次缓冲加载的视频范围（包含起始时间、本次加载时长），这样一来就可以实时获得缓冲情况。然后就是依靠AVPlayer的- (id)addPeriodicTimeObserverForInterval:(CMTime)interval queue:(dispatch_queue_t)queue usingBlock:(void (^)(CMTime time))block方法获得播放进度，这个方法会在设定的时间间隔内定时更新播放进度，通过time参数通知客户端。相信有了这些视频信息播放进度就不成问题了，事实上通过这些信息就算是平时看到的其他播放器的缓冲进度显示以及拖动播放的功能也可以顺利的实现。
最后就是视频切换的功能，在前面介绍的所有播放器中每个播放器对象一次只能播放一个视频，如果要切换视频只能重新创建一个对象，但是AVPlayer却提供了- (void)replaceCurrentItemWithPlayerItem:(AVPlayerItem *)item方法用于在不同的视频之间切换（事实上在AVFoundation内部还有一个AVQueuePlayer专门处理播放列表切换，有兴趣的朋友可以自行研究，这里不再赘述）。
下面附上代码：
//
//  ViewController.m
//  AVPlayer
//
//  Created by Kenshin Cui on 14/03/30.
//  Copyright (c) 2014年 cmjstudio. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>

@interface ViewController ()

@property (nonatomic,strong) AVPlayer *player;//播放器对象

@property (weak, nonatomic) IBOutlet UIView *container; //播放器容器
@property (weak, nonatomic) IBOutlet UIButton *playOrPause; //播放/暂停按钮
@property (weak, nonatomic) IBOutlet UIProgressView *progress;//播放进度

@end

@implementation ViewController

#pragma mark - 控制器视图方法
- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupUI];
    [self.player play];
}

-(void)dealloc{
    [self removeObserverFromPlayerItem:self.player.currentItem];
    [self removeNotification];
}

#pragma mark - 私有方法
-(void)setupUI{
    //创建播放器层
    AVPlayerLayer *playerLayer=[AVPlayerLayer playerLayerWithPlayer:self.player];
    playerLayer.frame=self.container.frame;
    //playerLayer.videoGravity=AVLayerVideoGravityResizeAspect;//视频填充模式
    [self.container.layer addSublayer:playerLayer];
}

/**
 *  截取指定时间的视频缩略图
 *
 *  @param timeBySecond 时间点
 */

/**
 *  初始化播放器
 *
 *  @return 播放器对象
 */
-(AVPlayer *)player{
    if (!_player) {
        AVPlayerItem *playerItem=[self getPlayItem:0];
        _player=[AVPlayer playerWithPlayerItem:playerItem];
        [self addProgressObserver];
        [self addObserverToPlayerItem:playerItem];
    }
    return _player;
}

/**
 *  根据视频索引取得AVPlayerItem对象
 *
 *  @param videoIndex 视频顺序索引
 *
 *  @return AVPlayerItem对象
 */
-(AVPlayerItem *)getPlayItem:(int)videoIndex{
    NSString *urlStr=[NSString stringWithFormat:@"http://192.168.1.161/%i.mp4",videoIndex];
    urlStr =[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSURL *url=[NSURL URLWithString:urlStr];
    AVPlayerItem *playerItem=[AVPlayerItem playerItemWithURL:url];
    return playerItem;
}
#pragma mark - 通知
/**
 *  添加播放器通知
 */
-(void)addNotification{
    //给AVPlayerItem添加播放完成通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(playbackFinished:) name:AVPlayerItemDidPlayToEndTimeNotification object:self.player.currentItem];
}

-(void)removeNotification{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}

/**
 *  播放完成通知
 *
 *  @param notification 通知对象
 */
-(void)playbackFinished:(NSNotification *)notification{
    NSLog(@"视频播放完成.");
}

#pragma mark - 监控
/**
 *  给播放器添加进度更新
 */
-(void)addProgressObserver{
    AVPlayerItem *playerItem=self.player.currentItem;
    UIProgressView *progress=self.progress;
    //这里设置每秒执行一次
    [self.player addPeriodicTimeObserverForInterval:CMTimeMake(1.0, 1.0) queue:dispatch_get_main_queue() usingBlock:^(CMTime time) {
        float current=CMTimeGetSeconds(time);
        float total=CMTimeGetSeconds([playerItem duration]);
        NSLog(@"当前已经播放%.2fs.",current);
        if (current) {
            [progress setProgress:(current/total) animated:YES];
        }
    }];
}

/**
 *  给AVPlayerItem添加监控
 *
 *  @param playerItem AVPlayerItem对象
 */
-(void)addObserverToPlayerItem:(AVPlayerItem *)playerItem{
    //监控状态属性，注意AVPlayer也有一个status属性，通过监控它的status也可以获得播放状态
    [playerItem addObserver:self forKeyPath:@"status" options:NSKeyValueObservingOptionNew context:nil];
    //监控网络加载情况属性
    [playerItem addObserver:self forKeyPath:@"loadedTimeRanges" options:NSKeyValueObservingOptionNew context:nil];
}
-(void)removeObserverFromPlayerItem:(AVPlayerItem *)playerItem{
    [playerItem removeObserver:self forKeyPath:@"status"];
    [playerItem removeObserver:self forKeyPath:@"loadedTimeRanges"];
}
/**
 *  通过KVO监控播放器状态
 *
 *  @param keyPath 监控属性
 *  @param object  监视器
 *  @param change  状态改变
 *  @param context 上下文
 */
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context{
    AVPlayerItem *playerItem=object;
    if ([keyPath isEqualToString:@"status"]) {
        AVPlayerStatus status= [[change objectForKey:@"new"] intValue];
        if(status==AVPlayerStatusReadyToPlay){
            NSLog(@"正在播放...，视频总长度:%.2f",CMTimeGetSeconds(playerItem.duration));
        }
    }else if([keyPath isEqualToString:@"loadedTimeRanges"]){
        NSArray *array=playerItem.loadedTimeRanges;
        CMTimeRange timeRange = [array.firstObject CMTimeRangeValue];//本次缓冲时间范围
        float startSeconds = CMTimeGetSeconds(timeRange.start);
        float durationSeconds = CMTimeGetSeconds(timeRange.duration);
        NSTimeInterval totalBuffer = startSeconds + durationSeconds;//缓冲总长度
        NSLog(@"共缓冲：%.2f",totalBuffer);
//
    }
}

#pragma mark - UI事件
/**
 *  点击播放/暂停按钮
 *
 *  @param sender 播放/暂停按钮
 */
- (IBAction)playClick:(UIButton *)sender {
//    AVPlayerItemDidPlayToEndTimeNotification
    //AVPlayerItem *playerItem= self.player.currentItem;
    if(self.player.rate==0){ //说明时暂停
        [sender setImage:[UIImage imageNamed:@"player_pause"] forState:UIControlStateNormal];
        [self.player play];
    }else if(self.player.rate==1){//正在播放
        [self.player pause];
        [sender setImage:[UIImage imageNamed:@"player_play"] forState:UIControlStateNormal];
    }
}


/**
 *  切换选集，这里使用按钮的tag代表视频名称
 *
 *  @param sender 点击按钮对象
 */
- (IBAction)navigationButtonClick:(UIButton *)sender {
    [self removeNotification];
    [self removeObserverFromPlayerItem:self.player.currentItem];
    AVPlayerItem *playerItem=[self getPlayItem:sender.tag];
    [self addObserverToPlayerItem:playerItem];
    //切换视频
    [self.player replaceCurrentItemWithPlayerItem:playerItem];
    [self addNotification];
}

@end
运行效果：
AVPlayer
到目前为止无论是MPMoviePlayerController还是AVPlayer来播放视频都相当强大，但是它也存在着一些不可回避的问题，那就是支持的视频编码格式很有限：H.264、MPEG-4，扩展名（压缩格式）：.mp4、.mov、.m4v、.m2v、.3gp、.3g2等。但是无论是MPMoviePlayerController还是AVPlayer它们都支持绝大多数音频编码，所以大家如果纯粹是为了播放音乐的话也可以考虑使用这两个播放器。那么如何支持更多视频编码格式呢？目前来说主要还是依靠第三方框架，在iOS上常用的视频编码、解码框架有：VLC、ffmpeg， 具体使用方式今天就不再做详细介绍。
```

