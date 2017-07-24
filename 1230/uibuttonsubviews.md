
#自定义UIButton/图片拉伸
## 调整UIBuuton内部子控件

* 默认,按钮内部有一个imageView(左边)和titleLabel(右边),在button外面修改按钮子控件的尺寸,按钮内部会覆盖掉
```objectivec
    button.titleLabel.frame = CGRectMake(0,0,100,70);//无效果
    button.imageView.frame = CGRectMake(100,0,70,70);//无效果
```

###解决方案

方案一:

* 可以自定义UIbutton,通过重写titleRectForContentRect和imageRectForContentRect方法

```objectivec
- (CGRect)titleRectForContentRect:(CGRect)contentRect{
    return CGRectMake(0,0,100,70);
}
- (CGRect)imageRectForContentRect:(CGRect)contentRect{
    return CGRectMake(100,0,70,70);
}
```
方案二:
* 在layoutSubViews中调整

```objectivec
- (void)layoutSubViews{
    self.titleLabel.frame = CGRectMake(0,0,100,70);
    self.imageView.frame = CGRectMake(100,0,70,70);
}
``` 
---
## 调整子控件内边距
* UIEdgeInsets insets = {top, left, bottom, right};

```objectivec
//self.button.contentEdgeInsets = UIEdgeInsetsMake(-20, -0, 0, 0);
    self.button.imageEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0);
    self.button.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, -10);
```
---
## 图片拉伸

* 图片正常拉伸会失真.

### 解决方案
* 拉伸不会失真的地方,保护会失真的地方

```objectivec
    //CapInsets:保护的地方
    // 一般图片保护:1.保护上边的一半,2.保护下边的一半-1 3.保护左边的一半 4.保护右边的一半-1
    
    //方式一

    //    UIImage *resizableImage = [image resizableImageWithCapInsets:UIEdgeInsetsMake(image.size.height * 0.5,
    //                                                                                      image.size.width * 0.5,
    //                                                                                  image.size.height * 0.5 - 1,
    //                                                                                  image.size.width * 0.5 -1
    //                                                                                  )];
    
    //    UIImageResizingModeTile //平铺(填充,默认)
    //    UIImageResizingModeStretch //拉伸(伸缩)
    //    UIImage *resizableImage = [image resizableImageWithCapInsets:UIEdgeInsetsMake(image.size.height * 0.5,
    //                                                                                 image.size.width * 0.5,
    //                                                                                 image.size.height * 0.5 - 1,
    //                                                                                         image.size.width * 0.5 -1
    //                                                                                 )
    //                                                   resizingMode:UIImageResizingModeTile];
    //方式二
    //已经自动-1
    UIImage *resizableImage = [image stretchableImageWithLeftCapWidth:image.size.width * 0.5 topCapHeight:image.size.width * 0.5];
    [self.button setBackgroundImage:resizableImage forState:UIControlStateNormal];
```

### 可以为保护图片创建分类Category

* 有时需要经常拉伸图片
* 可以创建分类

```objectivec

    #import <UIKit/UIKit.h>

    @interface UIImage (XMGExtention)
    + (instancetype)resizableImageWithLocalImageName: (NSString *)localImageName;
    @end
```
---
```objectivec
    #import "UIImage+XMGExtention.h"

    @implementation UIImage (XMGExtention)
    + (instancetype)resizableImageWithLocalImageName:(NSString *)localImageName{
    UIImage *image = [UIImage imageNamed:localImageName];
    CGFloat imageWidth = image.size.width;
    CGFloat imageHeiht = image.size.height;
    return [image stretchableImageWithLeftCapWidth:imageWidth * 0.5 topCapHeight:imageHeiht * 0.5 ];
    }
    @end
```

* 也可以在assets中设置

![](/1230/images/WX20170724-121729.png)








