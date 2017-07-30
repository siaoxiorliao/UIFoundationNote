# 啊
# MJExtension
* 此组件有很多方便的方法可以使用
* 有自动转模型框架,可以不用再提供模型构造方法.

```objectivec
- (NSArray *)dataArr
{
    if(!_dataArr) {
        NSString *path = [[NSBundle mainBundle] pathForResource:@"tgs" ofType:@"plist"];
        /*不使用组件*/
//        NSArray *tempArr = [NSArray arrayWithContentsOfFile:path];
//        NSMutableArray *tempArrM = [NSMutableArray array];
//        for (NSDictionary *dict in tempArr) {
//            Tg *tg = [Tg TgWithDict:dict];
//            [tempArrM addObject:tg];
//        }
//        _dataArr = tempArrM;
        
        /*使用组件*/
//      _dataArr = [Tg mj_objectArrayWithFile:path];
        _dataArr = [Tg mj_objectArrayWithFilename:@"tgs.plist"];
        //如果网络加载的
         NSArray *tempArr = [NSArray arrayWithContentsOfFile:path];
        _dataArr = [Tg mj_objectArrayWithKeyValuesArray:tempArr];
        
    }
    return _dataArr;
}
```
# xib 自定义等高cell 
* 在xib中设置 控件 位置 尺寸 约束 或标识 即可
* 只需创建自定义的cell类并在类中设置数据即可.
> 也可通过also create xib同时创建xib和cell类,并且它们是已经绑定好的

```objectivec
XMGCell.m
#import "XMGTgCell.h"
#import "XMGTg.h"

@interface XMGTgCell ()
@property (weak, nonatomic) IBOutlet UIImageView *iconImageView;
@property (weak, nonatomic) IBOutlet UILabel *titleLabel;
@property (weak, nonatomic) IBOutlet UILabel *priceLabel;
@property (weak, nonatomic) IBOutlet UILabel *buyCountLabel;

@end
@implementation XMGTgCell

- (void)setTg:(XMGTg *)tg
{
    _tg  = tg;
    self.iconImageView.image = [UIImage imageNamed:tg.icon];
    self.titleLabel.text = tg.title;
    self.priceLabel.text = [NSString stringWithFormat:@"￥%@",tg.price];
    self.buyCountLabel.text = [NSString stringWithFormat:@"%@人已购买",tg.buyCount];
}
@end
```

* **注意 : **为了达到循环利用的效果,需绑定cell标识,可通过以下两种方式
 1. 在xib中绑定indentifier
 2. 通过注册方式加载的时候就绑定
    ```objectivec
    //全局变量
    NSString *ID = @"tg";
    //viewDidLoad
    [self.tableView registerNib:[UINib nibWithNibName:NSStringFromClass([XMGTgCell class]) bundle:nil] forCellReuseIdentifier:ID];
    // cellForRowAtIndexPath
    XMGTgCell *cell = [tableView dequeueReusableCellWithIdentifier:ID];
    ```
    
* 通过xib创建cell默认高度是44,因为cell的高度是由tableView决定的,所以在xib设置高度是没有效果的,只需在控制器中设置cell高度即可
```objectivec
    self.tableView.rowHeight = 70;
```
    
# xib中不同cell共存
* 注册和绑定不同标识即可
![](/0108/images/WX20170730-101223.png)

# storyboard 自定义等高cell
* storyboard自定义cell使用代码注册没有相应添加标识的方法
**代码主动注册方式优先级>storyboard给定标识**,所以使用代码注册会报错
* 用storyboard方式自定义cell不用注册,直接在inspector设置标识即可
* 在storyboard中拖入cell当成xib来设置控件 位置 尺寸 约束 标识 即可
* 不同cell共存也是同样滴套路..

## storyboard中不同cell的共存
* cell分为动态cell(dynamic prototypes)和静态cell(static cells)
* 动态cell设置prototype cells数目就可以在storyboard中添加cell(也可以直接拖,属性会自动增加),再根据标识显示不同cell即可.

# 自定义分割线
* 设置separator style为none,往cell中增加view即可
* 最小约束高度为1,设置透明度,视觉效果上小了一点.

# 静态cell
* 主要用来表示app的设置界面
* 静态cell的cell内容是确定的,主要通过section(组)来确定界面




