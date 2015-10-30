---
layout: post
title: iOS 给图片添加水印
category: iOS
tags: iOS
keywords:
description:
---

### 图片添加水印 ###

    // UIImage 的 Category 方法
    - (UIimage *)imageWithWaterMarkImage:(UIImage *)waterMark {
    	UIGraphicsBeginImageContext(self.size); // 这不能写屏幕的大小。否则画出来的图片会失真
        CGContextRef context = UIGraphicsGetCurrentContext();
        
        // 因为 iOS view的原点坐标是左下角。但是画布的原点是左上角 
        CGContextTranslateCTM(context, 0, self.size.height);
        CGContextScaleCTM(context, 1.0, -1.0);
        
        // draw image and watermark image
        CGRect imageRect = CGRectMake(0, 0, self.size.width, self.size.height);
        CGContextDrawImage(context,imageRect,[self CGImage]);
        
        // the origin is left cornor
        CGRect waterMarkRect = CGRectMake(50, self.size.height - 172, 146, 172);
        CGContextDrawImage(context, waterMarkRect, [waterMarkImage CGImage]);
        UIImage *newImage = UIGraphicsGetImageFromCurrentImageContext();
        
        // end drawing image
        UIGraphicsEndImageContext();

        return newImage;
    }

备注： 把绘图的部分要写在线程里。不要写的主线程。
