# 自定义不等高cell
## 计算frame
**160108-16-15**
* UILabel大小随着字体大小而改变
```objectivec
    #define nameFont [UIFont systemFontOfSize:17]
    nameLabel.font = nameFont;
    
    CGFloat nameX = CGRectGetMaxX(self.iconImageView.frame) + space;
    CGFloat nameY = iconXY;
    //获取字体
    NSDictionary *nameAtt = @{NSFontAttributeName :nameFont};
    //通过字体设置nameLabel的大小
    CGSize nameSize = [self.status.name sizeWithAttributes:nameAtt];
    CGFloat nameW = nameSize.width;
    CGFloat nameH = nameSize.height;
    self.nameLabel.frame = CGRectMake(nameX, nameY, nameW, nameH);
```

* UILabel高度随着文本而改变
```objectivec
    #define textFont [UIFont systemFontOfSize:14]
    text_Label.font = textFont;
    
    CGFloat textX = iconXY;
    CGFloat textY = CGRectGetMaxY(self.iconImageView.frame) + space;
    CGFloat textW = self.contentView.frame.size.width - 2 * space;
    NSDictionary *textAtt = @{NSFontAttributeName :textFont};
//    CGSize textSize = [self.status.name sizeWithAttributes:textAtt];
//    CGFloat textH = textSize.height;
    //最大宽度和最大高度
    CGSize textSize = CGSizeMake(textW, MAXFLOAT);
//    CGFloat textH = [self.status.text sizeWithFont:textFont constrainedToSize:textSize].height;
    CGFloat textH = [self.status.text boundingRectWithSize:textSize options:NSStringDrawingUsesLineFragmentOrigin attributes:textAtt context:nil].size.height;
    self.text_Label.frame = CGRectMake(textX, textY, textW, textH);
```
## cellHeight返回方式

* 因为控制器加载tableview先会调用heightForRowAtIndexPath方法确定cell的高度再加载cell(调用layoutsubviews方法),所以需要在return cellHeight之前确定cellHeight,正常来说可以在return cellHeight之前再次计算cellHeight可以达到不等高cell的效果,但是这里重复代码过多并且重新计算了,每次重用或layoutsubviews都会再次计算**160108-17-15**
因此需要改变cellHeight加载方式 :     **160111-01-24**
 * frame只依赖于模型
 * cellHeight只依赖于模型
 * 将frame和cellHeight加入模型中
 * 因为每单个模型cellHeight只需要在第一次使用才计算,重用后只需访问之前已经计算好的即可,这里就使用懒加载思路,cellHeight如果等于0表示之前没有计算过,重新计算.cellHeight不等于0,说明已经计算过,直接返回之前计算了的即可.
 
**代码**
```objectivec
#import <UIKit/UIKit.h>
@interface XMGStatus : NSObject
@property (nonatomic, copy) NSString *icon;
@property (nonatomic, copy) NSString *name;
@property (nonatomic, copy) NSString *text;
@property (nonatomic, assign, getter=isVip) BOOL vip;
@property (nonatomic, copy) NSString *picture;
@property (nonatomic, assign) CGRect iconFrame;
@property (nonatomic, assign) CGRect nameFrame;
@property (nonatomic, assign) CGRect vipFrame;
@property (nonatomic, assign) CGRect textFrame;
@property (nonatomic, assign) CGRect pictureFrame;
@property (nonatomic, assign) CGFloat cellHeight;
@end
```

```objectivec
#import "XMGStatus.h"
@implementation XMGStatus
- (CGFloat)cellHeight
{
    if (_cellHeight == 0) {
        CGFloat space = 10;
        CGFloat iconX = space;
        CGFloat iconY = space;
        CGFloat iconWH = 30;
        self.iconFrame = CGRectMake(iconX, iconY, iconWH, iconWH);
        CGFloat nameX = CGRectGetMaxX(self.iconFrame) + space;
        CGFloat nameY = iconY;
        NSDictionary *nameAtt = @{NSFontAttributeName : [UIFont systemFontOfSize:17]};
        CGSize nameSize = [self.name sizeWithAttributes:nameAtt];
        CGFloat nameW = nameSize.width;
        CGFloat nameH = nameSize.height;
        self.nameFrame = CGRectMake(nameX, nameY, nameW, nameH);
        if (self.isVip) {
            CGFloat vipX = CGRectGetMaxX(self.nameFrame) + space;
            CGFloat vipW = 14;
            CGFloat vipH = nameH;
            CGFloat vipY = nameY;
            self.vipFrame = CGRectMake(vipX, vipY, vipW, vipH);
        }
        CGFloat textX = iconX;
        CGFloat textY = CGRectGetMaxY(self.iconFrame) + space;
        CGFloat textW = [UIScreen mainScreen].bounds.size.width - 2 * space;
        NSDictionary *textAtt = @{NSFontAttributeName : [UIFont systemFontOfSize:14]};
        CGSize textSize = CGSizeMake(textW, MAXFLOAT);
        CGFloat textH = [self.text boundingRectWithSize:textSize options:NSStringDrawingUsesLineFragmentOrigin attributes:textAtt context:nil].size.height;
        self.textFrame = CGRectMake(textX, textY, textW, textH);
        if (self.picture) {
            CGFloat pictureWH = 100;
            CGFloat pictureX = iconX;
            CGFloat pictureY = CGRectGetMaxY(self.textFrame) + space;
            self.pictureFrame = CGRectMake(pictureX, pictureY, pictureWH, pictureWH);
            _cellHeight = CGRectGetMaxY(self.pictureFrame) + space;
        } else {
            _cellHeight = CGRectGetMaxY(self.textFrame) + space;
        }
    }
    return _cellHeight;
}
@end
```
 
 
 

