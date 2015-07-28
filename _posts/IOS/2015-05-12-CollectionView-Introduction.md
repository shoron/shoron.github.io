---
layout: post
title: CollectionView 介绍
category: iOS
tags: UICollectionView
keywords: 
description:
---

#### CollectionView 介绍 ####

我所理解的CollectionView就像是系统给提供的一个大画布，一个容器。你自己需要定义画布上显示的内容以及各个元素的属性和显示的规则。在这个画布上一共可以显示两种类型的视图，UICollectionViewCell，UICollectionReusableView.UICollectionViewCell是这个画布上最主要的元素。UICollectionReusableView是作为UICollectionViewCell的辅助信息出现的（例如UITableView的Header和Footer），可有可无。布局对象来管理CollectionView出现的一系列的视图。  

CollectionView是由[dataSource和delegate](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html)驱动的。CollectionView的数据源是由数据源对象来提供的。CollectionView的行为是有delegate对象了来提供的。CollectionVview的数据布局是由布局对象来管理的。CollectionView只是提供了显示内容的容器而已。  

#### CollectionView 支持的视图 ####

* cell. cell用来提供你的CollectionView中的最主要的内容。由数据源提供。
* Supplementary views.追加视图。用来显示一个section相关点信息。由数据源提供。
* Decoration views.装饰视图。由布局对象提供的。和数据没有任何关系的。用作背景展示。  

#### Layout Object 自动布局对象

自动布局对象管理每一个视图的属性信息。包括

