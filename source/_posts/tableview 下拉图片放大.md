---
title: tableview 下拉图片放大
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc

#import "RootViewController.h"

@interface RootViewController ()

@end

@implementation RootViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.

    self.view.backgroundColor = [UIColor lightGrayColor];

    [self setAutomaticallyAdjustsScrollViewInsets:NO];

    self.myTableView = [[UITableView alloc] initWithFrame:[[UIScreen mainScreen] bounds] style:UITableViewStylePlain];
    [self.view addSubview:self.myTableView];
    [_myTableView release];

    //设置tableview的contentView距离上边界200
    //相对于0点,已经向下偏移了-200
    self.myTableView.contentInset = UIEdgeInsetsMake(200.0+64, 0, 0, 0);
    self.myTableView.delegate = self;
    self.myTableView.dataSource = self;


    //相对于0点,图片坐标应该是(0,-200)
    self.imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, -200.0, self.view.frame.size.width, 200)];
    self.imageView.image = [UIImage imageNamed:@"hua.jpg"];
    //设置imageView高度改变时宽度也跟着改变
    self.imageView.contentMode = UIViewContentModeScaleAspectFill;
    [self.myTableView addSubview:self.imageView];
    [_imageView release];

}


//指定多少行
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return 10;
}

//创建一个cell
-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    NSString *cellIdentifier = @"cell";
  UITableViewCell *cell =  [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if (!cell) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];

    }

    cell.textLabel.text = [NSString stringWithFormat:@"%ld",indexPath.row];
    cell.selectionStyle =  UITableViewCellSelectionStyleNone;

    return cell;

}
//设置行高
-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 80;
}
-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    //点击后自动取消选中置灰效果
    [tableView deselectRowAtIndexPath:indexPath animated:YES];

}
-(void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    //刚开始y的偏移量初始值就是-264
    NSLog(@"y1 === %f",scrollView.contentOffset.y);
    CGFloat y = scrollView.contentOffset.y + 64;//加上导航栏高度,第一次是-200
    NSLog(@"y2 === %f",y);

    if (y < -200) {
        CGRect frame = self.imageView.frame;
        frame.origin.y = y;//imageView的frame是不断往上偏移
        frame.size.height =  -y;//tablview向下偏移了多少,高度就增加多少
        self.imageView.frame = frame;
    }

}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}


@end
```

