# View封装思想/自定义控件/layoutSubviews/数据设置思路(重要)/init和initWithFrame
---

# view封装思想和步骤
* 如果一个view内部的子控件比较多，一般会考虑自定义一个view，把它内部子控件的创建屏蔽起来，不让外界关心
* 外界可以传入对应的模型数据给view，view拿到模型数据后给内部的子控件设置对应的数据
## 封装控件的基本步骤
1. 在initWithFrame:方法中添加子控件，提供便利构造方法
2. 在layoutSubviews方法中设置子控件的frame（一定要调用super的layoutSubviews）
3. 增加模型属性，在模型属性set方法中设置数据到子控件上

## 自定义控件
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
    [super layoutSubviews];//一定要先调用父类的布局方法
    CGFloat width = self.frame.size.width;
    CGFloat height = self.frame.size.height;
    self.iconView.frame = CGRectMake(0, 0, width, height);
    self.titleLabel.frame = CGRectMake(0, width, width, height-width);
}
@end

```
> 设置数据,因为是在类扩展定义的私有属性,外面拿不到,转移到.h中
,就完成了.

## 数据设置 思路(重要)
### 方式一
* 上面**方式一**在这里就有弊端,外面可以随时访问(写)子控件(属性),封装性不好,**1229-09-01(重要,封装性讲述)**

```objectivec
goodsView.titleLabel = nil;
```
* 属性如果改成只读的呢?
> 但是属性依然能拿到**(间接访问)**
![](/1229/images/WX20170722-174848.png)

### 方式二:
* 这时,我们可以提供接口方法,供外界访问
1. 属性设置为私有
2. self.iconView -> _iconView
3. 提供setter方法接口

![](/1229/images/WX20170722-175541.png)

### 方式三:传模型(最终方案)1229-10-
* 传模型 : 直接传入模型,在类里面通过模型设置属性

![](/1229/images/WX20170722-180902.png)

![](/1229/images/WX20170722-180752.png)

### 扩展:再次封装 1229-10-09
* 提供传入模型的快速构造方法(重写模型的setter方法),在方法里面初始化控件和设置数据

```objectivec
#import "XMGShopView.h"
#import "XMGShop.h"

@interface XMGShopView ()
@property (nonatomic, weak) UIImageView *iconView;
@property (nonatomic, weak) UILabel *titleLabel;
@end

@implementation XMGShopView

- (instancetype)init{
    if (self = [super init]) {
        [self setUp];
    }
    return self;
}

- (instancetype)initWithShop:(XMGShop *)shop{
    if (self = [super init]) {
        // 注意:先创建后赋值
        [self setUp];
        self.shop = shop;//同时调用setShop
        //[self setShop:shop];也可以
    }
    return self;
}

+ (instancetype)shopViewWithShop:(XMGShop *)shop{
    return [[self alloc] initWithShop:shop];
}

-
(void)setShop:(XMGShop *)shop{
_shop = shop;//不能用self.shop,会死循环
self.iconView.image = [UIImage imageNamed:shop.icon];
self.titleLabel.text = shop.name;
}

- (void)setUp{
    UIImageView *iconView = [[UIImageView alloc] init];
    iconView.backgroundColor = [UIColor blueColor];
    [self addSubview:iconView];
    _iconView = iconView;
    UILabel *titleLabel = [[UILabel alloc] init];
    titleLabel.backgroundColor = [UIColor yellowColor];
    titleLabel.textAlignment = NSTextAlignmentCenter;
    [self addSubview:titleLabel];
    _titleLabel = titleLabel;
}

- (void)layoutSubviews{
    [super layoutSubviews];
    CGFloat width = self.frame.size.width;
    CGFloat height = self.frame.size.height;
     self.iconView.frame = CGRectMake(0, 0, width, width);
     self.titleLabel.frame = CGRectMake(0, width, width, height - width);
}

@end
```

* 使用

```objectivec
ZWGoodsView *goodsView = [[ZWGoodsView alloc]initWithModel:self.dataArr[index]];
goodsView.frame = CGRectMake(x, y, width, height);
[self.shopCarView addSubview:goodsView];
```
---
### init和initWithFrame补充

* 创建了快速构造方法后,还有initWithFrame方法没有重写
* init和initWithFrame方法功能是用来初始化子控件的
* 创建对象使用alloc init方法和initWithFrame方法:都会调用initWithFrame方法,所以不用写init方法,写initWithFrame方法即可

```objectivec
//- (instancetype)init{
//    NSLog(@"aaa");
//    if (self = [super init]) {
//        [self setUp];
//    }
//    return self;
//}

- (instancetype)initWithFrame:(CGRect)frame{
    NSLog(@"bbb");
    if (self =[super initWithFrame:frame]) {
        [self setUp];
    }
    return self;
}
```
---
### 总结:
* 创建自定义的类,一般要提供init,initWith对象方法,[类 类方法]
来提供初始化的接口,如果和视图有关,则要同时提供initWithFrame,awakeFromNib构造方法(一般抽取setUp来初始化控件),再再layoutSubView里面布局子控件,如果有数据,则提供传数据的initWith,类名With的初始化方法并同样抽取设置数据的方法(这里重写模型setter方法)和在相应初始化方法里设置数据
* ...特别是模型,以后从服务器取数据多依靠模型来设置数据....











