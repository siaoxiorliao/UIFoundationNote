## 功能设计

> 添加商品,显示对应的商品  
> 减少商品,删除对应商品  
> 购物车空或满,显示对应提示

### 他的view增加的思路

![总体思路](/1228/images/WX20170720-202727.png)

* 根据添加的商品个数求3的余来换行,求得的余即是商品对应的行和列,再根据行列得到商品的位置

```objectivec

#import "ViewController.h"

@interface ViewController ()

// 购物车
@property (weak, nonatomic) IBOutlet UIView *shopCarView;
// 添加按钮0
@property (weak, nonatomic) IBOutlet UIButton *addButton;
// 删除按钮
@property (weak, nonatomic) IBOutlet UIButton *removeButton;
// 全局的下标
//@property (nonatomic, assign) NSInteger index;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // 给下标赋值
//    self.index = 0;
    
//    // 裁剪多余部分(不可取)
//    self.shopCarView.clipsToBounds = YES;
}

/**
 *  添加到购物车
 *
 *  @param button 按钮
 */
- (IBAction)add:(UIButton *)button {
/***********************1.定义一些常量*****************************/
    // 1.总列数
    NSInteger allCols = 3;
    // 2.商品的宽度 和 高度
    CGFloat width = 80;
    CGFloat height = 100;
    // 3.求出水平间距 和 垂直间距
    CGFloat hMargin = (self.shopCarView.frame.size.width - allCols * width) / (allCols -1);
    CGFloat vMargin = (self.shopCarView.frame.size.height - 2 * height) / 1;
    // 4. 设置索引
    NSInteger index = self.shopCarView.subviews.count;
    // 5.求出x值
    CGFloat x = (hMargin + width) * (index % allCols);
    CGFloat y = (vMargin + height) * (index / allCols);
    
/***********************2.创建一个商品*****************************/
  // 1.创建商品的view
    UIView *shopView = [[UIView alloc] init];
  // 2.设置frame
    shopView.frame = CGRectMake(x, y, width, height);
  // 3.设置背景颜色
    shopView.backgroundColor = [UIColor greenColor];
  // 4.添加到购物车
    [self.shopCarView addSubview:shopView];
    
/***********************3.设置按钮的状态*****************************/
//    if (index == 5) {
//        button.enabled = NO;
//    }
    button.enabled = (index != 5);
    
    // 5.设置删除按钮的状态
    self.removeButton.enabled = YES;
    
    // 让下标+1
//    self.index += 1;
}

/**
 *  从购物车中删除
 *
 *  @param button 按钮
 */
- (IBAction)remove:(UIButton *)button {
    // 1. 删除最后一个商品
    UIView *lastShopView = [self.shopCarView.subviews lastObject];
    [lastShopView removeFromSuperview];
    
    // 2.设置索引值 -1
//    self.index -= 1;
    
    // 3. 设置添加按钮的状态
    self.addButton.enabled = YES;
    
    // 4. 设置删除按钮的状态
    /*
    if (self.shopCarView.subviews.count == 0) {
        self.removeButton.enabled = NO;
    }
     */
    self.removeButton.enabled = (self.shopCarView.subviews.count != 0);
}

@end





```
# 我的扩展

* 一行放6个 间隔5.0
* 可增加更多的商品,直到超出手机范围
* 全代码实现

### 我的增加view的思路

* 手机宽度是不变的,商品view是不变的,所以有限宽度只能装下一定个数的商品:
  先计算一行能添加商品的个数,当新的商品被添加,则判断是否超过手机宽度,超过则重置x,y增加一个高度和间隔.
* 用临时每行和每列的个数,和位置确定要增加和删除的位置
#### 我的View设计代码

