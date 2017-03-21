---
title: iOS 打电话
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
- (void)telePhoneAction:(NSString *)teleNum {

    NSString *number = teleNum;// 此处读入电话号码
   
    //NSString *num = [[NSString alloc] initWithFormat:@"tel://%@",number]; //number为号码字符串 如果使用这个方法 结束电话之后会进入联系人列表
   
    NSString *num = [[NSString alloc] initWithFormat:@"telprompt://%@",number]; //而这个方法则打电话前先弹框  是否打电话 然后打完电话之后回到程序中 网上说这个方法可能不合法 无法通过审核
   
    [[UIApplication sharedApplication] openURL:[NSURL URLWithString:num]]; //拨号
}
```


