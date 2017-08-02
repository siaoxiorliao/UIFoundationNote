# 

### 增加圆角和边框
```objectivec
-(void)setUpBtn: (UIButton *)btn{
    btn.layer.borderWidth = 1.0;
    btn.layer.cornerRadius = 10;
    btn.layer.borderColor = [UIColor orangeColor].CGColor;
}
```

# 通知(NSNotification)
* 通知中心
* 监听通知
* 移除监听

* 系统随时都会发布通知
> UIDevie 设备通知 :
通过[UIDevice currentDevice]可以获取这个单例对象
> 通过它可以获得一些设备相关的信息，比如电池电量值(batteryLevel)、电池状态(batteryState)、设备的类型(model，比如iPod、iPhone等)、设备的系统(systemVersion)

* 键盘通知 
> 经常需要在键盘弹出或者隐藏的时候做一些特定的操作,因此需要监听键盘的状态

# 通知和代理的选择
* 共同点: 利用通知和代理都能完成对象之间的通信(比如A对象告诉D对象发生了什么事情, A对象传递数据给D对象)
* 不同点: 代理 : 1个对象只能告诉另1个对象发生了什么事情
通知 : 1个对象能告诉N个对象发生了什么事情, 1个对象能得知N个对象发生了什么事情

# 通知监听
* 通过通知发布和通知监听来监听动作

**code 160112-01**
* 发布通知

```objectivec
Cell
- (IBAction)clickAddBtn {
    self.reduceBtn.enabled = YES;
    self.wine.count ++;
    self.numLabel.text = [NSString stringWithFormat:@"%zd",self.wine.count];
    //
    [[NSNotificationCenter defaultCenter] postNotificationName:@"addClick" object:self userInfo:nil];
}

- (IBAction)clickReduceBtn {
    self.wine.count --;
    self.numLabel.text = [NSString stringWithFormat:@"%zd",self.wine.count];
    
    if(self.wine.count == 0)
    {
        self.reduceBtn.enabled = NO;
    }
    [[NSNotificationCenter defaultCenter] postNotificationName:@"reduceClick" object:self userInfo:nil];
}
```
* 监听通知

```objectivec
ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
//    self.tableView.rowHeight = 70;
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(addClick:) name:@"addClick" object:nil];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(reduceClick:) name:@"reduceClick" object:nil];
    
    
}
#pragma mark - 通知监听
- (void)addClick:(NSNotification *) noti{
    NSLog(@"add");
    self.accountBtn.enabled = YES;
    self.emptyBtn.enabled = YES;
    WineCell *cell = noti.object;
    NSInteger totalPrice = self.totalPriceLabel.text.integerValue + cell.wine.money.integerValue;
    self.totalPriceLabel.text = [NSString stringWithFormat:@"%ld",totalPrice];
}
- (void)reduceClick:(NSNotification *) noti{
    NSLog(@"reduce");
    WineCell *cell = noti.object;
    NSInteger totalPrice = self.totalPriceLabel.text.integerValue - cell.wine.money.integerValue;
    self.totalPriceLabel.text = [NSString stringWithFormat:@"%ld",totalPrice];
    if (self.totalPriceLabel.text.integerValue == 0)
    {
        self.accountBtn.enabled = NO;
        self.emptyBtn.enabled = NO;
    }
    
}

```

## 通知移除
* ios9以前通知不移除会野指针错误
* 为了习惯最好还是要移除通知

```objectivec
- (void)dealloc{
    [[NSNotificationCenter defaultCenter] removeObserver:self];
}
```

# 另一种监听方法 - KVO
**code 160112-02**

* KVO 键值监听

```objectivec
-(NSArray *)wines{
    if(!_wines){
        _wines = [WineGroup mj_objectArrayWithFilename:@"wine.plist"];
        //KVO 键值监听
        for (WineGroup *wine in _wines) {
            [wine addObserver:self forKeyPath:@"count" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:nil];
        }
    }
    return _wines;
}

- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context{

    NSInteger oldCount = [change[NSKeyValueChangeOldKey] integerValue];
    NSInteger newCount = [change[NSKeyValueChangeNewKey] integerValue];
    WineGroup *wine = object;
    if(wine.count > 0){  //防止清空和结算购count清零来到这里改变其他的值
    if (newCount>oldCount){
            NSInteger totalPrice = self.totalPriceLabel.text.integerValue + wine.money.integerValue;
            self.totalPriceLabel.text = [NSString stringWithFormat:@"%ld",totalPrice];
            self.accountBtn.enabled = YES;
            self.emptyBtn.enabled = YES;
    }else{
        NSInteger totalPrice = self.totalPriceLabel.text.integerValue - wine.money.integerValue;
        self.totalPriceLabel.text = [NSString stringWithFormat:@"%ld",totalPrice];
            if (self.totalPriceLabel.text.integerValue == 0)
            {
                self.accountBtn.enabled = NO;
                self.emptyBtn.enabled = NO;
            }
    }
}
}
//移除监听
- (void)dealloc{
    for (WineGroup *wine in _wines) {
        [wine removeObserver:self forKeyPath:@"count"];
    }
}
```

## kVO 性能问题
* 使用KVO属性监听的类,系统默认会为其创建一个子类 NSKVONotifying_类名

# 代理监听
* 普通的代理监听

```objectivec
cell.h
@property(weak,nonatomic) ViewController *delegate

cell.m
#import "ViewController.h"
- (IBAction)clickAddBtn {
    [self.delegate add];
}
-
(IBAction)reduceBtn {
[self.delegate reduce];
}

ViewController.h
-(void)add;
-(void)reduce;
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
    static NSString *ID = @"wine";
    WineCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    cell.wine = self.wines[indexPath.row];
    //设置代理
    cell.delegate = self;
    return cell;
}

ViewController.m
-(void)add;{
NSlog(@"add");
}
-
(void)reduce;{
NSlog(@"reduce");
}

```

> 这样的方法只限定了ViewController才能成为代理,耦合性高.

# 代理优化 - 代理设计模式
自定义代理
1. 添加代理属性
```objectivec
@property (nonatomic, weak) id<XMGWineCellDelegate> delegate;
```
2. 设置代理协议及代理协议方法

```objectivec
@class XMGWine ,XMGWineCell;
@protocol XMGWineCellDelegate <NSObject>
@optional
- (void)wineCellDidClickPlusButton:(XMGWineCell *)cell;
- (void)wineCellDidClickMinusButton:(XMGWineCell *)cell;
@end
```
3. 在必要时刻通知代理执行方法

```objectivec
- (IBAction)plusButtonClick {
    self.wine.count ++ ;
    self.countLabel.text = [NSString stringWithFormat:@"%d",self.wine.count];
    self.minusButton.enabled = YES;
    //通知代理执行协议中的方法
    if ([self.delegate respondsToSelector:@selector(wineCellDidClickPlusButton:)]) {
        [self.delegate wineCellDidClickPlusButton:self];
    }
}
```
4. 设置代理属性和对象

```objectivec
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *ID = @"wine";
    XMGWineCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    // 传递模型
    cell.wine = self.wineArray[indexPath.row];
    // 设置代理
    cell.delegate = self;
    
    return cell;
}
```
5. 设置代理对象遵守协议

```objectivec

@interface ViewController ()<UITableViewDataSource ,XMGWineCellDelegate ,UITableViewDelegate>
```

6. 让代理对象实现协议方法

```objectivec
- (void)wineCellDidClickPlusButton:(XMGWineCell *)cell
{
    NSLog(@"wineCellDidClickPlusButton");
}
```


