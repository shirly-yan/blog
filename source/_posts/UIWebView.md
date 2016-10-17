---
title: UIWebView
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
@property(nonatomic,retain)UIWebView *webView;

    //创建
    self.webView = [[UIWebViewalloc]initWithFrame:[[UIScreenmainScreen] bounds]];
    [self.view addSubview:self.webView];

   //要前往的URL
   NSString *str = @"http://www.youku.com";
    NSURL *url = [NSURL URLWithString:str];
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    [self.webView loadRequest:request];

   //成为代理
    self.webView.delegate = self;

   //自定义导航栏左右按钮
    self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc]initWithTitle:@"后退" style:UIBarButtonItemStylePlain target:self action:@selector(leftAction)];
   
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc]initWithTitle:@"前进" style:UIBarButtonItemStylePlain target:self action:@selector(rightAction)];
   
   
   
    UITextField *textf = [[UITextField alloc] initWithFrame:CGRectMake(0, 0, 150, 30)];
    textf.borderStyle = UITextBorderStyleRoundedRect;
    textf.clearButtonMode = UITextFieldViewModeAlways;
    self.navigationItem.titleView = textf;
    textf.delegate = self;



//后退
-(void)leftAction
{
    [self.webView goBack];
}
//前进
-(void)rightAction
{
    [self.webView goForward];
}


UITextFieldDelegate协议方法
-(BOOL)textFieldShouldReturn:(UITextField *)textField
{
   
   
    if ([textField.text hasPrefix:@"http://www."]) {
       
        NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:textField.text]];
        [self.webView loadRequest:request];
       
    }else
    {
        NSString *str = [NSString stringWithFormat:@"http://www.%@",textField.text];
        NSURLRequest *request = [NSURLRequest requestWithURL:[NSURL URLWithString:str]];
        [self.webView loadRequest:request];
    }
    [textField resignFirstResponder];
    return YES;

}

```

