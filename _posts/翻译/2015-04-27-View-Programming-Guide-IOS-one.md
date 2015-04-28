---
layout: post
title: View Programming Guide For IOS 翻译(一)之窗口和视图
category: 翻译
tags: 翻译 IOS官网文档
keywords: 
description:
---

[原文链接。](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewPG_iPhoneOS/Introduction/Introduction.html)  

## 关于窗口和视图 ##

在IOS中，你使用窗口和视图在屏幕上来展示你的应用程序的内容。窗口没有任何可见的内容但是为你的应用程序提供了一个基本的视图的容器。视图定义了你想填充内容的窗口的一部分。例如你可能有一些视图用来显示图片，文本，图形或者这些的内容的一些组合。你也可以用一些视图来组织或者管理其他的视图。  

### 总览 ###

每个应用拥有至少一个窗口和视图来展示它的内容。UIKit和其他的系统框架提供了预定义的视图来展示你的内容。这些视图简单到按钮或者文本标签复杂到列表视图选择器视图以及滚动视图。在没有提供你需要的视图的地方你可以自定义视图管理绘制以及处理事件消息。

#### 视图管理应用程序的虚拟的内容 ####

视图是UIView（或者它的子类）的实例，并且管理应用程序窗口的一个矩形区域。视图负责绘制内容，处理多指触摸事件，和管理子视图的布局。绘制内容涉及到了图形绘制技术例如Core Graphics，OpenGL ES，和在一个视图的矩形区域内绘制图形，图像，文本的UIKit控件。一个视图可以响应它所在的矩形区域的通过手势出发的触摸事件或者直接处理触摸事件。在视图结构中，父视图有责任放置子视图的位置设置子视图的大小，并且可以动态实现。这种动态修改视图，调整视图的约束的能力，例如界面旋转和动画。  

你可以认为视图就是创建用户界面的积木，而不是使用一个视图来显示所有的内容，你会经常使用几个视图来创建一个视图等级结构。视图结构中的每个视图都展示用户界面中的一个特殊的部分，然后根据显示内容来进行特定的优化。例如UIKit有专门为展示图片，文本和其他内容的视图。   

#### 视图展示的窗口坐标 ####

一个窗口是一个UIWindow类的实例并且处理所有展示的用户界面。和视图一起协作来管理交互，变化以及可见的视图等级结构。大多数情况下，你应用程序的窗口不会改变。当创建了窗口以后，它保持不变，窗口变化的时候才展示相关的视图。每个应用程序拥有至少一个窗口，它在用户的主屏幕上展示应用程序的用户界面。如果接入了一个外部的显示设备，应用程序在那个屏幕上创建第二个窗口来展示内容。   

#### 界面编辑器的使用 ####

界面编辑器是一个让你使用图形化的工具来配置应用程序的窗口和视图的应用程序。使用界面编辑器，你会在一个nib文件中配置你的视图，这会作为一个资源文件来存储一个特定版本的视图和其他对象。当你在运行时载入一个nib文件，存储在它里面的文件会被重新计算成你代码中的实际第对象，并以编程对方式操作。  

界面编辑器会大大简化创建应用程序界面的工作。因为支持界面编辑器和nib文件是贯穿整个IOS系统的，一点要求就是把nib文件合并到应用程序的设计中去。

### 看看其他的 ####

由于视图是非常复杂和灵活的对象，所以不太可能在一个文档中来覆盖它所有的行为。然后其他的文档可以帮助你了解其他方面的管理视图的内容以及把视图的相关知识联系起来作为一个整体。   

视图控制器是管理应用程序的视图的一个很重要的部分。一个视图控制器管理一个视图等级结构中的所有视图，并把这些视图展示在屏幕上。关于视图控制器的更多知识，请参考 [IOS视图控制器编程指南](https://developer.apple.com/library/ios/featuredarticles/ViewControllerPGforiPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007457)  

视图是应用程序中的关键的手势和触摸事件的接收者，关于更多的是用手势和处理触摸事件的信息，请参考 [IOS事件处理指南](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009541)   

自定义的视图必须使用现有的绘制技术来展示它的内容。关于更多的在IOS应用程序中使用这些技术来绘制视图，请参考 [IOS应用程序编程指南](https://developer.apple.com/library/ios/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010156)   

当标准视图动画不够使用的时候你可以使用核心动画技术，关于如何使用核心动画技术来实现动画请参考 [核心动画编程指南](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40004514)   
