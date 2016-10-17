---
title: target action
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->
//自定义target:
 action:方法
#warning第一步声明target和action属性@property(nonatomic,assign)idmyTarget;
@property(nonatomic,assign)SELmyAction;



#warning第二部声明方法-(void)addmyTarget:(id)target
 Ation:(SEL)action;

#warning第三步实现方法
-(void)addmyTarget:(id)target
 Ation:(SEL)action{
#warning第四步接收传进来的目标对象和行为方法   
   self.myTarget= target;
   self.myAction= action;
  

}


#warning第五步使用目标对象去执行行为方法
-(void)touchesBegan:(NSSet*)touches withEvent:(UIEvent*)event
{

    [self.myTargetperformSelector:self.myActionwithObject:nil];}