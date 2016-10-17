---
title: MPMoviePlayerViewController和MPMoviePlayerController
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
- (void)viewDidLoad {
    [super viewDidLoad];

    [self playVideo:[NSURL URLWithString:self.playURL]];

}
//根据视频url播放视频
- (void) playVideo:(NSURL *) movieURL
{
    MPMoviePlayerViewController *playerViewController = [[MPMoviePlayerViewController alloc]     initWithContentURL:movieURL];
    self.playerViewController = playerViewController;
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(playVideoFinished:) name:MPMoviePlayerPlaybackDidFinishNotification object:[playerViewController moviePlayer]];
    playerViewController.modalTransitionStyle = UIModalTransitionStyleFlipHorizontal;
    [self.view addSubview:playerViewController.view];
    MPMoviePlayerController *player = [playerViewController moviePlayer];
    [player play];
}

//当点击Done按键或者播放完毕时调用此函数
- (void) playVideoFinished:(NSNotification *)theNotification
{
    MPMoviePlayerController *player = [theNotification object];
    [[NSNotificationCenter defaultCenter] removeObserver:self name:MPMoviePlayerPlaybackDidFinishNotification object:player];
    [player stop];

    [self dismissViewControllerAnimated:YES completion:^{
       
       
    }];
}
```

MPMoviePlayerViewController和MPMoviePlayerController 使用场合不一样MPMoviePlayerViewController是在iOS3.2以后的平台上使用。MPMoviePlayerController在3.2之前使用，虽然在3.2之后也能使用，但是使用方法略有改变，建议3.2之后使用MPMoviePlayerViewController。
3.2之后,MPMoviePlayerController作为MPMoviePlayerViewController的一个属性存在。

以下是使用MPMoviePlayerViewController播放视频的代码:


去掉系统上的按钮导航条 self.play.controlStyle = MPMovieControlStyleNone;