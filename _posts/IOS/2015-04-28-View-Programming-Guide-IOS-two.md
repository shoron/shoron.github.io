---
layout: post
title: View Programming Guide For iOS 翻译(二)之窗口和视图结构
category: iOS
tags: iOS iOS官网文档
keywords: 
description:
---

[原文链接。](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/WindowsandViews/WindowsandViews.html#//apple_ref/doc/uid/TP40009503-CH2-SW1)  

## 窗口和视图结构 ##

视图和窗口展示你的应用程序的用户界面并且处理界面之间的交互。UIKit和其他系统框架提供了很多你可以直接使用或者修改一点就能使用的视图。你也可以定义自己的视图当你需要展示标准视图不支持的内容的时候。  

不管你使用系统的视图还是创建自定义的视图你都要明白UIView和UIWindow所提供的一些基础。这些类提供了先进的技术来管理布局和展示视图。了解这些技术是如何工作的对于你确保你的视图正确响应应用程序的变化是很重要的。  

### 视图结构基础 ###  


