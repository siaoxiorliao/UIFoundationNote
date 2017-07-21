## 功能设计

> 添加商品,显示对应的商品  
> 减少商品,删除对应商品  
> 购物车空或满,显示对应提示

### 他的view增加的思路

![总体思路](/1228/images/WX20170720-202727.png)

* 根据添加的商品个数求3的余来换行,求得的余即是商品对应的行和列,再根据行列得到商品的位置

# 我的扩展

* 一行放6个 间隔5.0
* 根据选择的图片添加对应商品
* 可增加更多的商品,直到超出手机范围
* 全代码实现

### 我的增加view的思路

* 手机宽度是不变的,商品view是不变的,所以有限宽度只能装下一定个数的商品:
  先计算一行能添加商品的个数,当新的商品被添加,则判断是否超过手机宽度,超过则重置x,y增加一个高度和间隔.
* 可以尝试通过瀑布流,自定商品view大小来添加...这个以后做吧

#### 我的View设计代码

```objectivec
#import "ViewController.h"

@interface ViewController ()

@property (nonatomic,weak) UIView *shopCarView;
@property (nonatomic,weak) UIButton *addBtn;
@property (nonatomic,weak) UIButton *removeBtn;

@property (nonatomic,assign) NSInteger goodsXCount;
@property (nonatomic,assign) NSInteger goodsYCount;
@property (nonatomic,assign) CGFloat tempX;

@end

@implementation ViewController

CGFloat bounds = 5.0;

- (void)viewDidLoad {
    [super viewDidLoad];
    _goodsXCount = 0;
    _goodsYCount = 0;
    _tempX = 0;
    [self initShopCarView];
    [self initAddButton];
    [self initRemoveButton];
}

- (void)clickAddBtn:(UIButton *) sender {

    CGFloat width = 50.0;
    CGFloat height = 100.0;
    if (_goodsXCount < [self calculateCount]){
        if (_tempX > self.shopCarView.frame.size.width){
            _goodsYCount += 1;
        }
        [self addView:width andHeight:height];
            _tempX += width + bounds;
            _goodsXCount += 1;
//        NSLog(@"Xcount : %d",_goodsXCount);

    }else{
        _goodsXCount = 0;
        _goodsYCount += 1;
        _tempX = 0;
        [self addView:width andHeight:height];
        _tempX += width + bounds;
        _goodsXCount += 1;
//        NSLog(@"Xcount: %d ,Ycount : %d",_goodsYCount,_goodsYCount);
    }
}

- (void)addView:(CGFloat) width andHeight:(CGFloat) height{

    UIView *goodsView = [[UIView alloc]initWithFrame:CGRectMake(bounds + _goodsXCount * (width + bounds),
                                                                bounds + _goodsYCount * (height + bounds),
                                                                50, 100)];
    goodsView.backgroundColor = [UIColor greenColor];
    [self.shopCarView addSubview:goodsView];
}

- (void)clickRemoveBtn:(UIButton *) sender {
    NSLog(@"减少一个商品");
}

- (NSInteger)calculateCount {
    return ((NSInteger)self.shopCarView.frame.size.width - bounds)/(50 + bounds);
}

- (void)initAddButton {
    UIButton *addBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    addBtn.frame =CGRectMake(20, self.shopCarView.frame.origin.y - 60, 50, 50);
    [addBtn setImage:[UIImage imageNamed:@"add"] forState:UIControlStateNormal];
    [addBtn setImage:[UIImage imageNamed:@"add_disabled"] forState:UIControlStateDisabled];
    [addBtn setImage:[UIImage imageNamed:@"add_highlighted"] forState:UIControlStateHighlighted];
    [addBtn addTarget:self action:@selector(clickAddBtn:) forControlEvents:UIControlEventTouchUpInside];
    self.addBtn = addBtn;
    [self.view addSubview:self.addBtn];
}

- (void)initRemoveButton {
    UIButton *removeBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    removeBtn.frame =CGRectMake(self.addBtn.frame.origin.x + self.shopCarView.frame.size.width - 50,self.addBtn.frame.origin.y, _addBtn.frame.size.width, _addBtn.frame.size.height);
    [removeBtn setImage:[UIImage imageNamed:@"remove"] forState:UIControlStateNormal];
    [removeBtn setImage:[UIImage imageNamed:@"remove_disabled"] forState:UIControlStateDisabled];
    [removeBtn setImage:[UIImage imageNamed:@"remove_highlighted"] forState:UIControlStateHighlighted];
    removeBtn.enabled = NO;
    [removeBtn addTarget:self action:@selector(clickRemoveBtn:) forControlEvents:UIControlEventTouchUpInside];
    self.removeBtn = removeBtn;
    [self.view addSubview:self.removeBtn];
}

- (void)initShopCarView {
    UIView *shopCarView = [[UIView alloc]initWithFrame:CGRectMake(20, 300, self.view.frame.size.width - 40, 500)];
    shopCarView.backgroundColor = [UIColor orangeColor];
    self.shopCarView = shopCarView;
    [self.view addSubview:self.shopCarView];
}

@end
```



