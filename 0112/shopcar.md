# 

### 增加圆角和边框
```objectivec
-(void)setUpBtn: (UIButton *)btn{
    btn.layer.borderWidth = 1.0;
    btn.layer.cornerRadius = 10;
    btn.layer.borderColor = [UIColor orangeColor].CGColor;
}
```

# 通知(NSNotification)
* 通知中心
* 监听通知
* 移除监听

## 系统随时都会发布通知
* UIDevie 设备通知 通过[UIDevice currentDevice]可以获取这个单粒对象
> 通过它可以获得一些设备相关的信息，比如电池电量值(batteryLevel)、电池状态(batteryState)、设备的类型(model，比如iPod、iPhone等)、设备的系统(systemVersion)

* 键盘通知 
> 经常需要在键盘弹出或者隐藏的时候做一些特定的操作,因此需要监听键盘的状态

# 通知和代理的选择
* 共同点: 利用通知和代理都能完成对象之间的通信(比如A对象告诉D对象发生了什么事情, A对象传递数据给D对象)
* 不同点: 代理 : 1个对象只能告诉另1个对象发生了什么事情
通知 : 1个对象能告诉N个对象发生了什么事情, 1个对象能得知N个对象发生了什么事情