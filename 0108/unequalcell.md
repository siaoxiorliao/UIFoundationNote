# 自定义不等高cell

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

* 因为控制器加载tableview先会调用heightForRowAtIndexPath方法确定cell的高度再加载cell(调用layoutsubviews方法),所以需要在return cellHeight之前确定cellHeight,因此,