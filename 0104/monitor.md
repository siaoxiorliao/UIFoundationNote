## 常见控件的监听
* 监听可通过delegate,addTarget或是手势,通知等等
* 只要(有)继承自UIControl的就(才)能通过addTarget方法去监听事件(控件继承关系1225)
```objectivec
    UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];
    [btn addTarget:self action:@selector(btnClick:) forControlEvents:UIControlEventTouchUpInside];//注意冒号
    
    //
    - (void)btnClick:(UIButton *)btn
    {
    
    }
```

## 退出键盘和textField
* iphone没有为键盘提供退出键盘的按钮,ipad有
* 退出键盘:
 ```objectivec
 - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event
{
    // 退出键盘
//    [self.textFlied endEditing:YES];
    // 辞去第一响应者(退出键盘),textField开始编辑即成为第一响应者
//    [self.textFlied resignFirstResponder];
        [self.view endEditing:YES];
}
 ```
 
* textField可以通过代理也可以通过addTarget监听事件

```objectivec
//addTarget
    [self.textFlied addTarget:self action:@selector(tfEditingDidBegin)forControlEvents:UIControlEventEditingDidBegin];
    [self.textFlied addTarget:self action:@selector(tfEditingDidEnd) forControlEvents:UIControlEventEditingDidEnd];
    [self.textFlied addTarget:self action:@selector(tfEditingChanged:) forControlEvents:UIControlEventEditingChanged];
//代理
- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string
{
    NSLog(@"shouldChangeCharactersInRange--%@",string);
    if ([string isEqualToString:@"1"]) {
        return NO;//禁止用户输入
    }
    return YES;//允许用户输入
}
//等等
```


