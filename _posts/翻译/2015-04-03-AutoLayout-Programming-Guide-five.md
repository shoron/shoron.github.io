---
layout: post
title: AutoLayout Programming Guide 翻译(五)之在自定义视图中应用自动布局
category: 翻译
tags: 翻译 IOS官网文档
keywords: 
description:
---

[原文链接](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ImplementingView/ImplementingView.html#//apple_ref/doc/uid/TP40010853-CH16-SW1)。   

## 自动布局的实例 ##

自动布局技术通过视图的自我管理降低了控制器的负担。如果你实现了一个自定义的视图，你必须提供足够的信息以便自动布局系统能够做出正确的计算和满足约束条件。特别是，应该确定这个视图有一个内在的大小，如果是这样的话，实现`intrinsicContentSize`来返回一个合适的值。  

### 一个视图指定的内在的内容的大小 ###

叶子层级的视图，例如按钮通常比代码了解更多的关于它们的尺寸的信息。这是通过方法``来计算的，这个方法告诉自动布局系统有在视图中一些它们不清楚的内容，并提供给自动布局系统这些内在的内容的大小。   

一个典型的例子是只有一个行的文本字段视图。布局系统并不了解文本，它只是被告知有一些东西在视图中，而那些东西需要一定的空间来避免被裁减掉一些内容。布局系统会调用`intrinsicContentSize`并且设置约束来指定：  

1. 这些不透明的内容不应该被压缩和裁剪。  
2. 该视图会紧紧拥抱它的内容。  

一个视图可以实现`intrinsicContentSize`来返回它自己的宽度和高度的绝对值。或者来返回`NSViewNoInstrinsicMetric`,因为对于任意一个或者全部，来表明它对于给定的尺寸没有内在的进行测量。  

对于更深的例子，考虑下面的`intrinsicContentSize`的实现。  

* 一个按钮的内在的大小被它的图片和标题决定。一个按钮的`intrinsicContentSize`方法应该返回一个宽度和高度都足够大的大小，以保证标题字段不会被裁减，它全部的图片都是可见的。  
* 一个水平的滑块有一个内在的高度但是没有内在的宽度，因为滑块的艺术的工作方式导致没有最好的宽度。一个水平的`NSSlider`对象返回一个`CGSize`，这被设置为`{NSViewNoInstrinsicMetric, <slider height>}`，任何一个滑块的使用者都要提供滑块宽度的约束。  
* 一个容器，例如一个`NSBox`的实例，没有内在的内容的大小，并且返回`{ NSViewNoInstrinsicMetric, NSViewNoInstrinsicMetric }`.它的子视图可能有内在的内容大小，但是子视图的内容并不是box自己内在的。  

### 当视图的内在的大小变化时，必须通知自动布局系统 ###
如果你的视图的任何属性改变了，并且他们的改变影响了内在的内容的大小，你的视图必须调用`invalidateIntrinsicContentSize`以便布局系统注意到这些变化并且重新计算布局。例如，一个文本字段视图在它的字符串改变时要调用`invalidateIntrinsicContentSize`。    

### 布局操作是对齐矩形不是边框 ###

当考虑自动布局的时候，请记住控制类型的元件的边框不如它可见的内容的重要。结果就是装饰物例如阴影雕刻线都是应该被布局忽略的。界面编辑器中在画布上在定位的时候就会忽略它们.在下面的例子中，蓝色虚线是和按钮的可见的内容对齐的，并不是按钮的边框（蓝色实线）.  

![buttonGuideFrame](/public/img/buttonGuideFrame.png)  

允许布局基于显示的内容而不是边框，视图提供了一个对齐的矩形，布局实际上就是这么做的。为了确定你的覆盖是否在`OSX`上是正确的，你可以设置在绘制对齐举行时`NSViewShowAlignmentRects`为`YES`。   

###  视图会表明基线的偏移 ###

通常情况下你可能要对齐基线控制，虽然你在界面编辑器中能够一直保持基线对齐，但这在以前是不大可能通过变成方式来做到这一点，现在你使用`NSLayoutAttributeBaseline`约束就能做到。  

方法`baselineOffsetFromBottom`返回的是`NSLayoutAttributeBottom`和`NSLayoutAttributeBaseline`之间的距离。`NSView`默认的返回值是0.当它起作用的时候子类可以覆盖这个方法。

## 应用自动布局 ##

那些应用自动布局的和没有应用自动布局的是不能在同一个窗口中共存的。也就是说现有的项目可以逐步采用自动布局，你不必让它一次就完全在你的app中应用起来。相反的，你可以通过使用属性`translatesAutoresizingMaskIntoConstraints`来转换你的app在一段时间内来应用自动布局技术。    

当这个属性值的默认值是`YES`的时候，视图的这个**`autoresizing mask`**属性将会转化成约束。例如如果一个视图像下面的一样来配置，并且`translatesAutoresizingMaskIntoConstraints`的值为`YES`，然后约束`|-20-[button]-20-|`和` V:|-20-[button(20)]`就会被添加到当前视图的父视图中。它的净效应是在10.7版本或者之前这些视图表现出来的和之前一样。  

![springsAndStruts](/public/img/springsAndStruts.png)   

如果你向左移动这个按钮15个点（包括在运行时调用`setFrame:`）,新的约束将会变成`|-5-[button]-35-|`和`V:|-20-[button(20)]`.

对于应用自动布局的视图，在大多数情况下你可能会把`translatesAutoresizingMaskIntoConstraints`设置成`NO`。原因是通过自动调整大小模式所产生的约束已经足够指定它的父视图的给定的视图的边框。例如这种转换阻止一个按钮在标题改变的时候自动采用它的最佳的宽度。  

在以下主要的你不应该调用`setTranslatesAutoresizingMaskIntoConstraints:`的情况是当你不是指定一个视图到它的容器的相对位置的那个人的时候。例如，一个`NSTableRowView`的实例是放在`NSTableView`中的。它可能把自动调整大小模式转化成约束，或者可能不会。这是一个非公开的细节。其它的你不应该调用`setTranslatesAutoresizingMaskIntoConstraints:`这个方法的视图是`NSTableCellView`，`NSSplitView`的子类，`NSTabViewItem`的一个视图，`NSPopover`的内容视图，`NSWindow`,和`NSBox`对象。对于那些熟悉的经典的布局，如果你没有调用`setAutoresizingMask:`,那么在自动布局的时候你也不应该调用`setTranslatesAutoresizingMaskIntoConstraints:`。  

如果你有一个通过调用`setFrame:`来做自己的自定义的布局的视图,那么你现有多代码应该是可以工作的。只是当你手动放置这些视图的时候不要调用`setTranslatesAutoresizingMaskIntoConstraints:`来设置为`NO`.








