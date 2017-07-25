# UIScrollView 滚动视图
* 继承自UIView

##scrollView常见属性
contentSize 内容大小
**contenOffset** 内容的偏移量(滚动的位置)
> * 如果设置负的 触摸一下即回到(0,0),可以用来做好的效果
> * 可以控制滚动位置

> ```objectivec
- (IBAction)bottom {
CGFloat offsetX = self.scrollView.contentOffset.x;
CGFloat offsetY = self.scrollView.contentSize.height - self.scrollView.frame.size.height;
CGPoint offset = CGPointMake(offsetX, offsetY);
[self.scrollView setContentOffset:offset animated:YES];
}
```

**contentInset** 在四周增加的内边距,和contentOffset无关系(用户滑动查看歌词)

scrollEnable 是否可滚动 默认YES
bounces  是否有弹性 默认YES
showHorizontalScrollIndicatior 是否显示水平滚动条 默认YES
showVerticalScrollIndicaior 是否显示竖直滚动条 默认YES
alwaysBounceHorizontal 是否水平滚动 默认NO,如果bounces是YES,也可以水平滚动
alwaysBounceVertical 是否竖直滚动 默认NO,如果bounces是YES,也可以竖直滚动

![](/0104/images/WX20170725-140222.png)

### scrollView获取子控件注意
* 刚开始获取subviews是都拿不到的(数组为nil),当添加了子控件即可拿到子控件,当viewDidAppear视图显示完毕后,会添加滚动条imageView到数组后两位;
* 设置contentSize后滚动条也会改变大小,所以滚动条会排到后面.

> 点击控制器的view自动调用方法
```objectivec
- (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:    (UIEvent *)event
    {
    //    self.scrollView.contentOffset = CGPointMake(-100,         -100);
    }
```

### 不能滚动可能的原因
* contentSize小于或等于scrollView的的尺寸
* scrollView.scrollEnable = NO;//仅让scrollView不能滚动
* scrollView.userInteractionEnabled = NO;//不响应用户所有操作
* ......

## scrollView代理 Delegate
* 要想监听UIScrollView的行为，就必须先给UIScrollView设置一个代理对象，通过代理得知UIScrollView的行为,然后做相应的动作.
遵守协议最好不要写在.h中,可写在类扩展中
![](/0104/images/WX20170725-143712.png)


* 代理方法有很多

```objectivec
//用户已经停止拖拽scrollView时就会调用这个方法
- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView willDecelerate:(BOOL)decelerate
{
    if (decelerate == NO) {
        NSLog(@"用户已经停止拖拽scrollView,停止滚动");
    } else {
        NSLog(@"用户已经停止拖拽scrollView,拖太大力,scrollView由于惯性会继续滚动并且减速");
    }
}

//scrollView减速完毕会调用,停止滚动
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
{
       NSLog(@"scrollView减速完毕会调用,停止滚动");
}
// 等等
```


