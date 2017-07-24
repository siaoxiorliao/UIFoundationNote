# KVC和KVO

## KVC - Key Value Coding 键值编码
* 是一种可以直接通过字符串key来访问或修改对象属性的机制。

### KVC赋值

1. KVC可以进行自动类型转换
 
 ```objectivec
     // KVC赋值
    [person setValue:@"王五" forKey:@"name"];
    [person setValue:@"19" forKeyPath:@"money"]; // 自动类型转换
    NSLog(@"%@-----%.2f", person.name, person.money);//王五-----19.00
```
2. KVC中的forKey和forKeyPath
     1> forKeyPath 包含了所有 forKey 的功能
     2> forKeyPath 可以进行内部的点语法,层层访问内部的属性
     3> 注意: key值一定要在属性中找到
```objectivec
    [person.dog setValue:@"阿黄" forKey:@"name"];
    [person setValue:@"旺财" forKeyPath:@"dog.name"];
    
    NSLog(@"%@", person.dog.name);
```
3. **可以用KVC来修改一个类私有的成员变量**
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
    [person setValue:@"88" forKeyPath:@"_age"]; // age _age
    [person printAge];//88
```

## KVO - Key Value Observing 键值观察