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

## 编辑模式
```objectivec
- (IBAction)remove {
    // 进入编辑模式
    //取反
//    self.tableView.editing = !self.tableView.isEditing;
    [self.tableView setEditing:!self.tableView.isEditing animated:YES];
}
```

# 批量删除
```objectivec
- (void)viewDidLoad {
    [super viewDidLoad];
//    self.tableView.allowsMultipleSelection = YES;
    // 告诉tableView在编辑模式下可以多选
    self.tableView.allowsMultipleSelectionDuringEditing = YES;
    self.deletedButton.hidden = YES;
}
#pragma mark - 按钮的点击
- (IBAction)MultipleRemove {
    // 进入编辑模式
    [self.tableView setEditing:!self.tableView.isEditing animated:YES];
    self.deletedButton.hidden = !self.tableView.isEditing;
}
- (IBAction)remove {
    // 千万不要一边遍历一边删除,因为每删除一个元素,其他元素的索引可能会发生变化
    NSMutableArray *deletedWine = [NSMutableArray array];
    for (NSIndexPath *indexPath in self.tableView.indexPathsForSelectedRows) {
        [deletedWine addObject:self.wineArray[indexPath.row]];
    }
    // 修改模型
    [self.wineArray removeObjectsInArray:deletedWine];
    // 刷新表格
//    [self.tableView reloadData];
    [self.tableView deleteRowsAtIndexPaths:self.tableView.indexPathsForSelectedRows withRowAnimation:UITableViewRowAnimationAutomatic];
}
```

## 自定义批量删除
* 在contentView中添加imageView,根据用户选择状态设置隐藏与否即可;  
**160111-17-13**
* 在模型中保存选中状态 checked
* 在控制器中设置选中的索引数组,在didSelectRowIndexpath中更新选中和没选中的索引,删除的时候根据选中的索引数组删除对应模型和cell即可....


