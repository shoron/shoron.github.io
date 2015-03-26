---
layout: post
title: UIView Bugs
category: "Bugs"
tags: IOS Bugs
keywords: Bugs
description: 
---

这篇文章中整理的都是一些自己开发中遇到的小的问题点。

####  UIView的alpha值 #######

UIView的alpha设置了之后，它的subViews的alpha值也会相应的改变。若想只改变父View的透明度可以设置父View的背景颜色为带透明度的Color。


