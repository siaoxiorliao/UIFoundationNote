
## 简单MVC思想
```objectivec
* 虽然困,还是要微笑面对...

* emoji🙄
```
---
 |character|word|解释
 |:----:| :----:| :----: |
 |M |model |数据模型
 |V |View |显示view和数据
 |C |controller |控制view和数据
 
## xml和json
* xml代码比较冗余复杂
* json数据简单(大部分都是用json)
* 大公司有其他的...不知道是啥哟

## xib和storyboard
* xib是轻量级的,用来描述界面的局部UI
* storyboard是重量级的描述整个app的多个界面,并且展示界面之间跳转关系
* 共同点:(1)都用来描述界面(2)都使用interface builder工具编辑
(3)本质都是通过代码来创建控件.

## xib基本使用

### xib加载
```objectivec
    // 方式一
    UIView *carView = [[[NSBundle mainBundle] loadNibNamed:@"CarView" owner:nil options:nil] firstObject];
    carView.frame = CGRectMake(0, 100, 200, 50);
//    carView.clipsToBounds = YES;
    [self.view addSubview:carView];
    // 方式二
//    UINib *nib = [UINib nibWithNibName:@"CarView" bundle:nil];
//    UIView *carView = [[nib instantiateWithOwner:nil options:nil] firstObject];
//    [self.view addSubview:carView];
```

### 使用xib自定义控件
* 直接设置class为自定义的那个类,加载使用即可
> 使用xib只能使用👆加载的加载方法.

### 为xib提供快速创建方法
* 如果经常需要经常使用xib,可为xib提供快速创建方法

```objectivec
+ (instancetype)CarView{
    return [[[NSBundle mainBundle] loadNibNamed:@"CarView" owner:nil options:nil] firstObject];
}
```
### xib使用注意
* 想用初始化方法init 和 initWithFrame通过**代码**来添加子控件是不可行的(不会调用它们).
* 但是可以使用initWithCoder(Coder代码)和awakeFromNib(唤醒),来添加子控件

```objectivec
- (instancetype)initWithCoder:(NSCoder *)aDecoder{
    if (self = [super initWithCoder:aDecoder]) {
        UILabel *label = [[UILabel alloc] init];
        label.backgroundColor = [UIColor grayColor];
        label.text = @"哈哈哈哈哈哈";
        [self addSubview:label];
        self.label = label;
        NSLog(@"1");
        
    }
    return self;
}
```
#### 从xib创建的子控件添加子控件
* 如果子控件是从**xib**中创建,是处于未唤醒状态(想要从xib创建的子控件中添加子控件(使用从xib创建的子控件)是不可行的),这时可以通过awakeFromNib中来使用从xib创建的子控件

```objectivec
- (instancetype)initWithCoder:(NSCoder *)aDecoder{
    if (self = [super initWithCoder:aDecoder]) {
        // 往imageView上加毛玻璃//不可行
        UIToolbar *toolBar = [[UIToolbar alloc] init];
        [self.iconView addSubview:toolBar];
        self.toolBar = toolBar;  
    }
    return self;
}
```

```objectivec
- (void)awakeFromNib{
    // 往imageView上加毛玻璃(从xib创建的子控件中添加子控件)//可行
    UIToolbar *toolBar = [[UIToolbar alloc] init];
    [self.iconView addSubview:toolBar];
    self.toolBar = toolBar;
}
```





