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