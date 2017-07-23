# UIButton
---
## 按钮方法

* 文字

* 文字颜色
- \- (void)setImage:(UIImage *)image forState:(UIControlState)state; 设置按钮内容图片
- \- (void)setBackgroundImage:(UIImage *)image forState:(UIControlState)state; 设置按钮的背景图片

## 按钮状态
* UIControlStateNormal  normal（普通状态） 
* UIControlStateHighlighted  highlighted（高亮状态）按下时没有放开的状态
* UIControlStateDisabled  disabled（失效状态)
* UIControlStateSelected selected(选中状态)

> 在storyboard中设置按钮显示的是normal状态的按钮(可以设置状态查看状态情况)

## 示例代码

```objectivec
#import "ViewController.h"

@interface ViewController ()
@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];
    btn.frame = CGRectMake(100, 100, 170, 60);
    //    btn.backgroundColor = [UIColor orangeColor];
    
    /*
     一般属性只可读的情况有两种,要么要在初始化的时候设置,要么已经setter接口方法
     */
    
    //设置不同状态的文本图片等等
    [btn setTitle:@"你好啊" forState:UIControlStateNormal];
    [btn setTitle:@"Hello" forState:UIControlStateHighlighted];
    [btn setTitleColor:[UIColor greenColor] forState:UIControlStateNormal];
    [btn setTitleColor:[UIColor yellowColor] forState:UIControlStateHighlighted];
    
    [btn setTitleShadowColor:[UIColor blackColor] forState:UIControlStateNormal];
    [btn setReversesTitleShadowWhenHighlighted:YES];//阴影反向偏移
    btn.titleLabel.shadowOffset = CGSizeMake(5, 5);
    btn.imageView.backgroundColor = [UIColor purpleColor];//也可以这样 ,UIView -> UIControl -> UIButton, UIView -> UIImageView;
    [btn setImage:[UIImage imageNamed:@"player_btn_pause_normal"] forState:UIControlStateNormal];
    [btn setImage:[UIImage imageNamed:@"player_btn_pause_highlighted"] forState:UIControlStateHighlighted];
    [btn setBackgroundImage:[UIImage imageNamed:@"buttongreen"] forState:UIControlStateNormal];
    [btn setBackgroundImage:[UIImage imageNamed:@"buttongreen_highlighted"] forState:UIControlStateHighlighted];
    [self.view addSubview:btn];
    SEL sel = @selector(clickBtn:);
    [btn addTarget:self action:sel forControlEvents:UIControlEventTouchUpInside];

    
}
//IBAction可以为故事板连线也可以为代码连线,void只可以为代码连线
- (void)clickBtn:(UIButton *)sender{
    NSLog(@"你好啊");
}
@end
```



