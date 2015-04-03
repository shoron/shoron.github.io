---
layout: post
title: AutoLayout Programming Guide 翻译(二)之使用自动布局技术
category: 翻译
tags: 翻译 IOS官网文档
keywords: 
description:
---

[原文链接](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/WorkingwithConstraints/WorkingwithConstraints.html#//apple_ref/doc/uid/TP40010853-CH8-SW1)。   

## 在界面编辑器中使用约束 ##

添加，编辑或者移除约束的最容易的方式是用界面编辑器的可视化布局工具。创建一个约束就像在两个视图之间拖拽一样简单，一次添加多个约束的时候你可以简单的使用各种弹出窗口

### 添加约束 ###

当你从对象库里拖动一个元素并且把他放在界面编辑器上时，它开始不受约束，更容易被拖拽来展示它的界面的样子。当你没有对一个元素添加任何约束就创建并且运行，你会发现界面编辑器会固定这个元素的宽度和高度并且把他放在父视图的左上角的位置。这意味着改变窗口的大小不会移动和改变这个元素的大小。为了让你的界面正确的应对大小和方向上的变化，你需要开始添加约束。  
> Important: 尽管当你创建一个用户界面的时候没有适当的约束，xcode也没有生成一个警告或者一个错误，但是你不应该在这种状态下提交你的应用程序。  
这有几种添加约束的方法。用什么方法取决于你想一次添加几个约束。

#### 按住Control键拖拽添加约束 ####

添加一个约束最快的方式就是按下Control键从画布上的视图上拖动就像你创建outlets活着actions一样。这个Control-drag方法是一个快速精确的方法，对于你明确知道你要创建什么类型，在哪创建的一个单独的约束。  

你可以拖拽一个元素到它自己，包含它的容器，或者到另外一个元素。这取决于你拖向哪个元素和你从哪拖过来的。Auto Layout会适当的限制一下约束。例如你从一个元素水平向右拖动到它的容器，你只有这个这个元素的右侧或者水平中心到它的容器的选项。  

![trailing_2x](/public/img/trailing_2x.png)  

> Tip: 从拖拽菜单中一次选择多个约束需要按下Command或者Shift键。  

#### 从Align和Pin菜单中添加约束 ####
你也可以用界面编辑器画布上的Auto Layout Menu来添加约束

![al_menu_2x](/public/img/al_menu_2x.png)  

除了添加对齐或者约束间距，你还可以用这个菜单来解决布局上的问题，并确定调整行为的约束。

![al_menu_callouts](/public/img/al_menu_callouts.png)  

- Align. 创建对齐的约束，如向父容器的中心对齐，两个视图的左侧对齐。
- Pin. 创建间距约束例如定义一个视图的高度，或者指定它距离另一个视图的水平方向上的距离。
- Issues. 根据建议，通过添加或者重新设置约束来解决布局问题。
- Resizing. 指定如何调整有影响的约束。  

![align_and_pin_menus_2x](/public/img/align_and_pin_menus_2x.png)  

如果你只选择了一个元素那么需要多个元素的约束将会被禁用。  

从Align和Pin菜单中添加约束:   

1. 选择合适的约束的旁边的复选框.为了选择一个间距最近的约束，选择对于该元件的适当多一侧的红色的约束。    
![spacing_to_nearest_neighbor](/public/img/spacing_to_nearest_neighbor.png)    
如果你需要创建一个相对于其他的不是距离最近的视图的约束，单击黑色三角形下拉菜单中的文本字段来显示其他的距离较近的视图。  
2. 输入对应的约束的值。  
3. 单击添加约束按钮创建约束   
	- 添加约束按钮 添加了选中的元件的新的约束。   
	- 添加并更新框架按钮 给选中的元件添加新的约束，并且移动这个元素也能适应每个约束适应的很好。 

> Note: 每次点击这两个按钮中的一个你都会添加一个新的约束，你不是编辑已经存在的约束。  

### 添加缺失的或者建议的约束 ###
当你需要一个布局的起点或者需要一个很多的快速变化需要用Issues menu添加约束。  

如果你需要添加大量的约束来描述你的界面的布局，并且你不希望一次只添加一个，这时候可以选择Issues->Add Missing Constraints来添加一组明确的约束。 这个命令是依据现有的布局来推断出的约束。  

如果你需要还原大量的没有错误的约束或者你只是想重新开始，选择Issues->Reset to Suggested Constraints来移除错误的约束并添加一组明确的约束。这相当于清除约束之后添加缺失的约束。

### 编辑约束 ###
你可以改变约束的常量，关系和优先级。你可以编辑这些属性通过双击这个画布上的约束并编辑相应的值活着通过选择约束使用属性编辑框。你不能改变约束的类型。例如你不能把一个宽度的约束变成一个高度的约束。  

### 删除约束 ###
任何时候都可以在画布上或者大纲视图上来选择约束并按下删除按钮来删除约束。  

## 编程的方式使用Auto Layout ##

尽管界面编辑器提供了一个方便的可视化的界面来用Auto Layout工作，你仍然可以通过代码来添加，删除，调整约束。如果你在运行时添加或者移除视图，你需要以编程的方式添加约束来保证你的界面能正确的响应大小和方向上的变化。  

### 以编程的方式创建约束

你用`NSLayoutConstraint`的实例来代笔约束。为了创建约束，你通常用`constraintsWithVisualFormat:options:metrics:views:`.  

这个方法的第一个参数是一个视觉格式化字符串，提供一个你想描述的布局的可视化的表示。这个视觉形式语言被设计的尽可能多可读。一个视图被表示为一个方括号，视图之间的连接用连字符号表示，或者被一个数字分开的两个连字符号用来表示视图之间的距离。为了了解视觉形式语言的更多的例子和语法，请参考[视觉形式语言](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1)。  

举个例子：你可能会表示两个按钮之间的约束：  

![twoButtons](/public/img/twoButtons.png)    

用如下的视觉形式语言表示：`[button1]-12-[button2]`.  
一个单独的连字符号表示标准的Aqua空间，所以你可以这样来表达它们之间的关系：`[button1]-[button2]`.   

视图的名称来自视图字典－字典Key值就是在格式化字符串中使用的名字，字典的值就是视图对象。比较方便的是`NSDictionaryOfVariableBindings`创建一个Key和值名字相等的字典。下面是创建约束的代码:  
  
	NSDictionary *viewsDictionary = NSDictionaryOfVariableBindings(self.button1,self.button2);
	NSArray *constraints = [NSLayoutConstraint constraintsWithVisualFormat:"[button1]-[button2]" 
																   options:0 
																   metrics:nil 
																     views:viewsDictionary]; 
视觉形式化语言更喜爱良好的可视化的完整性的表达。尽管大多数的用户界面上的有用的约束可以被语言来表达，但是一些不可以。一个有用的不能被语言来表达的约束是一个固定的宽高比。（例如：`imageView.width = 2 * imageView.height`）.创建这样的一个约束你可以用 `constraintWithItem:attribute:relatedBy:toItem:attribute:multiplier:constant:`.  

作为一个例子：你也可以用这个方法来创建之前的 `[button1]-[button2]` 的约束：  

	[NSLayoutConstraint constraintWithItem:self.button1 attribute:NSLayoutAttributeRight
								 relatedBy:NSLayoutRelationEqual 
								    toItem:self.button2
                      			 attribute:NSLayoutAttributeLeft
                      		    multiplier:1.0 
                      		      constant:-12.0];  

### 添加约束 ###

为了使一个约束处于激活状态，你必须把它添加到一个视图中去。这个包含约束的视图必须是涉及到这个约束的视图们的父视图，并且应该选择最近的祖先视图。（这是一个存在的`NSView`的API的祖先这个词的场景，一个视图的祖先就是它自己.）在这个视图的坐标系中解析这个约束。  

假设你在`zoomableView1`和`zoomableView2`中添加`[zoomableView1]-80-[zoomableView2]`这个约束。  

![helloHi1](/public/img/helloHi-1.png)  

80就是这个容器的坐标系中的值，就像是你自己画的约束一样。如果zoomableviews中的任何一个的边界改变了，他们之间但间隔仍然保持固定。  

![helloHi1](/public/img/helloHi-2.png)  

如果这个容器的边界值改变了，他们之间的间隔按照一定的比例缩放。    

![helloHi1](/public/img/helloHi-3.png)  

`NSView`提供了方法`addConstraint:`、`removeConstraint:`、`constraints`来添加约束，移除现有的约束，获取相关的约束。 `NSView`还提供了一个方法，`fittingSize`这个方法和`NSControl`的`sizeToFit`的方法很相似，但是这是针对很随意的视图的，并不是对带有操作的控件（`NSControl`）的。  

`fittingSize`方法返回的是对视图来说考虑它所有子视图的尽可能小的限制的理想的大小，这个适合的大小并不是这个视图在一个一般的带有约束的场景中的最好的大小。一个视图的最好的大小（如果你考虑所有的事情）是由它当前的大小所定义的。
