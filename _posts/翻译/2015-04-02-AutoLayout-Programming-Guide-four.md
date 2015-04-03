---
layout: post
title: AutoLayout Programming Guide 翻译(四)之自动布局的实例
category: 翻译
tags: 翻译 IOS官网文档
keywords: 
description:
---

[原文链接](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/AutoLayoutbyExample/AutoLayoutbyExample.html#//apple_ref/doc/uid/TP40010853-CH5-SW1)。   

## 自动布局的实例 ##
自动布局可以很容易的解决很多复杂的布局问题，不需要手动视图操作。通过创建正确的约束的组合，你可以创建传统上难以管理的代码所创造出来的布局，如方向和大小的变化的同时保持间距不变，滚动视图内的元件影响滚动的内容的大小，或者滚动视图内的元素不随着内容的滚动而滚动。   

### 在滚动视图内应用自动布局技术 ###
当你创建一个应用了自动布局技术的App的时候，滚动视图可能是唯一的一个挑战。滚动的内容的大小必须设置正确以便于用户可以滚动到任何可见的内容，例如如果你需要锁定一个上下文视图保持在一个滚动视图的顶部，如果地图的比例尺和说明。很难保证这个元件不随着其余的内容的滚动而滚动。  

#### 控制滚动视图的内容的大小 ####
滚动视图的内容的大小是由它的子视图的约束所决定的。  
**设置滚动视图的大小**   

1. 创建滚动视图。  
2. 在视图里面放上UI控件。   
3. 创建全面定义滚动视图的内容的宽度和高度的约束。  

你必须确保你给滚动视图里的所有子视图都创建了约束了。例如在给一个没有内在内容的大小的视图定义约束时，你必须不能只添加一个左侧边缘约束你必须也添加右侧边缘，宽度和高度的约束。不能有任何缺失的约束从滚动视图的一个边到其他的边。   

#### 在滚动视图内创建一个锚视图 ####
你可能发现了你想创建的区域不会随着一个用户滑动滚动视图的内容而滚动。你完成这个要用一个额外的容器视图。  

**在滚动视图内锁定一个视图**  

1. 创建一个容器视图用来包含这个滚动视图。  
2. 创建一个滚动视图把它放在这个容器视图里面并对齐所有的边缘。  
3. 在滚动视图内创建并放置一个子视图。  
4. 创建这个字视图到容器视图的约束。  

下面但例子使用上述的步骤来展示如果在滚动视图内放置一个文本视图。在这个例子中，这个文本视图保持在滚动视图的底部，不会在滑动滚动视图内容时随着滑动。    

首先创建用来包含滚动视图的容器视图。设置这个容器视图的大小为期望的滚动视图的大小。   

![sv_containerview](/public/img/sv_containerview.png)    

在容器视图创建后，创建一个滚动视图并把它放在这个容器视图内。重新设置滚动视图的大小来保证所有的边都是紧紧的贴着容器视图的边，通过设置距离为0来达到效果。   

![sv_scrollview](/public/img/sv_scrollview.png)    

创建另外一个视图并把它放在滚动视图内。在这个例子中将会把一个文本视图放在滚动视图中。   

![sv_textview](/public/img/sv_textview.png)   

在放好文本视图之后，创建从文本视图到容器视图的约束（跳过滚动视图）。创建文本视图到容器视图的约束就固定了文本视图到容器视图之间的相对距离，这就保证了滚动视图不会滑动文本视图。  

为了创建跨越多个视图等级的约束，很容易的做法是在界面编辑器的大纲视图中按下`Control`键从这个文本视图拖拽到容器视图.   

![sv_constraints](/public/img/sv_constraints.png)    

当约束面板出现后创建必要的约束。  

![sv_leadconstraint](/public/img/sv_leadconstraint.png)    

在这个例子中，创建了从文本视图到容器视图的左侧右侧和底部的约束。文本视图的底部也是被约束的。   

![sv_textviewconstraints](/public/img/sv_textviewconstraints.png)    

接下来的两个图展示了app在IOS模拟器的正常的和横屏的状态的情况。文本视图被约束在滚动视图的底部不会随着滚动视图的移动而移动。    

![uiscrollview_bottom_sideways](/public/img/uiscrollview_bottom_sideways.png)     
![uiscrollview_bottom_vertical](/public/img/uiscrollview_bottom_vertical.png)     

### 间距和包裹 ###
自动布局提供了几个技术来自动填充视图之间的间距和重新设置每个条目的大小根据他们的内容。接下来的部分描述了怎么创建约束来保持可见视图基于设备的方向按照比例隔开。  

#### 创建等间距的视图 ####

为了布局基于设备方向的按照比例隔开的视图，在可见视图中创建间距视图。正确地设置这些间距视图来保证这些可见视图基于设备的方向都能保持相等的间距。   

保持一定比例的间距的视图   

1.	创建可见视图。  
2.	创建间距视图，个数等于可见视图的个数加一。  
3.	轮流放置这些视图们，从一个间距视图开始。  
	为了放置两个可见视图，放置所有的视图按照下面的模式，从屏幕的左边一直移到右边：  
	`spacer1 | view1 | spacer2 | view2 | spacer3`  
4.	压迫间距视图以便他们的长度都互相相等。  
	> **Note:**间距视图的高度可以是任何值，包括0.然后你必须创建高度的约束不能让高度有任何模棱两可的问题。  
5.	创建一个从第一个间距视图到容器视图的左侧的约束。  
6.	创建一个从最后一个间距视图到容器视图右侧的约束。  
7.	创建间距视图和可见视图之间的约束。  

> **Note：**当垂直的放置视图的时候，先从屏幕的顶部开始放置视图，放置每一个视图在它之前的视图的下面。设置每一个间距视图的高度都相等。  

接下来的例子使用上述步骤来展示如何等间距的放置两个视图。间距视图用来辅助这个例子，但是通常情况下左侧留空没有背景。首先创建两个视图放在故事板中。  

![es_twoviews](/public/img/es_twoviews.png)    

添加三个间距视图，一个放在最左侧的可见视图的左侧，一个放在两个视图中间，一个放在最右侧的可见视图的右侧。这个时候这些间距视图并没有相同的大小，因为他们的大小将通过约束来设置。   

![es_spacersadded](/public/img/es_spacersadded.png)    

给这些间距视图添加如下的约束：  

- 将第二个和第三个间距视图的宽度设置成第一个间距视图的宽度。     
- 设置第一个间距视图的宽度大于等于期望的宽度值的最小值。    
- 创建一个从第一个间距视图到容器视图的左侧的约束。    
- 创建一个水平方向的聪明敏感第一个间距视图到第一个可见视图的约束。设置这个约束的优先级小于等于1000.   
- 创建从第二个间距视图到第一个和第二个可见视图的水平方向的间距。设置这些约束的优先级小于等于999.    
- 创建一个从第三个间距视图到第二个可见视图的水平方向的间距。设置这个约束的优先级的优先级小于等于1000.
- 创建一个从第三个间距视图到容器视图的右侧的约束。   

这些约束创建了两个可见视图和三个间距视图。这些间距视图随着设备方向的改变自动改变大小，保持可见视图等比例的间隔，就像下面两个图展示的一样：  

![es_vertical](/public/img/es_vertical.png)   
![es_horizontal](/public/img/es_horizontal.png)   

### 由自动布局产生的变化的动画 ###
如果你需要完全的控制自动布局产生的变化的动画，你必须以编程的方式来改变你的约束。 在`IOS`和`OSX`的基本概念是一样的。但是也会有一些小的差异。  

在你的IOS app中你的代码可能看起来像下面的：  

	[containerView layoutIfNeeded]; // Ensures that all pending layout operations have been completed
	[UIView animateWithDuration:1.0 animations:^{
     // Make all constraint changes here
     [containerView layoutIfNeeded]; // Forces the layout of the subtree animation block and then captures all of the frame changes
	}];  

在`OSX`中，当使用层支持的动画的时候，用下面的代码：  
	[containterView layoutIfNeeded];
	NSAnimationContext runAnimationGroup:^(NSAnimationContext *context) {
     [context setAllowsImplicitAnimation: YES];
     // Make all constraint changes here
     [containerView layoutIfNeeded];
	}];  

当你不使用层支持动画的时候，你必须使用约束的动画来动态改变：   

	[[constraint animator] setConstant:42];  


















