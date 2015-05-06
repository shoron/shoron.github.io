---
layout: post
title: ViewController Programming Guide For IOS（二）窗口
category: IOS
tags: IOS IOS官网文档
keywords: 
description:
---

以下是从原文档中提炼的一些知识点

1. 涉及到窗口的两个任务。
	* 从窗口到坐标系统转换一个点或者矩形到一个特定的视图的坐标系统中（反之亦然）
	* 当窗口发生变化时用相关的通知来追踪窗口。
2. 如果用nib文件创建窗口时，默认是全屏显示的。当你设置为非全屏显示时，超出窗口的位置是收不到touch事件的。
3. 当设置了窗口的根视图或者根视图控制器之后，还要设置它的大小和位置。对于有状态栏的应用程序要适当的减少根视图的大小，以防止视图被遮挡住。但是当窗口的根视图是是容器视图控制器（tab，navigation，split等视图控制器）的情况下，不需要设置大小。它会自动设置大小。  
4. 监控窗口的显示和消失可以用以下几个通知：
	* UIWindowDidBecomeVisibleNotification
	* UIWindowDidBecomeHiddenNotification
	* UIWindowDidBecomeKeyNotification
	* UIWindowDidResignKeyNotification
5. 如果想在额外的窗口上显示内容，需要创建一个额外的窗口。创建的窗口默认是和屏幕一样大小的。当连接上额外的屏幕的时候会给应用发送相应的连接和断开连接的通知.当你的程序在活跃的时候连接了一个新的显示设备，此时你应该给这个新的显示设备显示一些内容。可以不必是你最终要显示的内容，否则的话，这个新的设备会是一个黑屏。
	<pre>
	设置收到连接，断开连接的通知
	NSNotificationCenter* center = [NSNotificationCenter defaultCenter];
    [center addObserver:self selector:@selector(handleScreenConnectNotification:)
            name:UIScreenDidConnectNotification object:nil];
    [center addObserver:self selector:@selector(handleScreenDisconnectNotification:)
            name:UIScreenDidDisconnectNotification object:nil];
            
    处理连接的通知
    -(void)handleScreenConnectNotification:(NSNotification\*)aNotification {
    UIScreen \*newScreen = [aNotification object];
    CGRect screenBounds = newScreen.bounds;
    if (!_secondWindow) {
        _secondWindow = [[UIWindow alloc] initWithFrame:screenBounds];
        _secondWindow.screen = newScreen;
        [viewController displaySelectionInSecondaryWindow:_secondWindow];
    }
}
	处理断开连接的通知
-(void)handleScreenDisconnectNotification:(NSNotification*)aNotification {
    if (_secondWindow){
        // Hide and then delete the window.
        _secondWindow.hidden = YES;
        [_secondWindow release];
        _secondWindow = nil;
        [viewController displaySelectionOnMainScreen];
    }
}
         
6. 在展示一个窗口之前必须把它和一个屏幕关联起来。
7. 如果你想用一个非默认的屏幕模式，你应该在关联屏幕和窗口之前应用它。在UIScreenMode中定义了耽搁屏幕模式的属性。