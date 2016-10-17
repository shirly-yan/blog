---
title: iOS实现一个颜色渐变的弧形进度条
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->
转自：https://www.ganlvji.com/gradient_circle_progress/###1、先来一个结果
80%的状态：
[![80percent](http://img.ganlvji.com/2014/03/80percent.png)](http://img.ganlvji.com/2014/03/80percent.png)
99%的状态:
[![99percent](http://img.ganlvji.com/2014/03/99percent.png)](http://img.ganlvji.com/2014/03/99percent.png)
###2、需要用到的宏：
 
	#define degreesToRadians(x) (M_PI*(x)/180.0) //把角度转换成PI的方式
	#define  PROGREESS_WIDTH 80 //圆直径 
	#define PROGRESS_LINE_WIDTH 4 //弧线的宽度
 
###3、CAShapeLayer
首先，你得要引入Core Animation框架。为了实现环形效果，需要使用到CAShapeLayer，原理是CAShapeLayer可以通过指定Path的方式实现生成一个图形，非常方便。
###4、UIBezierPath
 
由于需要画一个圆形，UIBeziperPath是非常好用的画圆形的工具。实现下面的代码可以画出上面所示的整个轨道。这个圆形是从-210度的角度到30度。
 
	UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:CGPointMake(40, 40) radius:(PROGREESS_WIDTH-PROGRESS_LINE_WIDTH)/2 startAngle:degreesToRadians(-210) endAngle:degreesToRadians(30) clockwise:YES];
 
 
###5、画出一个完成的进度的背景轨道
 
这里原理很简单，就是使用CAShapeLayer和UIBezierPath结合起来就能够达成目标，这一步的结果如下所示：
[![0percent](http://img.ganlvji.com/2014/03/0percent.png)](http://img.ganlvji.com/2014/03/0percent.png)
 
	        _trackLayer = [CAShapeLayer layer];//创建一个track shape layer
	        _trackLayer.frame = self.bounds;
	        [self.layer addSublayer:_trackLayer];
	        _trackLayer.fillColor = [[UIColor clearColor] CGColor];
	         _trackLayer.strokeColor = [_strokeColor CGColor];//指定path的渲染颜色
	        _trackLayer.opacity = 0.25; //背景同学你就甘心做背景吧，不要太明显了，透明度小一点
	        _trackLayer.lineCap = kCALineCapRound;//指定线的边缘是圆的
	        _trackLayer.lineWidth = PROGRESS_LINE_WIDTH;//线的宽度
	        UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:CGPointMake(40, 40) radius:(PROGREESS_WIDTH-PROGRESS_LINE_WIDTH)/2 startAngle:degreesToRadians(-210) endAngle:degreesToRadians(30) clockwise:YES];//上面说明过了用来构建圆形
	        _trackLayer.path =[path CGPath]; //把path传递給layer，然后layer会处理相应的渲染，整个逻辑和CoreGraph是一致的。
###6、渐变进度条：
首先要明确的需求是，我们需要颜色根据百分比从红色渐变到黄色然后再到蓝色。
怎么实现这个颜色的渐变效果。这里我们需要使用到CAGradientLayer，CAGradientLayer是一个用来画颜色渐变的层（如果使用透明的颜色，也就可以做到透明渐变）。我们先用CAGradientLayer做出渐变效果，然后把ShapeLayer作为GradientLayer的Mask来截取出需要的部分，以此达到渐变的进度条效果。
 
首先，需要构建出顺着弧形的颜色渐变。上面的需求我们可以分解成两部分。
①左半部分，颜色从红色渐变到黄色。
②右半部分，颜色从黄色渐变到蓝色。
由此可以了解到是我们需要两个CAShapeLayer。
为什么要这么折腾？CAShapeLayer不能顺着弧线进行渐变只能指定两个点之间进行渐变。所以只能曲线救国了。
先看看这个部分的效果：
[![gradient](http://img.ganlvji.com/2014/03/gradient.png)](http://img.ganlvji.com/2014/03/gradient.png)
然后，创建一个新的CAShapeLayer来截取这个颜色渐变的层。
这部分代码如下所示：
	        _progressLayer = [CAShapeLayer layer];
	        _progressLayer.frame = self.bounds;
	        _progressLayer.fillColor =  [[UIColor clearColor] CGColor];
	        _progressLayer.strokeColor  = [PROCESS_COLOR CGColor];
	        _progressLayer.lineCap = kCALineCapRound;
	        _progressLayer.lineWidth = PROGRESS_LINE_WIDTH;
	        _progressLayer.path = [path CGPath];
	        _progressLayer.strokeEnd = 0;
	
	        CALayer *gradientLayer = [CALayer layer];
	        CAGradientLayer *gradientLayer1 =  [CAGradientLayer layer];
	        gradientLayer1.frame = CGRectMake(0, 0, self.width/2, self.height);
	        [gradientLayer1 setColors:[NSArray arrayWithObjects:(id)[[UIColor redColor] CGColor],(id)[UIColorFromRGB(0xfde802) CGColor], nil]];
	        [gradientLayer1 setLocations:@[@0.5,@0.9,@1 ]];
	        [gradientLayer1 setStartPoint:CGPointMake(0.5, 1)];
	        [gradientLayer1 setEndPoint:CGPointMake(0.5, 0)];
	        [gradientLayer addSublayer:gradientLayer1];
	
	        CAGradientLayer *gradientLayer2 =  [CAGradientLayer layer];
	        [gradientLayer2 setLocations:@[@0.1,@0.5,@1]];
	        gradientLayer2.frame = CGRectMake(self.width/2, 0, self.width/2, self.height);
	        [gradientLayer2 setColors:[NSArray arrayWithObjects:(id)[UIColorFromRGB(0xfde802) CGColor],(id)[MAIN_BLUE CGColor], nil]];
	        [gradientLayer2 setStartPoint:CGPointMake(0.5, 0)];
	        [gradientLayer2 setEndPoint:CGPointMake(0.5, 1)];
	        [gradientLayer addSublayer:gradientLayer2];
	
	        [gradientLayer setMask:_progressLayer]; //用progressLayer来截取渐变层
	        [self.layer addSublayer:gradientLayer];
 
 
###7、进度条效果
走到上面一步我们得到的效果是一个进度为100%的效果，_progressLayer的长度和_trackLayer的长度是一样的。那么怎么解决百分比的问题呢？
CAShapeLayer有一个strokeEnd的属性，这个属性是从0到1的浮点类型，正好可以用表达百分比，而且这个属性是animatable，可以动态的表示进度的变化。
如下代码所示：
	-(void)setPercent:(NSInteger)percent animated:(BOOL)animated
	{
	    [CATransaction begin];
	    [CATransaction setDisableActions:!animated];
	    [CATransaction setAnimationTimingFunction:[CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseIn]];
	    [CATransaction setAnimationDuration:MAIN_SCREEN_ANIMATION_TIME];
	    _progressLayer.strokeEnd = percent/100.0;
	    [CATransaction commit];
	
	    _percent = percent;
	}
