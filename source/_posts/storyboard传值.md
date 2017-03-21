---
title: storyboard传值
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
#pragma mark - Navigation

// In a storyboard-based application, you will often want to do a little preparation before navigation
- (void)prepareForSegue:(UIStoryboardSegue *)segue sender:(id)sender {
    // Get the new view controller using [segue destinationViewController].
    // Pass the selected object to the new view controller.
    
    
    //得到点击的cell，sender是指触发跳转的那个控件，这里指的是点击的cell
    UITableViewCell *cell = (UITableViewCell *)sender;
    
    //通过当前被点击的cell得到点击位置
    NSIndexPath *selectedIndexPath = [self.tableView indexPathForCell:cell];
    
    Roster *roster = self.rosterArray[selectedIndexPath.row];
    
    //得到要跳转的viewcontroller
    ChatTableViewController *chatTableViewController = (ChatTableViewController *)[segue destinationViewController];
    
    chatTableViewController.chatToRoster = roster;
    
}

```

