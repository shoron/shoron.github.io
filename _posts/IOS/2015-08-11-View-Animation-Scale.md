---
layout: post
title: iOS 视图缩放动画的两种方式
category: iOS
tags: iOS
keywords:
description:
---

### iOS 视图缩放动画的两种方式 ###
    
#### 单一缩放方式（由小到大或者由大到小） ####
	

		CABasicAnimation *theAnimation;
		theAnimation=[CABasicAnimation animationWithKeyPath:@"transform"];
		theAnimation.duration=8; // animation time
		theAnimation.fromValue = [NSNumber numberWithFloat:1];  // 开始时的缩放比例
		theAnimation.toValue = [NSNumber numberWithFloat:0.5];  // 结束时的缩放比例
 		[view.layer addAnimation:theAnimation forKey:@"animateTransform"];


#### 任意缩放 （可以由小到大再接着由大到小，可以循环变化）
	

	CAKeyframeAnimation *animation = [CAKeyframeAnimation animationWithKeyPath:@"transform"];
    NSMutableArray *values = [NSMutableArray array];
    [values addObject:[NSValue valueWithCATransform3D:CATransform3DMakeScale(0.2, 0.2, 1.0)]];
    [values addObject:[NSValue valueWithCATransform3D:CATransform3DMakeScale(1.3, 1.3, 1.0)]];
    [values addObject:[NSValue valueWithCATransform3D:CATransform3DMakeScale(1.0, 1.0, 1.0)]];
    animation.values = values;
    [view.layer addAnimation:animation forKey:@"scale-layer"];  


#### 备注：####

animation 的keyPath不要用 `@“transform.scale”` ,因为会 crash。
