---
layout: post
title: AutoLayout Programming Guide 翻译(三)之解决自动布局的问题
category: iOS
tags: iOS iOS官网文档
keywords: 
description:
---

[原文链接](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/ResolvingIssues/ResolvingIssues.html#//apple_ref/doc/uid/TP40010853-CH17-SW1)。   

## 解决自动布局的问题 ##
当你创建有冲突的约束，当你没有提供足够的约束，或者当你最终使用一组有歧义的约束的时候，自动布局会出问题。当自动布局出问题时，自动布局系统会尝试降低自己来尽可能优雅的让你的app保持可用的状态。但是在开发环境中捕捉自动布局的问题十分重要。自动布局提供几个特色来帮助你找到并解决问题的源头，包括界面编辑器的可见的提示，这会确定哪些问题已经发生。这样就可以用适当的解决方案解决它。  

### 确定问题 ###

界面编辑器会在几个不同的地方来展示自动布局中的问题。  

**问题导航条中.**问题导航条按照错误类型来把错误分类。  

![debug_navigator](/public/img/debug_navigator.png)  

**界面编辑器的大纲视图.**如果在你正在编辑的画布的顶层的视图中有任何问题，界面编辑器的大纲视图会显示一个警觉的箭头。如果有冲突或者歧义，那这个箭头是红色的。如果约束在视图中错位了，那这个箭头是黄色的。  

![red_arrow_in_outline_view_2x](/public/img/red_arrow_in_outline_view_2x.png)  

点击这个警觉的箭头你会发现按照类型分成的一组一组的带有相关的控件和约束的信息的错误信息。点击错误旁边的错误或者警告的符号你会发现发生了什么和改进的一些建议。  

![debug_info_view](/public/img/debug_info_view.png)  

**在画布上。**错位的和有歧义的约束是黄色的，冲突的约束是红色的。红色的点状的边框会预测错位的或者有歧义的视图运行时的位置。这些问题会在下面的部分中进行描述。  

### 解决视图错误问题 ##

在界面编辑器中约束和边框是分离的。如果有一个视图，你在画布上看到的边框和基于当前设置的约束在运行时的位置不一致，那么你的视图错位了。  

界面编辑器通过以黄色画出不匹配的约束来展示错位的视图。它也会你的视图在运行时的位置展示一个点状的红色的边框。

![misplacement](/public/img/misplacement.png)  

**解决错位问题**  
做下面的其中之一的操作：  

- Choose Issues > Update Frames.这个操作会导致元件会移动到它错位的位置。元件的边框会适应当前存在的约束。  
- Choose Issues > Update Constraints.这个操作会导致约束更新到元件的新的位置。这些约束会改变去匹配当前的边框的位置。  

## 解决约束之间的冲突 ##

当你有一组布局系统不能满足的约束的时候会发生冲突。例如对同一个元件设置两个不同的宽度。冲突的约束会在画布上显示为红色以实际对尺寸绘制出来。  

**解决约束冲突：** 删除有冲突的约束中的一个。  
**忠告：**如果你想删除所有的约束重新开始，choose Issues > Clear Constraints.  

### 解决歧义 ###

当你定义的约束对于视图有多种大小或者位置的解决方案的时候会产生歧义。这个可能会是一个原因，例如没有足够的约束或者因为视图的内容大小没有确定。   

界面编辑器会通过画出黄色的边框来显示歧义。想了解更多关于歧义的信息请使用问题导航栏。你解决歧义的方法取决于产生歧义的原因。  

![ambiguity_no_vertical](/public/img/ambiguity_no_vertical.png)   

#### 缺少一个或多个约束 ####

最简单的原因是没有足够的约束。例如你约束了一个元件的水平方向的位置但是没有约束垂直方向上的位置。解决这种歧义你需要添加缺失的约束。  

**添加缺失的约束:**  

- Choose Issues > Add Missing Constraints.  

#### 内容大小不确定 ####

一些容器类的视图例如堆叠视图它的大小取决于它的内容的大小以确定在运行时它们自己的大小，这就意味着在设计的时候它们的大小是不知道的。解决这种问题你需要设置约束的占位符，例如占位符可以作为一个视图的最小的宽度。（当视图确定了自己的大小时，占位符会在运行时移除掉）。  

**添加占位符约束**    

1. 像正常一样添加需要的约束。  
2. 选择这个约束，打开属性检查器。   
3. 选中占位符选项旁边的复选框   

![placeholder](/public/img/placeholder.png)    

#### 自定义视图的大小不确定 ####

不像标准的视图，自定义视图没有定义内在的内容的大小。在设计时，界面编辑器不会知道一个自定义的视图期望的大小。为了解决这个问题，你应该设置一个占位符内在的内容的大小来表明自定义视图的内容大小。  

**设置内在的内容的大小**   

1. 在当前视图选中的情况下，打开大小检查器    
2. 在内在的大小选项中选择占位符。   
3. 设置近似的宽度和高度。   

![size_inspector](/public/img/size_inspector.png)    
![intrinsic_content_size](/public/img/intrinsic_content_size.png)  

### 用代码调试 ###
笼统地说，在调试一个布局问题时有两个阶段：  

1. 从“这个视图的位置是错的”到“设置的约束是错的”  
2. 从“这个约束是错的”到“代码是错的”   

**调试自动布局的问题**  

1. 确定视图的边框大小是否正确。  
很明显的可以知道哪个视图有问题。如果不知道，你可以用`NSView`的`_subtreeDescription`方法来打印一个文本一样的关于视图的等级的描述。  
**Important:** `_subtreeDescription`不是公开的API,然而它可以被用来调试。  
2. 如果可能的话，在运行时重现这个问题。   
3. 找到有问题的约束。  
为了找到影响特定视图的约束可以在`OSX`中使用`constraintsAffectingLayoutForOrientation:`,在`iOS`中使用`constraintsAffectingLayoutForAxis:`.  
然后你可以在调试器中检查这些约束，调试器会用视觉形式语言的形式来打印约束的信息。如果你的视图有标识符，那么它打印出来的信息就像是这样的:  
  	`<NSLayoutConstraint: 0x400bef8e0 H:[refreshSegmentedControl]-(8)-[selectViewSegmentedControl] (Names: refreshSegmentedControl:0x40048a600, selectViewSegmentedControl:0x400487cc0 ) >`    
或者像这样的：   
	`<NSLayoutConstraint: 0x400cbf1c0 H:[NSSegmentedControl:0x40048a600]-(8)-[NSSegmentedControl:0x400487cc0]>`    
4. 如果这个时候还不明显，可以把约束传递给窗口显现在屏幕上通过`visualizeConstraints:`.  
当你点击一个约束的时候，它会在控制台打印出来。用这种方法你可以确定哪个是不正确的。  
这时候你可能就会知道布局是有歧义的。  
5. 找到有问题的代码。  
有时候一旦你确定了有问题的约束，你也就知道了怎么去修复它。如果情况不是这样的，用模拟器来查找这时候的约束（或者关于它的一些描述）。这会把约束生命周期中的有意思的事展示给你，包括它的创建，修改，在窗口使用等等。对于上述的每个情况你都能看到它发生的过程。找到这个阶段，哪些东西缺失了，看看它发生的过程，你就能找到有问题的代码了。  

### 不满足约束时自动布局优雅的降级 ###
配置了不能满足的约束是一种编程的错误，然而，面对不满足的约束自动布局系统尝试优雅的降级。  

1. `addConstraint:`方法（或者`NSLayoutConstraint`的`setConstraint:`方法）打印出不满足条件的约束（因为这也是一个编程的错误），并且抛出一个异常。  
2. 系统立即捕获异常。  
尽管添加了一个不能满足的约束是一个编程的错误，它是一个很容易想象发生在计算机上的错误，并且系统更容易比程序崩溃容易恢复。抛出异常以便你注意到这个问题，捕获这个问题，因为当系统还在工作的时候它很容易调试约束系统。  
3. 为了使布局系统继续工作，系统从互相不满足条件的约束中选择一个把它的优先级从“required”降低到一个非必需的可选的值，即便如此它的优先级也要比其他的你额外指定的优先级要高。其效果是随着系统的运行，这个不正确的约束系统会首先尝试满足它。（注意一点，每个约束都是一开始被设置为"required"的，因为如果不是这样就不会产生问题。）   

		2010-08-30 03:48:18.589 ak_runner[22206:110b] Unable to simultaneously satisfy constraints:
		(
    	"<NSLayoutConstraint: 0x40082d820 H:[NSButton:0x4004ea720'OK']-(20)-|   (Names: '|':NSView:0x4004ea9a0 ) >",
    	"<NSLayoutConstraint: 0x400821b40 H:[NSButton:0x4004ea720'OK']-(29)-|   (Names: '|':NSView:0x4004ea9a0 ) >"
		)
		Will attempt to recover by breaking constraint
		<NSLayoutConstraint: 0x40082d820 H:[NSButton:0x4004ea720'OK']-(20)-|   (Names: '|':NSView:0x4004ea9a0 ) >
