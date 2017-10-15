---
title: RCTActivityIndicatorView
date: {{ date }}
categories: React Native
tags: React Native
comment: true
---
```objective-c
// RCTActivityIndicatorView.h
#import <UIKit/UIKit.h>

@interface RCTActivityIndicatorView: UIActivityIndicatorView
@end
```
```objective-c
// RCTActivityIndicatorView.m
#import "RCTActivityIndicatorView.h"

@implementation RCTActivityIndicatorView {
}

- (void)setHidden:(BOOL)hidden
{
    if ([self hidesWhenStopped] && ![self isAnimating]) {
		[super setHidden: YES];
    } else {
        [super setHidden: hidden];
    }
}

@end
```

```objective-c
// RCTActivityIndicatorViewManager.h
#import <React/RCTViewManager.h>

@interface RCTConvert (UIActivityIndicatorView)
  
+ (UIActivityIndicatorViewStyle)UIActivityIndicatorViewStyle:(id)json;
  
@end
  
@interface RCTActivityIndicatorViewManager: RCTViewManager
  
@end
```

```objective-c
// RCTActivityIndicatorViewManager.m
#import "RCTActivityIndicatorViewManager.h"

#import "RCTActivityIndicatorView.h"
#import "RCTConvert.h"

@implementation RCTConvert (UIActivityIndicatorView)

// 注意：支持 UIActivityIndicatorViewStyleGray 是没有意义的，因为我们可以设置颜色为我们想要的
// 任意值。

RCT_ENUM_CONVERTER(UIActivityIndicatorViewStyle, (@{
  @"large": @(UIActivityIndicatorViewStyleWhiteLarge),
  @"small": @(UIActivityIndicatorViewStyleWhite),
}), UIActivityIndicatorViewStyleWhiteLarge, integerValue)

@end
  
@implementation RCTActivityIndicatorViewManager
  
@end
```

