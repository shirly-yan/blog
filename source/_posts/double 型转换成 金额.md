---
title: double 型转换成 金额
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
- (NSString *)priceString:(double)input {
DDLogDebug(@”input %f”,input);
NSNumber *price = [NSNumber numberWithDouble:input];
DDLogDebug(@”price %@”,price);
NSNumberFormatter *nf = [NSNumberFormatter new];
[nf setNumberStyle:NSNumberFormatterCurrencyStyle];
nf.locale = [NSLocale localeWithLocaleIdentifier:@”zh_CN”];
DDLogDebug(@”nf %@”,[nf stringFromNumber:price]);
return [nf stringFromNumber:price];
}


![这里写图片描述](http://img.blog.csdn.net/20151218115131757 "")
