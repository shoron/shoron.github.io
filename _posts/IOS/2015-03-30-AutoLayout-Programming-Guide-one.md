---
layout: post
title: AutoLayout Programming Guide 翻译(一)之自动布局基础
category: iOS
tags: iOS iOS官网文档
keywords: 
description:
---

在学习 iOS 开发的过程中，一直都是通过其他博客的教程来学习的，但是其他博客的教程难免会有一些疏漏之处，在工作中经常会因为一些小的点浪费大把的时间。于是乎痛定思痛，决定以后都从苹果官网中学习 iOS 开发者知识。之前由于英语水平的问题一直不是太喜欢看英文文档，现在看来不提升自己的英语水平是真的不行了。不仅仅是 iOS 开发，多数的技术文档的第一手资料都是英文的。所以从今天开始翻译官网的一些开发者文档。  

[原文链接](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/AutolayoutPG/AutoLayoutConcepts/AutoLayoutConcepts.html#//apple_ref/doc/uid/TP40010853-CH14-SW1)。   

## Auto Layout简介 ##
Auto Layout 是一个可以让你的App的用户界面通过创建一个元素之间的数据表达式关系来自动布局的一个系统。你可以定义这些关系，无论是在单个元素之间还是在元素的集合之间来定义约束。使用 Auto Layout，你可以创建动态的，炫酷的界面，来适当的响应在屏幕尺寸，设备方向和定位上的变化。Auto Layout 可以在 Xcode5 中的 IB 文件中创建，并且它可以用在 iOS 和 OSX 中。当你创建一个新的工程的时候，Auto Layout 默认是开启的。如果你有一个现存的没有使用 Auto Layout 的工程。你可以阅读应用 Auto Layout 章节。

典型的创建用户界面的流程是通过使用界面生成器来创建，调整大小和自定义你的视图和控件。当你对当前视图或者控件的位置和设置满意的时候就可以开始添加自动布局的约束了，以便你的界面可以适应方向，大小和定位的变化。

在 Xcode5 中 Auto Layout 提供了强大的工作流来快速容易的创建和维护在 OSX 和 iOS 中的应用。在 Xcode5 中你可以：   

- 添加约束当你准备好的时候。  
- 使用 Control－drag 或者菜单选项来快速的添加约束。  
- 分别更新约束和框架的大小。  
- 为动态的视图指定占位符约束.   
- 看见，理解，解决模棱两可或者冲突的视图。

### Auto Layout概念 ###

Auto Layout 的基本构建模块是 constraint (约束)。约束表达了你的界面中的元素的布局的规则。例如：你创建了一个约束来制定一个元素的宽度，或者它在水平方向和另外一个元素的距离。你添加，移除或者改变这个约束点属性都会影响你的界面的布局。当计算用户界面上的元素在运行时的位置时，Auto Layout系统会同时计算所有的约束并且按照最能满足所有约束的情况来设置元素的位置。

### 约束的基础 ###
你可以把一个约束当作符合人的表达习惯的一个表达式。例如如果你定义一个按钮的位置，你可能想说“按钮的左侧边缘应该距离它的父视图的左侧边缘20个点”。更正式地说，这个按钮点约束应该是：`button.left = (container.left + 20)`,这是从`y ＝ m*x + b`表达式中转化来的。在这   

- x 和 y是视图的属性。    
- m 和 b是float值.   

属性包括：`left, right, top, bottom, leading, trailing, width, height, centerX, centerY`和`baseline`。在从左到右到语言（例如英语）中leading，trailing和left，right是一样的，但是在从右到左的语言（例如希伯来语和阿拉伯语）中，leading，trailing 和 right left是一致的。当你创建约束的时候默认使用 leading 和 trailing。你通常都应该用 leading，trailing 来确保你的界面在所有语言中都是合适的，除非你正在创建的约束可以在任何语言中都保持一致。


约束的属性有：  

- 常量（Constant Value）：约束在物理上的大小和位移（offset）的点数.   
- 关系（Relation） ：Auto Layout 在视图属性上不仅仅支持约束常量。你可以使用关系和不平等（例如：大于等于，小于等于）。例如一个视图点宽度 >= 20 或者 textView。leading >= (superview.leading + 20).   
- 优先级（Priority Level）。约束有一个优先级。拥有高优先级的约束比低优先级的约束要更优先满足。默认的优先级别是要求的（NSLayoutPriorityRequired），这意味着这个约束必须被精确的适配。Layout系统尽可能的去适配一些可选的约束，哪怕他们不被适配也可以。优先级允许你来表达有用的有条件的行为。例如：它们可以用来描述以下行为：一些控件在没有更重要的优先级的情况下应该适应它的内容的大小。关于优先级的更多的信息请参考 NSLayoutPriority.

约束是可以累积的，不会互相覆盖。当你在有一个已经存在的约束时，设置另一个同一类型点约束时不会覆盖掉之前的约束。例如：给视图设置另外一个宽度约束不会移除或者改变第一个宽度约束－你需要手动的移除掉第一个约束。


你不能跨越视图的层级来设置约束。

### 内在的内容大小 ###

子视图，例如按钮通常比代码更清楚他们的大小。这就是通过固有视图的大小来告诉自动布局系统一个视图包含了一下不太理解对内容的大小，并且该固有内容的大小来指示这个内容应该是多大。例如 text labels，你应该特别的指定它的固有的大小。这意味着这个元素将会随着不同内容不同语言适当的增长或者收缩。

### 应用构架 ###

Auto Layout结构分配了控制器和视图之间的布局的责任。与其写一个智能的知道视图在哪个位置的控制器，不如让视图变的更容易自我管理。这种方法降低了控制器逻辑的复杂性，并且不需要相应变化的布局代码就更容易的重新设计视图。你可能仍然希望控制器在运行时来添加移除或者调整约束。可以在“用代码写AutoLayout”学习更多的知识。

#### 控制器的角色 ####

尽管一个视图指定其内在内容的大小，视图的使用者决定它的优先级。例如在默认情况下一个按钮  

- 强烈的希望一个按钮可以自然展示它在垂直方向上的内容（按钮也真的应该是它们的自然高度）
- 不是那么强烈的希望自然展示它的内容。（标题和内容之间的空隙是可以接受的）
- 强烈抵抗水平和垂直方向上裁减内容
例如在用户界面上包含互相挨着的两个按钮，这是由控制器来决定当有额外的空间的时候这两个按钮是否应该变长。是否是只有其中一个按钮变长？是否两个按钮等同增长？又或者它俩保持一定的比例？如果没有足够的空间来适应两个按钮不压缩裁减内容，是否其中一个按钮应该变短，或者都变短，等等。  

你可以用`setContentHuggingPriority:forAxis:`和`setContentCompressionResistancePriority:forAxis:`来设置一个视图的实例的延展性和压缩性。默认情况下所有的UIKit和AppKit都提供了的视图都有`NSLayoutPriorityDefaultHigh`和`NSLayoutPriorityDefaultLow`的值。

