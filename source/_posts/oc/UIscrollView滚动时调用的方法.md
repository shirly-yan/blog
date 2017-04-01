---
title: UIscrollView滚动时调用的方法
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
//UIscrollView开始拖拽的时候调用
- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView
{
    NSLog(@"开始拖拽");
}


//UIscrollView拖拽结束的时候调用
-(void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    NSLog(@"拖拽结束");
}


//UIscrollView拖拽结束的时候调用
-(void)scrollViewDidScroll:(UIScrollView *)scrollView
{
    //在滚动的时候调用
}
 
//UIscrollView开始减速的时候调用
-(void)scrollViewWillBeginDecelerating:(UIScrollView *)scrollView
{
    NSLog(@"开始减速");
   
}

//UIscrollView减速停止的时候调用
-(void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
{
    NSLog(@"减速停止");
}

-(UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView
{
    return [scrollView viewWithTag:9999];
    //只有实现这个方法 才能执行缩放 指定对象;
}
-(void)scrollViewDidZoom:(UIScrollView *)scrollView
{
    NSLog(@"缩放时一直触发");
   
}


- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView
{
    return _imageView;
}

#pragma mark 当缩放完毕的时候调用
- (void)scrollViewDidEndZooming:(UIScrollView *)scrollView withView:(UIView *)view atScale:(float)scale
{
//    NSLog(@"结束缩放 - %f", scale);
}

#pragma mark 当正在缩放的时候调用
- (void)scrollViewDidZoom:(UIScrollView *)scrollView
{
//    NSLog(@"-----");
}
```

