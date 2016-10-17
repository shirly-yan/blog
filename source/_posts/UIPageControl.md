---
title: UIPageControl
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
  /*****************创建UIPageControl*********************/
    //创建UIPageControl
   self.pageC= [[UIPageControlalloc]initWithFrame:CGRectMake((WIDTH-300)/2,HEIGHT-60,300,50)];
    [self.viewaddSubview:self.pageC];
    [self.pageCrelease];
   
    //设置页数
   self.pageC.numberOfPages =10;

   //设置当前页码的颜色
   self.pageC.currentPageIndicatorTintColor = [UIColoryellowColor];

   //设置所有页的颜色
    self.pageC.pageIndicatorTintColor= [UIColorredColor];
   
    //核心方法
    [self.pageCaddTarget:selfaction:@selector(pageAction:)forControlEvents:UIControlEventValueChanged];
   
   
    self.scroll.delegate= self;
   
}
-(void)pageAction:(UIPageControl*)pageC
{
    //打印当前页码

    NSLog(@"切换页码= %ld ",(long)pageC.currentPage);
   
  //页面跟着page的点动
    [self.scrollsetContentOffset:CGPointMake(375*self.pageC.currentPage,0)animated:YES];
   
   
}
   /*****************************************************/ 
```


