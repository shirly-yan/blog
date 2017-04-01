---
title: UIAlertController
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

   //初始化方法
    + (instancetype)alertControllerWithTitle:(nullableNSString *)title message:(nullableNSString *)message preferredStyle:(UIAlertControllerStyle)preferredStyle;
   
   //UIAlertControllerStyle
   typedefNS_ENUM(NSInteger, UIAlertControllerStyle) {
            UIAlertControllerStyleActionSheet =0,            UIAlertControllerStyleAlert     }
   /**************自动消失的UIAlertController*********/   //创建UIAlertController
   UIAlertController*alertController = [UIAlertControlleralertControllerWithTitle:@"标题"message:@"UIAlertController"preferredStyle:UIAlertControllerStyleAlert];

    [selfpresentViewController:alertControlleranimated:YEScompletion:nil];

   //线程2秒后执行
   dispatch_after(dispatch_time(DISPATCH_TIME_NOW,
 (int64_t)(2.f*NSEC_PER_SEC)),dispatch_get_main_queue(), ^{
       
        [alertControllerdismissViewControllerAnimated:YEScompletion:^{
           
           
        }];
    });   /**************自动消失的UIAlertController*********/ 
 /*
     ******添加按钮******
     Title :标题名称
     style :样式[Cancle(取消)
 Default(默认的) destructive(重置)]
     handler:处理程序(点击按钮执行的代码)
     */
   
   /**************带按钮的的UIAlertController*********/
   //创建UIAlertController
   UIAlertController*alertController = [UIAlertControlleralertControllerWithTitle:@"标题"message:@"UIAlertController"preferredStyle:UIAlertControllerStyleAlert];
   
   //创建"取消"样式按钮
   UIAlertAction*cancelAction = [UIAlertActionactionWithTitle:@"取消"style:UIAlertActionStyleCancelhandler:^(UIAlertAction*action) {
       //添加点击事件
       self.view.backgroundColor= [UIColoryellowColor];
       
    }];
   
   //创建"默认"样式按钮
   UIAlertAction*defaultAction = [UIAlertActionactionWithTitle:@"默认default"style:UIAlertActionStyleDefaulthandler:^(UIAlertAction*_Nonnullaction) {
       
       
    }];
   
   //创建“警示”样式按钮
   UIAlertAction*destructiveAction = [UIAlertActionactionWithTitle:@"重置deatructive" style:UIAlertActionStyleDestructivehandler:^(UIAlertAction*_Nonnullaction) {
       
       
    }];

   //将按钮添加到alertController上
    [alertControlleraddAction:cancelAction];
    [alertControlleraddAction:defaultAction];
    [alertControlleraddAction:destructiveAction];

   
    [selfpresentViewController:alertControlleranimated:YEScompletion:nil];
   /**************带按钮的的UIAlertController*********/


/**************带输入框的UIAlertController*********/   //创建UIAlertController
   UIAlertController*alertController = [UIAlertControlleralertControllerWithTitle:@"文本对话框"message:@"登录和密码对话框示例"preferredStyle:UIAlertControllerStyleAlert];
   
   //添加输入框
    [alertControlleraddTextFieldWithConfigurationHandler:^(UITextField*textField){
        textField.placeholder=@"登录";
       
       //添加通知，监听输入框的变化
        [[NSNotificationCenterdefaultCenter]addObserver:selfselector:@selector(alertTextFieldDidChange:)name:UITextFieldTextDidChangeNotificationobject:textField];
    }];
   
    [alertControlleraddTextFieldWithConfigurationHandler:^(UITextField*textField) {
        textField.placeholder=@"密码";
        textField.secureTextEntry=YES;
    }];
   
   //添加按钮
   UIAlertAction*okAction = [UIAlertActionactionWithTitle:@"好的"style:UIAlertActionStyleDefaulthandler:^(UIAlertAction*action) {
       
       //获取到输入框的内容
       UITextField*login = alertController.textFields.firstObject;
       UITextField*password = alertController.textFields.lastObject;
       
       //移除通知
        [[NSNotificationCenterdefaultCenter]removeObserver:selfname:UITextFieldTextDidChangeNotificationobject:nil];
       
    }];
   
   //冻结按钮
    okAction.enabled=NO;
    [alertControlleraddAction:okAction];
   
    [selfpresentViewController:alertControlleranimated:YEScompletion:^{
       
       
    }];
   /**************带输入框的UIAlertController*********/
}
//通知触发的方法
- (void)alertTextFieldDidChange:(NSNotification*)notification{
   UIAlertController*alertController = (UIAlertController*)self.presentedViewController;
   
   if(alertController) {
       UITextField*login = alertController.textFields.firstObject;
       UIAlertAction*okAction = alertController.actions.lastObject;
       
       //当输入的字数大于2时解冻
        okAction.enabled= login.text.length>2;
    }
}
