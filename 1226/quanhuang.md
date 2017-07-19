<!-- toc -->
### 拳皇动画实例
1. 初始化,如果在点击执行后才加载图片,那其他地方要用的时候就也要加载图片,所以把加载图片的过程放在viewDidLoad里面,再用全局变量接收,只要加载一次图片,就可以在任何地方使用图片了.
2. 代码抽取:重复代码过多,代码之间区别很小,只有一点不同的地方,这时可以抽取代码,将不同的地方作为参数即可.
3. 大小招动画执行完后需要站立,但是直接用站立则覆盖了大小招动画,所以可以加入一个延迟方法,在点击按钮后先加载3秒大小招,延迟3秒执行站立的动画,就可以达到效果
```objectivec
[self performSelector:@selector(stand) withObject:nil afterDelay;self.imageView.animationDuration];
//<#(nonnull SEL)#>:@selector(stand) = NSSelectorFromString(stand) 方法
//<#(NSTimeInterval)#>: 间隔时间
```
4. 内存优化:图片加载完后内存大,不玩了,就游戏结束,释放内存
 (1) 释放数组
 ```objectivec
 self.standImages = nil;
 self.smaillImages = nil;
 self.bigImages = nil;
```
游戏结束后还是在站立(有站立的缓存)
加入 self.imageView.animationImages = nil;清除缓存即可
##### 播放音效

```objectivec
    #import <AVFoundation/AVFoundation.h>
    self.bgImageView.frame = self.view.bounds;//设置大小
    UIToolbar *tb = [[UIToolbar alloc]init];//毛玻璃凶过
    tb.frame = self.bgImageView.bounds;
    tb.barStyle = UIBarStyleBlack;
    tb.alpha = 0.95;
    [self.bgImageView addSubview:tb];
    NSString *path = [[NSBundle mainBundle] pathForResource:@"mySong2.mp3" ofType:nil];//取资源路径
    NSURL *url = [NSURL fileURLWithPath:path];//路径转url
    AVPlayerItem *playItem = [[AVPlayerItem alloc]initWithURL:url];//创建歌曲
    self.player = [[AVPlayer alloc]initWithPlayerItem:playItem];//创建 并 接收avplayer
    
```

