# 常见控件的监听

* 只要(有)继承自UIControl的就(才)能通过addTarget方法去监听事件(控件继承关系1225)
```objectivec
    UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];
    [btn addTarget:self action:@selector(btnClick:) forControlEvents:UIControlEventTouchUpInside];//注意冒号
    
    //
    - (void)btnClick:(UIButton *)btn
    {
    
    }
```

