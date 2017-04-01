---
title: NSHTTPCookieStorage
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
//保存cookie
+ (void)saveCookies {
    NSData *cookiesData = [NSKeyedArchiver archivedDataWithRootObject: [[NSHTTPCookieStorage sharedHTTPCookieStorage] cookies]];
    NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
    [defaults setObject: cookiesData forKey:kCookie];
    [defaults synchronize];
}

//加载cookie
+ (void)loadCookies {
   
    NSArray *cookies = [NSKeyedUnarchiver unarchiveObjectWithData: [[NSUserDefaults standardUserDefaults] objectForKey: kCookie]];
    NSHTTPCookieStorage *cookieStorage = [NSHTTPCookieStorage sharedHTTPCookieStorage];
   
    for (NSHTTPCookie *cookie in cookies){
        [cookieStorage setCookie: cookie];
    }
   
}

//删除cookie
+ (void)clearCookies {
   
    NSArray *cookies = [[NSHTTPCookieStorage sharedHTTPCookieStorage] cookies];
    for (NSHTTPCookie *cookie in cookies)
    {
        [[NSHTTPCookieStorage sharedHTTPCookieStorage] deleteCookie:cookie];
    }
}
```

