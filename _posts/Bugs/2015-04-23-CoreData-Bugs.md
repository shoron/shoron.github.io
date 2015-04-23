---
layout: post
title: 常见Bugs
category: "Bugs"
tags: IOS Bugs
keywords: Bugs
description: 
---

这篇文章中整理的都是一些自己开发中遇到的一些典型的Bug。

#### CoreData的实体之间的关系 ######

+ Bug描述：在做评论回复的过程中出现了只有回复该条评论的最新的一个回复才能正确显示原评论的昵称。之前的改评论的回复的全部显示为null。    
+ Bug原因：在评论实体的关系中，评论的有一个Relationships是parent用来存储所回复的那条评论，该Relationship还是指向评论实体。    

![errorCommentRelationshipSetting](/public/img/errorCommentRelationshipSetting.png)   

+ 解决方案：修改Relationships设置如下图所示（其中parent那个relationship是to－one。children是to－many。）

![correct](/public/img/correctCommentRelationshipSetting.png)    
