---
title: UIImagePickerController从本地相册加载图片
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->
/*1.签两个协议协议
          UIImagePickerControllerDelegate
          UINavigationControllerDelegate
  2.两个方法

方法一：
//点击按钮触发的方法
-(void)buttonAction:(UIButton*)button
{

    //创建获取本地相册控制器对象
    UIImagePickerController *picker = [[UIImagePickerControlleralloc]init];
   
    //成为代理
    picker.delegate= self;
   
    //设置相片来源类型
    picker.sourceType=UIImagePickerControllerSourceTypePhotoLibrary;
   
    //是否可以编辑
    picker.allowsEditing= YES;
   
    //模态显示本地相册
    [selfpresentViewController:pickeranimated:YEScompletion:^{
       
       
    }]; 
}

方法二：
//当用户选择相册中的某个图片时触发的方法,协议里的方法
//该方法可以获取到选中的图片

- (void)imagePickerController:(UIImagePickerController*)picker didFinishPickingMediaWithInfo:(NSDictionary*)info
{

    NSLog(@"info = %@",info);
    UIImage *image=
    [info objectForKey:@"UIImagePickerControllerOriginalImage"];
    //通过tag值取对象
    UIButton *button = (UIButton*)[self.viewviewWithTag:10000];
    [button setBackgroundImage:imageforState:UIControlStateNormal];
   
    //让相册模态消失
    [picker dismissViewControllerAnimated:YEScompletion:^{
       
       
    }];
   
}
数据来源类型一共有三种：
enum {
   UIImagePickerControllerSourceTypePhotoLibrary ,//来自图库
   UIImagePickerControllerSourceTypeCamera ,//来自相机
   UIImagePickerControllerSourceTypeSavedPhotosAlbum //来自相册
};
在用这些来源的时候最好检测以下设备是否支持；
 if([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera])
    {
        NSLog(@"支持相机");
    }
    if([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypePhotoLibrary])
    {
        NSLog(@"支持图库");
    }
    if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeSavedPhotosAlbum])
    {
        NSLog(@"支持相片库");
    }

调用摄像头来获取资源
- (void)viewDidLoad {
    [super viewDidLoad];
    picker = [[UIImagePickerController alloc]init];
    picker.view.backgroundColor = [UIColor orangeColor];
    UIImagePickerControllerSourceType sourcheType = UIImagePickerControllerSourceTypeCamera;
    picker.sourceType = sourcheType;
    picker.delegate = self;
    picker.allowsEditing = YES;
}

```


	
	
	
