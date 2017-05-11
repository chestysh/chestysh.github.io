---
title: CAGradientLayer 的使用
date: 2017-05-11 15:49:15
tags:
---
## CAGradientLayer 一些属性设置

> 根据两点的坐标，颜色可以渐变显示

```
CAGradientLayer *colorLayer = [CAGradientLayer layer];
 colorLayer.frame    = (CGRect){CGPointZero, CGSizeMake(200, 200)};
 colorLayer.position = self.view.center;
 [self.view.layer addSublayer:colorLayer];

// 颜色分配
colorLayer.colors = @[(__bridge id)[UIColor redColor].CGColor,
                      (__bridge id)[UIColor greenColor].CGColor,
                      (__bridge id)[UIColor blueColor].CGColor,
                      (__bridge id)[UIColor blackColor].CGColor];
 // 颜色分割线位置(如果不设置分割线，颜色会渐变显示)
 colorLayer.locations  = @[@(0.25), @(0.5), @(0.65),@(0.10)];

 // 起始点
 colorLayer.startPoint = CGPointMake(0, 0);

 // 结束点
 colorLayer.endPoint   = CGPointMake(1, 0);
```
