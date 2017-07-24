# KVC和KVO

## KVC - Key Value Coding 键值编码
* 是一种可以直接通过字符串key来访问或修改对象属性的机制。

### KVC赋值

1.KVC可以进行自动类型转换
```objectivec
     // KVC赋值
    [person setValue:@"王五" forKey:@"name"];
    [person setValue:@"19" forKeyPath:@"money"]; // 自动类型转换
    NSLog(@"%@-----%.2f", person.name, person.money);//王五-----19.00
```

2.KVC中的forKey和forKeyPath

     1> forKeyPath 包含了所有 forKey 的功能
     2> forKeyPath 可以进行内部的点语法,层层访问内部的属性
     3> 注意: key值一定要在属性中找到
     
```objectivec
    [person.dog setValue:@"阿黄" forKey:@"name"];
    [person setValue:@"旺财" forKeyPath:@"dog.name"];
    
    NSLog(@"%@", person.dog.name);
```

3.**可以用KVC来修改一个类私有的成员变量**(UIPageControl)

```objectivec
    @implementation XMGPerson
    {
    int _age;
    }

    - (void)printAge{
    NSLog(@"age:%d", _age);
    }
@end
```
修改:
```objectivec
    [person printAge];//0
    [person setValue:@"88" forKeyPath:@"_age"]; // age _age都可以
    [person printAge];//88
```
4.**使用KVC快速字典转模型(重要)**

```objectivec
- (instancetype)initWithDict:(NSDictionary *)dict{
    if (self = [super init]) {
        /*
        self.name = dict[@"name"];
        self.money = [dict[@"money"] floatValue];
         */
        [self setValuesForKeysWithDictionary:dict];
    }
    return self;
}
```

* **但是这些情况不能使用**

```objectivec
/**
 开发中是不建议使用setValuesForKeysWithDictionary:
 1> 字典中的key必须在模型的属性中找到
 2> 如果模型中带有模型,setValuesForKeysWithDictionary不好使
 3> 模型中有数组,也不好使
 应用场景: 简单的字典转模型(单层) ---> **有一个框架插件 (MJExtention,无论什么数据都可以转)**
 */
    NSDictionary *dict = @{
                           @"name" :@"lurry",
                           @"money" : @189.88,
                           @"dog" : @{//模型中有模型
                                   @"name" : @"wangcai",
                                   @"price" : @8
                                   },
                           @"books": @[//模型中有数组
                                   @{@"name" :@"iOS快速开发", @"price" : @1999},
                                   @{@"name" :@"iOS快速发", @"price" : @199},
                                   @{@"name" :@"iOS快开发", @"price" : @99}
                                   ]
                           };
    XMGPerson *person = [[XMGPerson alloc] initWithDict:dict];
    NSLog(@"%@", person.dog);//等于下面
    /*
    [person setValue: @{
                        @"name" : @"wangcai",
                        @"price" : @8
                        } forKeyPath:@"dog"];
     */
```

5.模型转字典

```objectivec     
XMGPerson *person = [[XMGPerson alloc] init];
    person.name = @"lurry";
    person.money = 21.21;
    
    NSDictionary *dict = [person dictionaryWithValuesForKeys:@[@"name", @"money"]];
    NSLog(@"%@", dict);
```
6.取出数组中所有模型的某个属性值

```objectivec
    XMGPerson *person1 = [[XMGPerson alloc] init];
    person1.name = @"zhangsan";
    person1.money = 12.99;
        
    XMGPerson *person2 = [[XMGPerson alloc] init];
    person2.name = @"zhangsi";
    person2.money = 22.99;
        
    XMGPerson *person3 = [[XMGPerson alloc] init];
    person3.name = @"wangwu";
    person3.money = 122.99;
        
    NSArray *allPersons = @[person1, person2, person3];
    NSArray *allPersonName = [allPersons valueForKeyPath:@"name"];
    NSLog(@"%@", allPersonName);
```
### KVC取值
```objectivec
    XMGPerson *person = [[XMGPerson alloc] init];
    person.name = @"张三";
    person.money = 12332;
    NSLog(@"%@ --- %.2f", [person valueForKeyPath:@"name"], [[person valueForKey:@"money"] floatValue]);
```
## KVO - Key Value Observing 键值观察

* 是一种回调机制，在某个对象注册监听者后，当被监听的对象发生改变时，对象会发送一个通知给监听者，以便监听者执行回调操作。

```objectivec

#import "ViewController.h"
#import "XMGPerson.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    XMGPerson *person = [[XMGPerson alloc] init];
    person.name = @"zs";
    person.age = 5;
    [person addObserver:self forKeyPath:@"name" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:@"你好啊"];
    [person addObserver:self forKeyPath:@"age" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:@"你好"];
    person.name = @"ls";
    person.age = 10;
    person.name = @"ww";
     person.age = 20;
    // 移除监听
    [person removeObserver:self forKeyPath:@"name"];
     [person removeObserver:self forKeyPath:@"age"];
}

- (void)observeValueForKeyPath:(NSString *)keyPath 
ofObject:(id)object
change:(NSDictionary<NSString *,id> *)change
context:(void *)context{
    NSLog(@"%@------%@------%@", keyPath, object, change);
    /*
    name------<XMGPerson: 0x60000002ff80>------{
    kind = 1;
    new = ls;
    old = zs;
}-----你好啊
    age------<XMGPerson: 0x60000002ff80>------{
    kind = 1;
    new = 10;
    old = 5;
}-----你好
    name------<XMGPerson: 0x60000002ff80>------{
    kind = 1;
    new = ww;
    old = ls;
}-----你好啊
    age------<XMGPerson: 0x60000002ff80>------{
    kind = 1;
    new = 20;
    old = 10;
}-----你好
    */
    
}
@end

```


## KVO - Key Value Observing 键值观察