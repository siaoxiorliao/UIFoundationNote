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
* 在xib中设置 位置 尺寸 约束 等
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


# 不同cell共存
* 注册和绑定不同标识即可
![](/0108/images/WX20170730-101223.png)


