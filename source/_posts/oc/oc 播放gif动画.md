---
title: oc 播放gif动画
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
// 设定位置和大小
    CGRect frame = CGRectMake(50,50,0,0);
    frame.size = [UIImage imageNamed:@"load.gif"].size;
    // 读取gif图片数据
    NSData *gif = [NSData dataWithContentsOfFile: [[NSBundle mainBundle] pathForResource:@"load" ofType:@"gif"]];
    // view生成
    UIWebView *webView = [[UIWebView alloc] initWithFrame:frame];
    webView.userInteractionEnabled = NO;//用户不可交互
    [webView loadData:gif MIMEType:@"image/gif" textEncodingName:nil baseURL:nil];
    [self.view addSubview:webView];
```

