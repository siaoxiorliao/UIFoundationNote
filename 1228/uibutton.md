# UIButton

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
    btn.imageView.backgroundColor = [UIColor purpleColor];
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













