---
title: UITextView
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];
   
    self.textView = [[UITextView alloc]initWithFrame:CGRectMake(0, 667-80, 375, 80)];
   
    self.textView.delegate = self;
   
    self.textView.font = [UIFont systemFontOfSize:20];
   
    self.textView.textColor = [UIColor blackColor];
   
    self.textView.contentInset = UIEdgeInsetsMake(0, 0, 0, 0);//添加滚动区域

    self.textView.text = @"习大大";
   
    self.textView.scrollEnabled = YES;//是否可滚动

    self.textView.editable = YES;//是否可编辑
   
    self.textView.returnKeyType = UIReturnKeyDefault;//返回键的类型
   
    self.textView.keyboardType = UIKeyboardTypeDefault;//键盘类型
   
    self.textView.scrollEnabled = YES;//是否可以拖动
   
    self.textView.autoresizingMask = UIViewAutoresizingFlexibleHeight;//自适应高度
   
    self.textView.layer.cornerRadius = 8; //边框设置为圆角
   
    self.textView.layer.borderWidth = 1;
   
//    设置边框颜色 参数2是一个数组 分别为三基色和alpha分量；
    CGFloat color[]={0.5, 0.5, 0.5, 1};
    self.textView.layer.borderColor = CGColorCreate(CGColorSpaceCreateDeviceRGB(),color);
    self.textView.layer.masksToBounds = YES;
   
//    [self.textView becomeFirstResponder];//获得焦点 程序运行就获取光标

    self.myView = [[UIView alloc]initWithFrame:[[UIScreen mainScreen]bounds]];
    self.myView.tag = 1000;
   
    [self.view addSubview:self.myView];
   
    [self.myView addSubview:self.textView];
   
    //注册通知,监听键盘弹出事件
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardDidShow:) name:UIKeyboardDidShowNotification object:nil];
   
    //注册通知,监听键盘消失事件
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardDidHidden) name:UIKeyboardDidHideNotification object:nil];
}

//将要开始编辑
- (BOOL)textViewShouldBeginEditing:(UITextView *)textView{
   
    return YES;
}

//将要结束编辑
- (BOOL)textViewShouldEndEditing:(UITextView *)textView{
   
    return YES;
}

//开始编辑
- (void)textViewDidBeginEditing:(UITextView *)textView{
   
}

//结束编辑
- (void)textViewDidEndEditing:(UITextView *)textView{
   
}

//焦点发生改变
- (void)textViewDidChangeSelection:(UITextView *)textView{
   
}

//内容发生改变编辑
- (void)textViewDidChange:(UITextView *)textView{

}

//内容将要发生改变编辑

//控制输入文字的长度和内容，可通调用以下代理方法实现
- (BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange)range replacementText:(NSString *)text{
   
    if (range.location>=100){
        //控制输入文本的长度
        return  NO;
    }
    if ([text isEqualToString:@"\n"]) {
        //禁止输入换行
        return NO;
    }
    else{
        return YES;
    }
}

// 键盘弹出时
-(void)keyboardDidShow:(NSNotification *)notification{
   
    //获取键盘高度
    NSValue *keyboardObject = [[notification userInfo] objectForKey:UIKeyboardFrameEndUserInfoKey];
   
    CGRect keyboardRect;
   
    [keyboardObject getValue:&keyboardRect];
   
    //调整放置有textView的view的位置
   
    //设置动画
    [UIView beginAnimations:nil context:nil];
   
    //定义动画时间
    [UIView setAnimationDuration:kAnimationDuration];
   
    //设置view的frame，往上平移
    [(UIView *)[self.view viewWithTag:1000] setFrame:CGRectMake(0, self.view.frame.size.height-keyboardRect.size.height-kViewHeight, 375, kViewHeight)];
   
    [UIView commitAnimations];
}

//键盘消失时
-(void)keyboardDidHidden
{
    //定义动画
    [UIView beginAnimations:nil context:nil];
    [UIView setAnimationDuration:kAnimationDuration];
    //设置view的frame，往下平移
    [(UIView *)[self.view viewWithTag:1000] setFrame:CGRectMake(0, self.view.frame.size.height-kViewHeight, 375, kViewHeight)];
    [UIView commitAnimations];
}

-(void)touchesBegan:(NSSet *)touches withEvent:(UIEvent *)event{
   
    [self.textView resignFirstResponder];
}
@end
```

