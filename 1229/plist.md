# 本地存储文件plist(属性列表文件)
## 为什么要用plist?
* 直接将数据直接写在代码里面，不是一种合理的做法。如果数据经常改，就要经常翻开对应的代码进行修改，造成代码扩展性低
* 局限性:一般只能存储NSArray和NSDictionary

## plist写入
* 手动写入 : 麻烦
* 代码写入
```objectivec
 NSArray *persons = @[
                             @{@"name" : @"mj", @"age":@38},
                             @{@"name" : @"yjh", @"age":@25, @"friends":@[@"大神11期", @"sz"]}
                             ];
        BOOL flag = [persons writeToFile:@"/Users/xiaomage/Desktop/persons.plist" atomically:YES];
        if (flag) {
            NSLog(@"写入成功!");
        }
```

## plist读取

### 读取注意
* 放入项目,记得加入资源包中
