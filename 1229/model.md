# 模型
模型即是数据模型,专门用来存储数据的对象

## 用模型取代字典的好处

* 使用字典的坏处

 1. 使用字典,手敲key值,没有提示
 2. key如果写错了,编译器不会有任何警告和报错,难找错误
 ```objectivec
dict[@"name"] = @"jack";
NSString *name = dict[@"name"];
```

* 使用模型的好处

 1. 更加专业
 2. 使用模型读和取数据都是通过访问属性,写的时候会有提示,写错编译器即会报错.
 ```objectivec
 app.name = @"jack";
 NSString *name = app.name;
 ```
 
## 字典转模型 原理

1.创建模型类
![](/1229/images/WX20170722-145243.png)

2.字典转模型,变成模型数组

导入头文件 #import "goodsModel.h"

![](/1229/images/WX20170722-145307.png)

3.使用模型

![](/1229/images/WX20170722-145320.png)

## 快速转模型(提供快速构造方法)
```objectivec
- (instancetype) initWithIcon: (NSString *)icon name: (NSString *)name {
    if (self = [super init]){
        self.icon = icon;
        self.name = name;
    }
    return self;
}

+ (instancetype) goodsModelWithIcon: (NSString *)icon name:(NSString *)name{
    return [[self alloc]initWithIcon:icon name:name];
}
```
* 这样没有保证封装性:

```objectivec
XMGShop *shop = [[XMGShop alloc] initWithIcon:dict[@"icon"] name:dict[@"name"]];
XMGShop *shop = [XMGShop shopWithIcon:dict[@"icon"] name:dict[@"name"]];
shop.name = dict[@"name"];
shop.icon = dict[@"icon"];
```
* 应该这样 :

```objectivec
- (instancetype)initWithDict:(NSDictionary *)dict{
    if (self = [super init]) {
        self.icon = dict[@"icon"];
        self.name = dict[@"name"];
    }
    return self;
}

+ (instancetype)shopWithDict:(NSDictionary *)dict{
    return [[self alloc] initWithDict:dict];
}
```

* 保证了封装性
```objectivec
XMGShop *shop = [XMGShop shopWithDict:dict];
```

### 类前缀
区分框架等...
NS即是Foundation框架里的
UI即是UIKit框架里的

在项目开发中有时需要类前缀,用来与系统的类区分或分工功能的区分
* ZSShop
* LSShop
* WWShop

更改类的前缀
![](/1229/images/WX20170722-150340.png)





