---
title: model归档
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
#import <Foundation/Foundation.h>

#warning 归档第一步：让model签订NSCoding协议
@interface Student : NSObject<NSCoding>

@property(nonatomic,copy)NSString *name;
@property(nonatomic,assign)NSInteger age;
@property(nonatomic,copy)NSString *gender;
@end


#import "Student.h"

@implementation Student

#warning 归档第二步：对model属性进行编码
-(void)encodeWithCoder:(NSCoder *)aCoder
{
    [aCoder encodeObject:self.name forKey:@"name"];
    [aCoder encodeObject:self.gender forKey:@"gender"];
    [aCoder encodeInteger:self.age forKey:@"age"];
}
#warning 归档第三步：对model进行解码
-(id)initWithCoder:(NSCoder *)aDecoder
{
    self = [super init];
    if (self) {
        self.name =
        [aDecoder decodeObjectForKey:@"name"];
        self.gender =
        [aDecoder decodeObjectForKey:@"gender"];
        self.age =
        [aDecoder decodeIntegerForKey:@"age"];
    }
    return self;
}


@end


 /***************写入model*************************/
    //缓存自定义model模型数据
    //需要使用iOS中归档/反归档，专门来存储model数据到本地
   
#warning 归档第四步：对model进行归档操作
    Student *stu = [[Student alloc]init];
    stu.name = @"大水杯";
    stu.gender = @"男";
    stu.age = 18;
    NSString *archiverPath = [documentPath stringByAppendingPathComponent:@"student"];
   
    //归档类
    BOOL result3 =
    [NSKeyedArchiver archiveRootObject:stu toFile:archiverPath];
   
    if (result3) {
        NSLog(@"归档成功");
    }else
    {
        NSLog(@"归档失败");
    }
   
    //反归档取出model数据
    Student *stu1 = [NSKeyedUnarchiver unarchiveObjectWithFile:archiverPath];
    NSLog(@"name = %@ %@ %ld",stu1.name,stu1.gender,stu1.age);
    /***************写入model*************************/
   
```

