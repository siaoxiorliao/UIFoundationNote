# 自定义控件
* 在其他地方想要用到,可以封装一个自定义的控件类,以后要用的时候直接调用控件类创建就行了

## 如何自定义控件

![](/1229/images/WX20170722-165350.png)

> * 在初始化的方法里,是拿不到self.frame的(视图生命周期:先初始化,再开始布局)

* 但是有一个方法 **layoutSubviews**,是在初始化完毕后执行的**布局方法**

## layoutSubviews
* layoutSubviews是在视图初始化后执行的,是用来布局子控件的,所以在这个方法里面可以拿到子控件的frame

```objectivec

#import "ZWGoodsView.h"

@interface ZWGoodsView ()

@property (nonatomic,weak) UIImageView *iconView;
@property (nonatomic,weak) UILabel *titleLabel;
@end
@implementation ZWGoodsView
/**
 初始化子控件
 @return 子控件
 */
- (instancetype)init
{
    if(self = [super init]){
        UIImageView *iconView = [[UIImageView alloc] init];
//        iconView.frame = CGRectMake(0, 0, width, width);
        iconView.backgroundColor = [UIColor blueColor];
        [self addSubview:iconView];
        self.iconView = iconView;
        UILabel *titleLabel = [[UILabel alloc] init];
//        titleLabel.frame = CGRectMake(0, width, width, height - width);
        titleLabel.backgroundColor = [UIColor yellowColor];
        titleLabel.textAlignment = NSTextAlignmentCenter;
        [self addSubview:titleLabel];
        self.titleLabel = titleLabel;
    }
    return self;
}
- (void)layoutSubviews{
    [super layoutSubviews];
    CGFloat width = self.frame.size.width;
    CGFloat height = self.frame.size.height;
    self.iconView.frame = CGRectMake(0, 0, width, height);
    self.titleLabel.frame = CGRectMake(0, width, width, height-width);
}
@end

```
> 设置数据,因为是在类扩展定义的私有属性,外面拿不到,转移到.h中
,就完成了.

