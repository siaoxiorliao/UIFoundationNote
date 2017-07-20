# UIButton

## 按钮方法

* 文字

* 文字颜色

* - \(void\)setImage:\(UIImage \*\)image forState:\(UIControlState\)state; 设置按钮内容图片
* - \(void\)setBackgroundImage:\(UIImage \*\)image forState:\(UIControlState\)state; 设置按钮的背景图片

## 按钮状态

* UIControlStateNormal  normal（普通状态） 
* UIControlStateHighlighted  highlighted（高亮状态）按下时没有放开的状态
* UIControlStateDisabled  disabled（失效状态\)
* UIControlStateSelected selected\(选中状态\)

### test ruby
```ruby
//
//  ViewController.m
//  07-拳皇动画(加载图片)
//
//  Created by xiaomage on 15/12/26.
//  Copyright © 2015年 小码哥. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;

/** 站立 */
@property (nonatomic, strong) NSArray *standImages;
/** 小招 */
@property (nonatomic, strong) NSArray *smallImages;
/** 大招 */
@property (nonatomic, strong) NSArray *bigImages;
@property (nonatomic,strong) AVPlayer *player;

@end

@implementation ViewController

/**
    图片的两种加载方式:
    1> imageNamed:
      a. 就算指向它的指针被销毁,该资源也不会被从内存中干掉
      b. 放到Assets.xcassets的图片,默认就有缓存
      c. 图片经常被使用
 
    2> imageWithContentsOfFile:
      a. 指向它的指针被销毁,该资源会被从内存中干掉
      b. 放到项目中的图片就不由缓存
      c. 不经常用,大批量的图片

 */

// 初始化一些数据
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 1.加载所有的站立图片
    self.standImages = [self loadAllImagesWithimagePrefix:@"stand" count:10];
    
    // 2.加载所有的小招图片
    self.smallImages = [self loadAllImagesWithimagePrefix:@"xiaozhao3" count:39];
    
    // 3.加载所有的大招图片
    self.bigImages = [self loadAllImagesWithimagePrefix:@"dazhao" count:87];
    
    // 4.站立
    [self stand];

    AVPlayer *player = [[AVPlayer alloc]init];
    self.player = player;
}

/**
 *  加载所有的图片
 *
 *  @param imagePrefix 名称前缀
 *  @param count       图片的总个数
 */
- (NSArray *)loadAllImagesWithimagePrefix:(NSString *)imagePrefix count:(int)count{
    NSMutableArray<UIImage *> *images = [NSMutableArray array];
    for (int i=0; i<count; i++) {
        // 获取所有图片的名称
        NSString *imageName = [NSString stringWithFormat:@"%@_%d",imagePrefix, i+1];
        // 创建UIImage
//        UIImage *image = [UIImage imageNamed:imageName];
        NSString *imagePath = [[NSBundle mainBundle] pathForResource:imageName ofType:@"png"];
//        NSLog(@"%@",imagePath);
        UIImage *image = [UIImage imageWithContentsOfFile:imagePath];
        // 装入数组
        [images addObject:image];
    }
    return images;
}

/**
 *  站立
 */
- (IBAction)stand {
    /*
    // 2.设置动画图片
    self.imageView.animationImages = self.standImages;
    
    // 3.设置播放次数
    self.imageView.animationRepeatCount = 0;
    
    // 4.设置播放的时长
    self.imageView.animationDuration = 0.6;
    
    // 5.播放
    [self.imageView startAnimating];
    */
    [self palyZhaoWithImages:self.standImages count:0 duration:0.6 isStand:YES zhaoName:nil];
}

/**
 *  大招
 */
- (IBAction)bigZhao {
    /*
    // 2.设置动画图片
    self.imageView.animationImages = self.bigImages;
    
    // 3.设置动画次数
    self.imageView.animationRepeatCount = 1;
    
    // 4.设置播放时长
    self.imageView.animationDuration = 2.5;
    
    // 5.播放
    [self.imageView startAnimating];
    
    // 6.站立
    [self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
    */
    [self palyZhaoWithImages:self.bigImages count:1 duration:4.5 isStand:NO zhaoName:@"bigzhao"];
}

/**
 *  游戏结束
 */
- (IBAction)gameOver {
    self.standImages = nil;
    self.smallImages = nil;
    self.bigImages = nil;
    
    self.imageView.animationImages = nil;

}


/**
 *  放招
 *
 *  @param images   图片数组
 *  @param count    播放次数
 *  @param duration 播放时间
 *  @param isStand  是否站立
 */
- (void)palyZhaoWithImages:(NSArray *)images count: (NSInteger)count duration:(NSTimeInterval)duration isStand:(BOOL)isStand zhaoName:(NSString *) zhaoName{
    // 1.设置动画图片
    self.imageView.animationImages = images;
    
    // 2.设置动画次数
    self.imageView.animationRepeatCount = count;
    
    // 3.设置播放时长
    self.imageView.animationDuration = duration;
    
    // 4.播放
    [self.imageView startAnimating];
    
    NSString *musicName = nil;
    // 5.站立
    if (!isStand) {
        if ([zhaoName  isEqual: @"xiaozhao"]){
            musicName = @"xiaozhao1.mp3";
        }
        else{
            musicName = @"dazhao.mp3";
        }
        NSURL *url = [[NSBundle mainBundle] URLForResource:musicName withExtension:nil];
        AVPlayerItem *zhaoMusicName = [[AVPlayerItem alloc]initWithURL:url];
        [self.player replaceCurrentItemWithPlayerItem:zhaoMusicName];
        [self.player play];
     [self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
    }
}

@end
```


