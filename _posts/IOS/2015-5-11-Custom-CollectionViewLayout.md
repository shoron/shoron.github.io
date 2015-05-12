---
layout: post
title: 瀑布流CollectionView的实现过程
category: IOS
tags: UICollectionView
keywords: 
description:
---

由于工作需要，自己写了一个UICollectionView的瀑布流的实现。在此总结一下自己的学习过程。

### CollectionView的理解 ###

我所理解的CollectionView就像是系统给提供的一个大画布。你自己需要定义画布上显示的内容以及各个元素的属性和显示的规则。在这个画布上一共可以显示两种类型的视图，UICollectionViewCell，UICollectionReusableView.UICollectionViewCell是这个画布上最主要的元素。UICollectionReusableView是作为UICollectionViewCell的辅助信息出现的（例如UITableView的Header和Footer），可有可无。对于我们写自定义的UICollectionView就是需要提供自定义的管理，显示规则（UICollectionViewLayout）和自定义的内容。

说完比喻，说一些技术层面上的。UICollectionView和UITableView一样也是由数据（dataSource）和事件（delegate）驱动的。CollectionView中的Cell和Supplementary views都是由数据源提供的，并且由布局对象控制。关于UICollectionView的自定义实际上也就是数据源和布局对象的的自定义。

### 自定义CollectionView的数据源 ###

数据源有两类，一类是UICollectionViewCell，一类是UICollectionViewReusableView.

#### 自定义UICollectionViewCell ####

1. 实现UICollectionViewCell的一个子类（eg：YSRCollectionViewCell）。
2. 向CollectionView注册实现的cell的子类YSRCollectionViewCell。
	<pre><code>registerClass:forCellWithReuseIdentifier:;(纯代码实现cell的子类的时候用)
	registerNib:forCellWithReuseIdentifier:;(用nib实现cell的子类的时候用)</code></pre>  
3. 实现数据源(UICollectionViewDataSource)的函数。
	```
	collectionView:numberOfItemsInSection:和collectionView:cellForItemAtIndexPath:
	```

#### 自定义UICollectionViewLayout ####

布局对象是UICollectionView中的难点也是UICollectionView中最强大的地方。我们来一起学习一下。  

1. 首先我们来看一下UICollectionView的继承关系。
2. 











鸣谢：[answer-huang](http://objccn.io/issue-3-3/),[枯木园](http://blog.csdn.net/lmbda/article/details/18051801),[onevcat](http://onevcat.com/2012/08/advanced-collection-view/),[一个waterFlow的实现](https://github.com/linshaolie/WaterFlow)