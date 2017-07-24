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

## 调整子控件内边距
* UIEdgeInsets insets = {top, left, bottom, right};

```objectivec
//    self.button.contentEdgeInsets = UIEdgeInsetsMake(-20, -0, 0, 0);
    self.button.imageEdgeInsets = UIEdgeInsetsMake(0, 0, 0, 0);
    self.button.titleEdgeInsets = UIEdgeInsetsMake(0, 0, 0, -10);
```