## test objectivec
```objectivec
//
// ViewController.m
// 07-拳皇动画(加载图片)
//
// Created by xiaomage on 15/12/26.
// Copyright © 2015年 小码哥. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;

/** 站立 */
@property (nonatomic, strong) NSArray *standImages;
/** 小招 */
@property (nonatomic, strong) NSArray *smallImages;
/** 大招 */
@property (nonatomic, strong) NSArray *bigImages;
@property (nonatomic,strong) AVPlayer *player;

@end

@implementation ViewController

/**
图片的两种加载方式:
1> imageNamed:
a. 就算指向它的指针被销毁,该资源也不会被从内存中干掉
b. 放到Assets.xcassets的图片,默认就有缓存
c. 图片经常被使用
2> imageWithContentsOfFile:
a. 指向它的指针被销毁,该资源会被从内存中干掉
b. 放到项目中的图片就不由缓存
c. 不经常用,大批量的图片

*/

// 初始化一些数据
- (void)viewDidLoad {
[super viewDidLoad];
// 1.加载所有的站立图片
self.standImages = [self loadAllImagesWithimagePrefix:@"stand" count:10];
// 2.加载所有的小招图片
self.smallImages = [self loadAllImagesWithimagePrefix:@"xiaozhao3" count:39];
// 3.加载所有的大招图片
self.bigImages = [self loadAllImagesWithimagePrefix:@"dazhao" count:87];
// 4.站立
[self stand];

AVPlayer *player = [[AVPlayer alloc]init];
self.player = player;
}

/**
* 加载所有的图片
*
* @param imagePrefix 名称前缀
* @param count 图片的总个数
*/
- (NSArray *)loadAllImagesWithimagePrefix:(NSString *)imagePrefix count:(int)count{
NSMutableArray<UIImage *> *images = [NSMutableArray array];
for (int i=0; i<count; i++) {
// 获取所有图片的名称
NSString *imageName = [NSString stringWithFormat:@"%@_%d",imagePrefix, i+1];
// 创建UIImage
// UIImage *image = [UIImage imageNamed:imageName];
NSString *imagePath = [[NSBundle mainBundle] pathForResource:imageName ofType:@"png"];
// NSLog(@"%@",imagePath);
UIImage *image = [UIImage imageWithContentsOfFile:imagePath];
// 装入数组
[images addObject:image];
}
return images;
}

/**
* 站立
*/
- (IBAction)stand {
/*
// 2.设置动画图片
self.imageView.animationImages = self.standImages;
// 3.设置播放次数
self.imageView.animationRepeatCount = 0;
// 4.设置播放的时长
self.imageView.animationDuration = 0.6;
// 5.播放
[self.imageView startAnimating];
*/
[self palyZhaoWithImages:self.standImages count:0 duration:0.6 isStand:YES zhaoName:nil];
}

/**
* 大招
*/
- (IBAction)bigZhao {
/*
// 2.设置动画图片
self.imageView.animationImages = self.bigImages;
// 3.设置动画次数
self.imageView.animationRepeatCount = 1;
// 4.设置播放时长
self.imageView.animationDuration = 2.5;
// 5.播放
[self.imageView startAnimating];
// 6.站立
[self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
*/
[self palyZhaoWithImages:self.bigImages count:1 duration:4.5 isStand:NO zhaoName:@"bigzhao"];
}

/**
* 游戏结束
*/
- (IBAction)gameOver {
self.standImages = nil;
self.smallImages = nil;
self.bigImages = nil;
self.imageView.animationImages = nil;

}


/**
* 放招
*
* @param images 图片数组
* @param count 播放次数
* @param duration 播放时间
* @param isStand 是否站立
*/
- (void)palyZhaoWithImages:(NSArray *)images count: (NSInteger)count duration:(NSTimeInterval)duration isStand:(BOOL)isStand zhaoName:(NSString *) zhaoName{
// 1.设置动画图片
self.imageView.animationImages = images;
// 2.设置动画次数
self.imageView.animationRepeatCount = count;
// 3.设置播放时长
self.imageView.animationDuration = duration;
// 4.播放
[self.imageView startAnimating];
NSString *musicName = nil;
// 5.站立
if (!isStand) {
if ([zhaoName isEqual: @"xiaozhao"]){
musicName = @"xiaozhao1.mp3";
}
else{
musicName = @"dazhao.mp3";
}
NSURL *url = [[NSBundle mainBundle] URLForResource:musicName withExtension:nil];
AVPlayerItem *zhaoMusicName = [[AVPlayerItem alloc]initWithURL:url];
[self.player replaceCurrentItemWithPlayerItem:zhaoMusicName];
[self.player play];
[self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
}
}

@end
```


