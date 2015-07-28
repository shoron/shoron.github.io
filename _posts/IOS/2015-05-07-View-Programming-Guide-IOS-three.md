---
layout: post
title: View Programming Guide For iOS（三）视图
category: iOS
tags: iOS iOS官网文档
keywords: 
description:
---

以下是从原文档中提炼的一些知识点  

1. 视图的一些常用属性及解释。
    * alpha. 改变视图的透明度。
    * hidden. 设置视图是否是隐藏的。
    * opaque. 设置当前视图和其他视图是否需要复合。
    * bounds. 当前视图的大小（位置是相对于自己的坐标）
    * frame. 相对于父视图的位置，以及视图的大小。
    * center. 视图的中心相对于父视图的位置坐标。
    * transform。视图的一些缩放旋转之类的动画属性。
    * autoresizingMark. 是否自动计算自己在父视图中的大小。
    * autoresizesSubviews. 当前视图中的子视图是否需要自动重新计算大小。
    * contentMode. 当视图的大小改变时，应该怎样显示视图的内容。
    * contentStretch. Deprecated in iOS 6.0
    * contentScaleFactor. 当你需要在高分辨的屏幕上定制绘制的行为的时候会用到。
    * gestureRecognizers. 该属性包含了该视图中的所有手势识别。
    * userInteractionEnabled. 决定是否忽略用户的手势事件，并从事件队列中移除该事件。
    * multipleTouchEnabled. 是否支持多指触摸事件。
    * exclusiveTouch. 接收者是否只处理触摸事件。
    * backgroundColor. 背景色。
    * subviews. 返回所有的子视图的数组。
    * drawRect:和layer层的一些相关属性。  
    更多属性信息请参考 [UIView类参考](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/doc/uid/TP40006816)

