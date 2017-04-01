---
title: MessageUI.framework发短信
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
#import "ViewController.h"
#import <MessageUI/MessageUI.h>
@interface ViewController ()<MFMessageComposeViewControllerDelegate>

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
   
   
   
    UIButton *b = [UIButton buttonWithType:UIButtonTypeCustom];
    [self.view addSubview:b];
    b.frame = CGRectMake(100, 100, 200, 100);
    b.backgroundColor = [UIColor blackColor];
    [b setTitle:@"sendMessage" forState:UIControlStateNormal];
    [b addTarget:self action:@selector(buttonAction) forControlEvents:UIControlEventTouchUpInside];
   
}
-(void)buttonAction
{

    if (![MFMessageComposeViewController canSendText])
    {
        NSLog(@"当前设备不能发短信");
        return;
       
    }else
    {
   
    MFMessageComposeViewController *messageVC = [[MFMessageComposeViewController alloc]init];
   
    messageVC.recipients = @[@"10086",@"10010"];
    messageVC.body = @"大水杯";
    messageVC.messageComposeDelegate = self;
   
    [self presentViewController:messageVC animated:YES completion:^{
       
       
    }];
    }
}

-(void)messageComposeViewController:(MFMessageComposeViewController *)controller didFinishWithResult:(MessageComposeResult)result
{
   
    [self dismissViewControllerAnimated:YES completion:^{
       
       
    }];
   
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

@end

```