# test swift
```swift
//
// ViewController.m
// 07-拳皇动画(加载图片)
//
// Created by xiaomage on 15/12/26.
// Copyright © 2015年 小码哥. All rights reserved.
//

#import "ViewController.h"
#import <AVFoundation/AVFoundation.h>
@interface ViewController ()
@property (weak, nonatomic) IBOutlet UIImageView *imageView;

/** 站立 */
@property (nonatomic, strong) NSArray *standImages;
/** 小招 */
@property (nonatomic, strong) NSArray *smallImages;
/** 大招 */
@property (nonatomic, strong) NSArray *bigImages;
@property (nonatomic,strong) AVPlayer *player;

@end

@implementation ViewController

/**
图片的两种加载方式:
1> imageNamed:
a. 就算指向它的指针被销毁,该资源也不会被从内存中干掉
b. 放到Assets.xcassets的图片,默认就有缓存
c. 图片经常被使用
2> imageWithContentsOfFile:
a. 指向它的指针被销毁,该资源会被从内存中干掉
b. 放到项目中的图片就不由缓存
c. 不经常用,大批量的图片

*/

// 初始化一些数据
- (void)viewDidLoad {
[super viewDidLoad];
// 1.加载所有的站立图片
self.standImages = [self loadAllImagesWithimagePrefix:@"stand" count:10];
// 2.加载所有的小招图片
self.smallImages = [self loadAllImagesWithimagePrefix:@"xiaozhao3" count:39];
// 3.加载所有的大招图片
self.bigImages = [self loadAllImagesWithimagePrefix:@"dazhao" count:87];
// 4.站立
[self stand];

AVPlayer *player = [[AVPlayer alloc]init];
self.player = player;
}

/**
* 加载所有的图片
*
* @param imagePrefix 名称前缀
* @param count 图片的总个数
*/
- (NSArray *)loadAllImagesWithimagePrefix:(NSString *)imagePrefix count:(int)count{
NSMutableArray<UIImage *> *images = [NSMutableArray array];
for (int i=0; i<count; i++) {
// 获取所有图片的名称
NSString *imageName = [NSString stringWithFormat:@"%@_%d",imagePrefix, i+1];
// 创建UIImage
// UIImage *image = [UIImage imageNamed:imageName];
NSString *imagePath = [[NSBundle mainBundle] pathForResource:imageName ofType:@"png"];
// NSLog(@"%@",imagePath);
UIImage *image = [UIImage imageWithContentsOfFile:imagePath];
// 装入数组
[images addObject:image];
}
return images;
}

/**
* 站立
*/
- (IBAction)stand {
/*
// 2.设置动画图片
self.imageView.animationImages = self.standImages;
// 3.设置播放次数
self.imageView.animationRepeatCount = 0;
// 4.设置播放的时长
self.imageView.animationDuration = 0.6;
// 5.播放
[self.imageView startAnimating];
*/
[self palyZhaoWithImages:self.standImages count:0 duration:0.6 isStand:YES zhaoName:nil];
}

/**
* 大招
*/
- (IBAction)bigZhao {
/*
// 2.设置动画图片
self.imageView.animationImages = self.bigImages;
// 3.设置动画次数
self.imageView.animationRepeatCount = 1;
// 4.设置播放时长
self.imageView.animationDuration = 2.5;
// 5.播放
[self.imageView startAnimating];
// 6.站立
[self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
*/
[self palyZhaoWithImages:self.bigImages count:1 duration:4.5 isStand:NO zhaoName:@"bigzhao"];
}

/**
* 游戏结束
*/
- (IBAction)gameOver {
self.standImages = nil;
self.smallImages = nil;
self.bigImages = nil;
self.imageView.animationImages = nil;

}


/**
* 放招
*
* @param images 图片数组
* @param count 播放次数
* @param duration 播放时间
* @param isStand 是否站立
*/
- (void)palyZhaoWithImages:(NSArray *)images count: (NSInteger)count duration:(NSTimeInterval)duration isStand:(BOOL)isStand zhaoName:(NSString *) zhaoName{
// 1.设置动画图片
self.imageView.animationImages = images;
// 2.设置动画次数
self.imageView.animationRepeatCount = count;
// 3.设置播放时长
self.imageView.animationDuration = duration;
// 4.播放
[self.imageView startAnimating];
NSString *musicName = nil;
// 5.站立
if (!isStand) {
if ([zhaoName isEqual: @"xiaozhao"]){
musicName = @"xiaozhao1.mp3";
}
else{
musicName = @"dazhao.mp3";
}
NSURL *url = [[NSBundle mainBundle] URLForResource:musicName withExtension:nil];
AVPlayerItem *zhaoMusicName = [[AVPlayerItem alloc]initWithURL:url];
[self.player replaceCurrentItemWithPlayerItem:zhaoMusicName];
[self.player play];
[self performSelector:@selector(stand) withObject:nil afterDelay:self.imageView.animationDuration];
}
}

@end
```








