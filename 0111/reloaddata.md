# 

# 数据刷新
* 全局刷新 [self.tableView reloadData]

```objectivec
    // 修改模型
    XMGWine *wine = [[XMGWine alloc] init];
    wine.image = @"newWine";
    wine.money = @"55.5";
    wine.name = @"女儿红";
    [self.wineArray insertObject:wine atIndex:0];
    [self.tableView reloadData];
```

* 局部刷新

```objectivec
    NSArray *indexPaths = @[
                            [NSIndexPath indexPathForRow:0 inSection:0]
                            ];
    [self.tableView insertRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationRight];
    //[self.tableView deleteRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationMiddle];
    //[self.tableView reloadRowsAtIndexPaths:indexPaths withRowAnimation:UITableViewRowAnimationLeft];
```
# 左滑删除