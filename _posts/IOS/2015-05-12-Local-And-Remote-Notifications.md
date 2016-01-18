---
layout: post
title: iOS通知编程指南
category: iOS
tags: iOS Notification
keywords:
description:
---

### Notification Introduction ###
    
#### 关于本地通知和远程通知 ####

本地通知和远程通知是用户通知中的两种类型（远程通知又名推送通知）。两种通知都可以使后台的应用程序来告诉用户有新的消息。这些新的消息可以是一段文本消息，也可以是一个日历事件还可以是远程服务器发送来的数据。本地通知和远程通知看起来是一样的。它们都可以展示一段警示消息，改变应用程序图标，还可以在改变应用程序图标上的数字的时候来播放一段声音。  

值得注意的是，远程通知和本地通知是跟广播通知（NSNotificationCenter）和键值观察通知是不一回事。   
#### 注册，计划，处理本地和远程通知 ####

为了使之后的一段时间提交一个本地通知，应用程序需要注册通知类型（iOS8及其以后），创建本地通知，给它标记一个执行的日期和时间，特殊的展示细节。并按照计划提交它。为了接收远程通知，应用程序必须注册通知类型，然后把操作系统中收集到的设备口令提供给服务器。  

当操作系统提交一个本地和远程的通知，并且目标应用程序没有在前台运行，它仍然可以通过警告框，应用图标数字，或者声音把通知呈现在用户面前。如果有一个通知以警告框对方式出现在用户面前，用户点击了其中的按钮，应用程序会调用本地相关的方法。  

#### 苹果推送通知服务 ####

苹果推送通知服务（APNs）传送远程通知到有相关应用程序并且注册这些通知类型的设备。每个设备都会建立服务器任何的加密的IP连接，并在此永久连接上接收通知。  

苹果推送服务有一个反馈服务，这个反馈服务器一直维持一个发送推送通知失败的设备列表。周期性的，通知提供者需要连接到反馈服务器上来查看哪些设备无法接收通知。  


### 深度解读本地和远程通知 ###

当你的应用程序在最前面时，收到一个本地的或者远程的通知时，应用程序代理会调用
application:didReceiveRemoteNotification: 或 application:didReceiveLocalNotification:。如果应用程序在后台时，你可以通过检查方法application:didFinishLaunchingWithOptions: 中的options字典中的UIApplicationLaunchOptionsLocalNotificationKey or UIApplicationLaunchOptionsRemoteNotificationKey这两个key值来处理通知。  

#### 本地通知 ####

本地通知特别适合基于时间的一些行为，例如日历，to－do列表类应用程序。在后台运行的 iOS 应用程序如果允许本地通知，你会发现很有用处。一个本地通知是UILocalNotification（或NSUserNotification）的实例,并且带有三种属性:    

-  计划时间。你必须制定日期和时间来执行系统推送通知。这就是我们熟知的 firedate 。 你可以要求这个执行时间指定特殊的时区，以便系统在用户出游的时候调整时间。你也可以要求系统定期推送通知。  
-  通知类型。这个属性包括提示信息，默认动作按钮的标题，应用的图标通知数，播放声音，一组自定义的动作。  
-  自定义的数据。本地通知包括一个自定义的用户信息字典。  

每一个应用程序提交不超过64个计划中的本地通知。系统会丢弃超过此限制数量的通知。重复的通知认为是一个通知。

#### 远程通知 ####

远程通知一般发生在服务器有新数据需要下载的时候，而且对于客户端还没有额外的开销。大多数的远程通知是一个由APNs定义的用户如何得到相关数据的一个属性列表。尽管你可以自定义payload的属性，但是你永远也不要使用远程通知来进行数据传输。  

### 注册，定时，处理用户通知 ###