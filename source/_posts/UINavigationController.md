---
title: UINavigationController
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
属性 ：
viewControllers属性:         存储了栈中的所有被管理的控制器
navigationController属性:  父类中的属性，每个在栈中的控制器，都能    通过此属性，获取自己所在的UINavigationController对象。
     /***********创建导航控制器***********************/
RootViewController *rootVC = [[RootViewControlleralloc]init];

    //创建一个导航控制器
    UINavigationController *naVC = [[UINavigationController alloc]initWithRootViewController:rootVC];
    [rootVC release];
   
    //指定导航控制器为window的根视图
    self.window.rootViewController = naVC;
    [naVC release];
    /*****************************************************/    


    /****************设置导航栏外观***********************/
    //导航栏背景颜色
    naVC.navigationBar.barTintColor = [UIColor purpleColor];
    //设置导航栏不显示半透明效果
    naVC.navigationBar.translucent = NO;
    //将导航栏隐藏
    naVC.navigationBar.hidden = NO;
    //设置背景图片
    UIImage *image = [UIImage imageNamed:@"bk.png"];
    [naVC.navigationBar setBackgroundImage:image forBarMetrics:UIBarMetricsDefault];


     /****************设置导航栏内容***********************/
    //设置导航栏上的内容：navigationItem
    //title
    self.navigationItem.title = @"第二页";
 
    //titleView
    UISegmentedControl *segmentC = [[UISegmentedControl alloc]initWithItems:[NSArray arrayWithObjects:@"消息",@"通话", nil]];
   
    [segmentC addTarget:self action:@selector(segmentAction:) forControlEvents:UIControlEventEditingChanged];
 
    self.navigationItem.titleView = segmentC;
    [segmentC release];
   
   
    //设置自定义内容放在左侧
    UITextField *textf = [[UITextField alloc]initWithFrame:CGRectMake(0, 0, 100, 40)];
    textf.backgroundColor = [UIColor yellowColor];
    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc]initWithCustomView:textf];




     //自定义导航栏左侧按钮
    UIImage *image = [UIImage imageNamed:@"1.png"];
    //取消渲染
    image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc]initWithImage:image style:UIBarButtonItemStylePlain target:self action:@selector(leftBarAction:)]autorelease];

-(void)leftBarAction:(UIBarButtonItem *)barItem
{
    [self.navigationController popViewControllerAnimated:YES];
}

    //自定义导航栏右侧按钮为可以编辑Edit
    self.navigationItem.rightBarButtonItem = self.editButtonItem;

/****************编辑***********************/
//重写系统编辑按钮触发的方法
-(void)setEditing:(BOOL)editing animated:(BOOL)animated
{
    [super setEditing:editing animated:animated];
   
    //通过系统编辑按钮控制tableview的编辑状态
    [self.tableView setEditing:editing animated:animated];
}

    //隐藏导航栏返回按钮
    self.navigationItem.hidesBackButton =YES;

    //自定义导航栏右侧按钮
    self.navigationItem.rightBarButtonItem = [[[UIBarButtonItem alloc]initWithTitle:@"下一页" style:UIBarButtonItemStylePlain target:self action:@selector(rightBarAction:)]autorelease];
   
   
}
-(void)rightBarAction:(UIBarButtonItem *)barItem
{
    SecondViewController *sVC = [[SecondViewController alloc]init];
    [self.navigationController pushViewController:sVC animated:YES];
     [sVC release]; 
}
/*****************************************************/


/****************跳转页面的方法***********************/
pushViewController:animated    //进入下一个视图控制器
popViewControllerAnimated:      //返回上一个视图控制器
popToViewController:animated   //返回到指定的视图控制器
popToRootViewControllerAnimated   //返回到根视图控制器
    //进入下一页
    SecondViewController *secondVC = [[SecondViewController alloc]init];
   
    [self.navigationController pushViewController:secondVC animated:YES];

   //返回上一页
    [self.navigationController popViewControllerAnimated:YES];
   
    //跳到根页面
    [self.navigationController popToRootViewControllerAnimated:YES];
   
   
   //跳到指定页面
    UIViewController *vc =
    [self.navigationController.viewControllers objectAtIndex:0];
 
    [self.navigationController popToViewController:vc animated:YES];
/*****************************************************/

自定义导航栏左侧按钮后恢复右拉返回功能
self.navigationController.interactivePopGestureRecognizer.delegate=(id)self;

```

