---
layout: post
title: 瀑布流CollectionView的实现过程
category: iOS
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

布局对象（UICollectionViewLayout）是UICollectionView中的难点也是UICollectionView中最重要也是最强大的地方。我们来一起学习一下。  

首先我们来一起看一下自定义UICollectionViewLayout时涉及到的一些比较重要的函数。  

- prepareLayout:  
	- 子类必须调用 [super prepareLayout];  
	- 这个方法是用来生成各个cell和reusableView的布局对象（UICollectionViewLayoutAttributes）的函数，当生成布局对象的时候会掉用此函数。我们可以看一下UICollectionViewLayoutAttributes的成员，都是一些cell的成员变量。所以prepareLayout这个函数就是把每个元素（cell和reusableView）长什么样什么位置给确定下来。
	- 当布局对象(UICollectionViewLayout)掉用invalidated方法后，再次请求布局对象的信息之前会掉用此函数（等于刷新各个cell的布局）。
- layoutAttributesForElementsInRect:  
	- 这个方法意思是返回在该Rect中的各个cell和ReusableView的布局对象（UICollectionViewLayoutAttributes）。我们可以判断每个元素的frame是否在这个rect内。然后返回相关的元素的布局对象的array即可。当然我们也可以用一个偷懒的做法，直接返回所有的元素的布局对象。  
- layoutAttributesForItemAtIndexPath:  
	- 这个方法就是返回特定的IndexPath的cell的布局对象。
- layoutAttributesForSupplementaryViewOfKind:atIndexPath:  
	- 这个方法是返回特定的SupplementaryView的布局对象，当然，前提是你的这个 Cunstom UICollectionViewLayout支持SupplementaryView。  
- layoutAttributesForDecorationViewOfKind:atIndexPath:  
	- 这个方法是返回特定的DecorationView的布局对象。如果你的自定义的UICollectionViewLayou对象不支持，就不用重写此方法了。  
- shouldInvalidateLayoutForBoundsChange:  
	- 这个方法是说，当CollectionView的Bounds改变时是否需要刷新布局对象。

根据上述方法的描述我们知道了自定义瀑布流的UICollectionViewLayout必须要实现的一些方法及其含义。接下来我们就要把每个cell的布局的规则弄明白。    
由于要实现的这个瀑布流的起始位置不一致，所以可以用一个delegate方法来设置每一列的起始位置。现在每一列的第一个cell的起始纵坐标我们清楚了，然后就是要弄明白哪一个cell放在哪一列上。在这我们是吧cell接在最短的列的下面。所以判断每个cell的frame的时候都要计算每一列的最大的纵坐标。所以现在cell的布局规则就是：  

1. 由相关delegate方法给出每一列的起始纵坐标
2. 计算每一列的高度和起始纵坐标的和，并把cell接到最短的列的下面。

对于SupplementaryView和DecorationView根据实际的需求来定制。  

[相关代码]()

鸣谢：[answer-huang](http://objccn.io/issue-3-3/), [枯木园](http://blog.csdn.net/lmbda/article/details/18051801), [onevcat](http://onevcat.com/2012/08/advanced-collection-view/), [一个waterFlow的实现](https://github.com/linshaolie/WaterFlow)