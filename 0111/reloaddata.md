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

```objectivec
//- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath
//{
//    NSLog(@"commitEditingStyle--");
//    [self.wineArray removeObjectAtIndex:indexPath.row];
//    [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationTop];
//}

//
//- (NSString *)tableView:(UITableView *)tableView titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath
//{
    return @"删除";
//}

//使用此方法则上面的方法被覆盖
- (NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath
{
//    self.tableView.editing = YES;//让tableview进入编辑模式
    UITableViewRowAction *action = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"关注" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
//        [self.tableView reloadData];
//        [self.tableView reloadRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationRight];
        // 退出编辑模式
        self.tableView.editing = NO;
    }];
    
    UITableViewRowAction *action1 = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleDestructive title:@"删除" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
        
        [self.wineArray removeObjectAtIndex:indexPath.row];
        [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationAutomatic];
        
    }];
    return @[action1,action];
}
```