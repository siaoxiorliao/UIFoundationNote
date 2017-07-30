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
* 在xib中设置 位置尺寸约束等
* 只需在自定义的cell类中设置数据即可.

```objectivec

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
