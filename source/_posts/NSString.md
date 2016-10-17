---
title: NSString
date: 2016-10-08 11:39:43
categories: objective-c
---
NSString
<!-- more -->

```objc
/***********************NSString***********************/
#pragma mark 获取字符串长度 string.length
    NSString *string = @"12345678";
    NSLog(@"%ld",string.length);

#pragma mark 获取子字符串，调用系统方法 substringFromIndex:
    NSString *string = @"www.lanou.com";
   
    NSString *str1 =  [string substringFromIndex:4];
   
    NSLog(@"%@",str1);

   
#pragma mark 截取指定范围的字符串 substringWithRange:
    //NSRange表示范围的结构体
    NSRange a = {4,5};

    NSString *str3 = [string substringWithRange:a];
   

    NSLog(@"%@",str3);
   
#pragma mark 拼接字符串 stringByAppendingString:
    NSString *string = @"I love";
    NSString *string1 = @" Kitty";
   
    //appand是拼接的意思
    NSString *string2 =  [string stringByAppendingString:string1];
    NSLog(@"%@",string2);
   
   
   
#pragma mark 替换字符串 stringByReplacingOccurrencesOfString:@"I" withString:
    NSString *string = @"I love You";
    NSString *string1 = @" Kitty";
    NSString *string2 =
    [string stringByReplacingOccurrencesOfString:@"I" withString:@"You"];
    NSLog(@"%@",string2);
   
   
#pragma mark 判断字符串是否相等 isEqualToString:
    NSString *string = @"abcd";
    NSString *string1 = @"abc";
    if ([string isEqualToString:string1]) {
        NSLog(@"字符串相等");
    }else
    {
        NSLog(@"字符串不相等");
    }
   
   
   
#pragma mark 字符串是否一一个字符开头 hasPrefix:
    NSString *string = @"abcdefg";
    if ([string hasPrefix:@"a"]) {
        NSLog(@"字符串以a开头");
    }else
    {
        NSLog(@"字符串不以a开头");
    }
   
#pragma mark 字符串是否一一个字符结尾 hasSuffix:  
    if ([string hasSuffix:@"efg"]) {
        NSLog(@"是以efg结尾");
    }else
    {
        NSLog(@"不是以efg结尾");
    }
   
    给定一个图片文件名，判断字符串中是否以“png”结尾，如果是就替换成“jpg”如果不是，就拼接“.jpg”
    NSString *string = @"abc.png";
   
    if ([string hasSuffix:@"png"]) {
        string = [string stringByReplacingOccurrencesOfString:@"png" withString:@"jpg"];

    } else {
         string =  [string stringByAppendingString:@".jpg"];
    }
   
   
    NSLog(@"%@",string);
/***********************NSString************************/




/***********************NSMutableString******************/
#pragma mark - 可变字符串
    //可变字符串本身是可以修改的
    NSMutableString *mutString = [NSMutableString stringWithFormat:@"张三"];


#pragma mark 插入字符串 insertString: atIndex:
    [mutString insertString:@"大" atIndex:1];
    NSLog(@"mutString = %@",mutString);


#pragma mark 删除字符串 deleteCharactersInRange:
    NSRange a = {1,2};
    [mutString deleteCharactersInRange:a];
    NSLog(@"mutString = %@",mutString);
   
#pragma mark 拼接 appendString:
    NSMutableString *mutString = [NSMutableString stringWithFormat:@"张三"];
    [mutString appendString:@"好"];
    NSLog(@"mutString = %@",mutString);   

/***********************NSMutableString******************/

```

