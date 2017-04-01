---
title: iOS 使用纯代码或xib创建圆角视图
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->
####尊重原创 转自：http://www.jianshu.com/p/80f1fd3f63a0
####

####引言:
> 在我们日常开发中, 很多中情况下我们需要设置UIView或者UIImageView的圆角以及边框等,例如个人中心的用户头像等等。
> 
> **例如 简书:**
![](http://upload-images.jianshu.io/upload_images/1975627-4cd88898bae5d219.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
简书-我的界面#####我们通常的做法是:
1. 使用纯代码。
2. 在xib下设置属性。

***
####纯代码创建圆角视图:
代码如下:
	    UIView *view = [[UIView alloc] initWithFrame:(CGRectMake(50, 100, 200, 200))];
	    view.backgroundColor = [UIColor brownColor];
	
	    // 设置圆角的角度(当view的宽和高相等, 且 圆角角度为宽的一半时 为圆形)
	    view.layer.cornerRadius = 20;
	
	    // 设置允许裁剪
	    view.layer.masksToBounds = YES;
	
	    // 设置边框
	    view.layer.borderWidth = 5;
	
	    // 设置边框颜色
	    view.layer.borderColor = [[UIColor redColor] CGColor];
	
	    [self.view addSubview:view];
创建imageView 和 view 的方式是一样的, 在这里只演示view的创建。
***
####xib下创建圆角视图:
正常情况下, 我们在xib中使用 keyPath 来设置圆角。如图:
![](http://upload-images.jianshu.io/upload_images/1975627-5e1fb160b9f1c014.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
xib.png
这里虽然设置了, 但是我们并不能立即在xib中看到设置后圆角效果的, 只有在程序运行后才会看到效果。 所以我们要使用`IB_DESIGNABLE`和`IBInspectable`来实现实时查看圆角效果。
######IB_DESIGNABLE:
这是一个宏定义, 功能就是让XCode动态渲染出该类图形化界面。
使用方法: 在自定义的view的.h文件中 添加该 宏定义。
######IBInspectable:
IBInspectable 宏定义的功能是可以可视化显示自定义view中相关的属性。
***
####如何实现实时查看圆角效果:
1.自定义view , CornerView.h:
	#import <UIKit/UIKit.h>
	IB_DESIGNABLE  // 动态刷新
	@interfaceCornerView : UIView
	// 注意: 加上IBInspectable就可以可视化显示相关的属性
	/** 可视化设置边框宽度 */
	@property (nonatomic, assign)IBInspectable CGFloat borderWidth;
	/** 可视化设置边框颜色 */
	@property (nonatomic, strong)IBInspectable UIColor *borderColor;
	/** 可视化设置圆角 */
	@property (nonatomic, assign)IBInspectable CGFloat cornerRadius;
	@end
CornerView.m:
	@implementationCornerView.m
	
	#pragma mark - 设置边框宽度
	- (void)setBorderWidth:(CGFloat)borderWidth {
	
	    if (borderWidth < 0) return;
	
	    self.layer.borderWidth = borderWidth;
	}
	
	#pragma mark - 设置边框颜色
	- (void)setBorderColor:(UIColor *)borderColor {
	
	    self.layer.borderColor = borderColor.CGColor;
	}
	
	#pragma mark - 设置圆角
	- (void)setCornerRadius:(CGFloat)cornerRadius {
	
	    self.layer.cornerRadius = cornerRadius;
	    self.layer.masksToBounds = cornerRadius > 0;
	}


2.在xib中拖一个view, 将view设定为 自定义view: CornerView 如图:
![](http://upload-images.jianshu.io/upload_images/1975627-4db67f19b49fe871.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
自定义view
注意:`Designable`如果没有这行字，说明没有正确添加添加IB_DESIGNABLE，如果它的状态显示up to date,说明设置成功, 如果是updating，说明视图在更新，案如果是build failed的话，请检查布局代码，可能有哪里出错了。
3.如图, 我们就可以看到自定义view中设置的属性了:
![](http://upload-images.jianshu.io/upload_images/1975627-07722f927de30e71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
自定义属性4.设置好 属性后 , 右边的view就会立即 更新了!
![](http://upload-images.jianshu.io/upload_images/1975627-e8d8c896c0de4983.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
效果图.jpg该方法同样适用于UIIamgeView ! ! ! !
***
####效果图:
![](http://upload-images.jianshu.io/upload_images/1975627-ebdb869d89da681f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
效果图


本篇文章对应的源代码下载地址:[CornerViewDemo](https://github.com/LiCheng244/CornerViewDemo)
**该 demo 中封装了 自定义view 和 自定义imageView , 可以将CornerTool文件夹拖入到项目中直接使用, `
xib下 view` 继承 `CornerView`类, `imageView` 继承 `CornerImageView`即可设置属性。**


文／Li_Cheng（简书作者）
原文链接：http://www.jianshu.com/p/80f1fd3f63a0
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。