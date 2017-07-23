# iOS中的动画
* 帧动画 NSArray imageView.animationImages
* 渐变动画
* 核心动画 CoreAniamtion

## 帧动画
* 1226 - 拳皇实例

## 渐变动画

### 平移
```objectivec
// 方式一 首尾式
[UIView beginAnimations:nil context:nil];
[UIView setAnimationDuration:1.0];
CGRect frame = self.animationView.frame;
frame.origin.y -= 50;
self.animationView.frame = frame;
[UIView commitAnimations];
```
```objectivec
// 方式二 block式(常用)
    // 1.
    [UIView animateWithDuration:2.0 animations:^{
        CGRect frame = self.animationView.frame;
        frame.origin.y -= 50;
        self.animationView.frame = frame;
    }];
    
    // 2.
    [UIView animateWithDuration:1.0 animations:^{
        CGRect frame = self.animationView.frame;
        frame.origin.y -= 50;
        self.animationView.frame = frame;
    } completion:^(BOOL finished) {
        self.animationView.backgroundColor = [UIColor blackColor];
    }];
    
    // 3.
    /*
     UIViewAnimationOptionCurveEaseInOut  动画开始/结束比较缓慢,中间相对较快
     UIViewAnimationOptionCurveEaseIn     动画开始比较缓慢
     UIViewAnimationOptionCurveEaseOut    动画结束比较缓慢
     UIViewAnimationOptionCurveLinear     线性---> 匀速
     */
    [UIView animateWithDuration:1.0 delay:1.0 options:UIViewAnimationOptionCurveEaseInOut animations:^{
        CGRect frame = self.animationView.frame;
        frame.origin.y += 50;
        self.animationView.frame = frame;
        
    } completion:^(BOOL finished) {
        self.animationView.backgroundColor = [UIColor greenColor];
    }];
```
---

### 缩放
```objectivec
 [UIView animateWithDuration:1.0 delay:1.0 options:UIViewAnimationOptionCurveLinear animations:^{
        CGRect frame = self.animationView.frame;
        CGRect tempFrame = CGRectMake(frame.origin.x, frame.origin.y, frame.size.width * 0.9, frame.size.height * 0.9);
        self.animationView.frame = tempFrame;
    } completion:^(BOOL finished) {
        NSLog(@"动画完成");
    }];
```

### 透明度
```objectivec
 [UIView animateWithDuration:1.0 delay:0.5 options:UIViewAnimationOptionCurveEaseOut animations:^{
        self.animationView.alpha -= 0.9;
    } completion:^(BOOL finished) {
    //嵌套
       [UIView animateWithDuration:2.0 animations:^{
           self.animationView.alpha += 0.9;
       }];
    }];
```

## 核心动画



