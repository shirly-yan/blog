---
title: UITextField
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
  //创建 
     UITextField*textfield = [[UITextField             alloc]initWithFrame:CGRectMake(20,20,200,40)];
    [self.window addSubview:self.myTextField];
   
    //设置背景颜色
    self.myTextField.backgroundColor = [UIColor clearColor];
   
    //设置输入框样式
    self.myTextField.borderStyle = UITextBorderStyleRoundedRect;
   
    //密文输入
    self.myTextField.secureTextEntry = YES;
   
    //设置占位提示符
    self.myTextField.placeholder = @"请输入";
   
    //设置输入框删除按钮
    self.myTextField.clearButtonMode = UITextFieldViewModeAlways;
   
    //设置键盘类型
    self.myTextField.keyboardType = UIKeyboardTypeNumberPad;


/******按按钮键盘隐藏*********************************/
  //创建按钮
    UIButton *Button = [UIButton buttonWithType:UIButtonTypeCustom];
    [self.window addSubview:Button];
    Button.frame = CGRectMake(20, 80, 100, 30);
    Button.backgroundColor = [UIColor cyanColor];

    [Button addTarget:self action:@selector(buttonAction:) forControlEvents:UIControlEventTouchUpInside];

-(void) buttonAction:(UIButton *)b
{
    [self.myTextField resignFirstResponder];
    //取消第一响应者
}
/******************************************************/



/******按return键盘隐藏*********************************/
1.签UITextFieldDelegate协议
@interface AppDelegate : UIResponder <UIApplicationDelegate,UITextFieldDelegate>

2.成为代理人
哪个输入框成为代理人哪个输入框有此功能
 self.myTextField.delegate = self;

3.实现协议里的方法
//点击return建的时候被触发
通用，一个就可以给所有输入框用
- (BOOL)textFieldShouldReturn:(UITextField *)textField
{
    [self.myTextField resignFirstResponder];
    return YES;

}
/******************************************************/

/******按空白处键盘隐藏*********************************/
 //按空白处键盘回收
    UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc]initWithTarget:self action:@selector(backgroundTap:)];
    [self.view addGestureRecognizer:tap];

//按空白处键盘回收调用的方法
- (IBAction)backgroundTap:(id)sender {
    [self.studentDetailV.tf1  resignFirstResponder];
    [self.studentDetailV.tf2  resignFirstResponder];
    [self.studentDetailV.tf3  resignFirstResponder];
    [self.studentDetailV.tf4  resignFirstResponder];
    [self.studentDetailV.tf5  resignFirstResponder];
   
}
/******************************************************/




```