```objectivec
#import "ViewController.h"

@interface ViewController ()

@property (nonatomic,weak) UIView *shopCarView;
@property (nonatomic,weak) UIButton *addBtn;
@property (nonatomic,weak) UIButton *removeBtn;
@property (nonatomic,weak) UILabel *lab;
@property (nonatomic,assign) NSInteger goodsXCount;
@property (nonatomic,assign) NSInteger goodsYCount;
@property (nonatomic,assign) CGFloat tempX;
@property (nonatomic,assign) CGFloat tempY;
@end

@implementation ViewController

CGFloat bounds = 5.0;

- (void)viewDidLoad {
    [super viewDidLoad];
    _goodsXCount = 0;
    _goodsYCount = 0;
    _tempX = 60.0;
    _tempY = 105.0;
    [self initShopCarView];
    [self initAddButton];
    [self initRemoveButton];
}

- (void)clickAddBtn:(UIButton *) sender {
    self.lab.hidden = YES;
    self.removeBtn.enabled = YES;
    CGFloat width = 50.0;
    CGFloat height = 100.0;
    if (_tempX <= self.shopCarView.frame.size.width || _tempY <= self.shopCarView.frame.size.height){
        if (_tempX > self.shopCarView.frame.size.width){
            _goodsXCount = 0;
            _goodsYCount += 1;
            _tempX = 60.0;
            _tempY += (height + bounds);
        }
        if (_tempY > self.shopCarView.frame.size.height){
            self.addBtn.enabled = NO;
            [self initLabWith:@"购物车都满了,你这么富的吗,dailao"];
            return;
        }
        [self addView:width andHeight:height];
        _tempX += (width + bounds);
        _goodsXCount += 1;
    }
}

- (void)clickRemoveBtn:(UIButton *) sender {
    [self.lab removeFromSuperview];
    UIView *lastView = [self.shopCarView.subviews lastObject];
    CGFloat x = lastView.frame.origin.x;
    CGFloat y = lastView.frame.origin.y;
    [self resetXY:x withY:y];
    [lastView removeFromSuperview];
}

-(void)resetXY: (CGFloat) x withY: (CGFloat) y {
    self.addBtn.enabled = YES;
    self.tempX = x + 55;
    self.tempY = y + 100;
    self.goodsXCount = ((x-5)/55);
    self.goodsYCount = (y-5)/105;
    if (_goodsXCount == 0 && _goodsYCount == 0){
        self.removeBtn.enabled = NO;
        [self initLabWith:@"这么穷,连购物车都要整整吗? qixi"];
    }
}

- (void)apperaRemind{
    if (self.shopCarView.subviews.count == 0){
        self.lab.hidden = NO;
    }else{
        self.lab.hidden = YES;
    }
}

- (void)initLabWith: (NSString *)text{
    UILabel *lab = [[UILabel alloc]init];
    lab.frame = CGRectMake(self.shopCarView.frame.size.width * 0.1,self.shopCarView.frame.size.height * 0.45, self.shopCarView.frame.size.width * 0.8, self.shopCarView.frame.size.height * 0.1);
    lab.text = text;
    lab.backgroundColor =[UIColor whiteColor];
    lab.textColor = [UIColor blackColor];
    lab.textAlignment = NSTextAlignmentCenter;
    self.lab = lab;
    [self.shopCarView addSubview:self.lab];
}

- (void)addView:(CGFloat) width andHeight:(CGFloat) height{
    
    UIView *goodsView = [[UIView alloc]initWithFrame:CGRectMake(bounds + _goodsXCount * (width + bounds),
                                                                bounds + _goodsYCount * (height + bounds),
                                                                50, 100)];
    goodsView.backgroundColor = [UIColor greenColor];
    [self.shopCarView addSubview:goodsView];
    
}

- (void)initAddButton {
    UIButton *addBtn = [UIButton buttonWithType:UIButtonTypeCustom];
    addBtn.frame =CGRectMake(self.shopCarView.frame.origin.x, self.shopCarView.frame.origin.y - 60, 50, 50);
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
    UIView *shopCarView = [[UIView alloc]initWithFrame:CGRectMake(40, 300, self.view.frame.size.width - 79, 425)];
    shopCarView.backgroundColor = [UIColor orangeColor];
    self.shopCarView = shopCarView;
    [self.view addSubview:self.shopCarView];
}

@end

```
### 对比

* 虽然我的代码稍微多一点,但是我做了间隔,是全代码实现,并且扩展性很好,改变购物车大小或商品view大小也很容易适配,但是阅读性不怎么好,很笨的方法



