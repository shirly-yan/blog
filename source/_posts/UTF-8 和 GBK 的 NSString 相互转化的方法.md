---
title: UTF-8 和 GBK 的 NSString 相互转化的方法
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

应用都要遇到一个很头疼的问题：文字编码，汉字的 GBK 和 国际通用的 UTF-8 的互相转化稍一不慎，
就会满屏乱码。下面介绍 UTF-8 和 GBK 的 NSString 相互转化的方法
 
 NSStringEncoding enc = CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000);
    char* c_test = "北京";
    int nLen = strlen(c_test);
    NSString* str = [[NSString alloc]initWithBytes:c_test length:nLen encoding:enc ];
    NSLog(@"str = %@",str);
 
**从 GBK 转到 UTF-8
**用 NSStringEncoding enc =CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000) ，
然后就可以用initWithData：encoding来实现。
 
**从 UTF-8 转到 GBK
**CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000)，
得到的enc却是kCFStringEncodingInvalidId。
没关系，试试 NSData *data=[nsstring dataUsingEncoding:-2147482063];
 
转换字符编码主要用到CFStringConvertEncodingToNSStringEncoding函数,具体的大家可以看看这个函数的用法
NSStringEncoding enc = CFStringConvertEncodingToNSStringEncoding (kCFStringEncodingGB_18030_2000);
 
 
完整代码如下:
**NSURL *url = [NSURL URLWithString:urlStr];
NSData *data = [NSData dataWithContentsOfURL:url]; 
NSStringEncoding enc = CFStringConvertEncodingToNSStringEncoding(kCFStringEncodingGB_18030_2000);
NSString *retStr = [[NSString alloc] initWithData:data encoding:enc];
** 
一个比较方便的转换NSString为UTF8编码的函数,大家可以试试
 
**头文件：
**@interface NSString (OAURLEncodingAdditions) 
- (NSString *)URLEncodedString; 
- (NSString *)URLDecodedString; 
@end
**m文件:
** 
@implementation 
NSString (OAURLEncodingAdditions) 
 - (NSString *)URLEncodedString
{ 
 NSString *result = (NSString *)CFURLCreateStringByAddingPercentEscapes(kCFAllocatorDefault,(CFStringRef)self,NULL,CFSTR("!*'();:@&=+$,/?%#[]"),kCFStringEncodingUTF8);
 [result autorelease];
 return result; 
}
 
- (NSString*)URLDecodedString
{
   NSString *result = (NSString *)CFURLCreateStringByReplacingPercentEscapesUsingEncoding(kCFAllocatorDefault,(CFStringRef)self, CFSTR(""),kCFStringEncodingUTF8);CFSTR(""),kCFStringEncodingUTF8); 
   [result autorelease];    
   return result; 
} 
@end
如果需要转换一个NSString, 只需要
 
NSString *temp = [@"测试utf8" URLEncodedString];  
NSString *decoded = [temp URLDecodedString];
