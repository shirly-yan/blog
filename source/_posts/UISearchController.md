---
title: UISearchController
date: 2016-10-08 11:39:43
categories: objective-c
---
<!-- more -->

```objc
  self.allMovieArray = [NSMutableArray array];
    self.searchArray = [NSMutableArray array];

    /************UITableView*************/
    self.myTableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, self.view.frame.size.width, self.view.frame.size.height) style:UITableViewStylePlain];
    [self.view addSubview:self.myTableView];
    [_myTableView release];
    self.myTableView.delegate = self;
    self.myTableView.dataSource = self;
    /************UITableView*************/
   
   
    /************UISearchController*************/
    //创建UISearchController
    self.searchController = [[UISearchController alloc] initWithSearchResultsController:nil];
    self.myTableView.tableHeaderView = self.searchController.searchBar;
   
    //成为代理
    self.searchController.searchResultsUpdater = self;
    /************UISearchController*************/

   
    /************interface*************/
    //设置开始搜索时背景显示与否 页面背景阴影效果,一般不要设置.
    self.searchController.dimsBackgroundDuringPresentation = NO;
   
    //设置点击搜索框时候隐藏导航栏
    self.searchController.hidesNavigationBarDuringPresentation = NO;
    /************interface*************/
   
   
    /************searchBar*************/
    self.searchController.searchBar.frame = CGRectMake(self.searchController.searchBar.frame.origin.x, self.searchController.searchBar.frame.origin.y, self.searchController.searchBar.frame.size.width, 44.0);
//    [searchBar sizeToFit]:设置searchBar位置自适应
    [self.searchController.searchBar sizeToFit];
    /************searchBar*************/



/*****************searchMethod*********s*********/
//UISearchController的移除
//在viewWillDisappear中要将UISearchController移除, 否则切换到下一个View中, 搜索框仍然会有短暂的存在.
- (void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    if (self.searchController.active) {
        self.searchController.active = NO;
        [self.searchController.searchBar removeFromSuperview];
    }
}
-(void)updateSearchResultsForSearchController:(UISearchController *)searchController {
    NSString *searchString = [self.searchController.searchBar text];
    NSLog(@"===%@",searchString);

  NSPredicate *preicate = [NSPredicate predicateWithFormat:@"SELF CONTAINS[c] %@", searchString];
    if (self.searchArray!= nil) {
        [self.searchArray removeAllObjects];
    }
    //过滤数据
    /*
     对象数组过滤

     ios提供了一个filteredArrayUsingPredicate 方法，通过给定条件来进行过滤，过滤后形成一个新的数组。 而NSMutableArray提供了一个filterUsingPredicate方法，在原数组中保留符合条件的数组元素。
     */
    self.searchArray= [NSMutableArray arrayWithArray:[self.allMovieArray filteredArrayUsingPredicate:preicate]];
    NSLog(@"searchArray===%@",self.searchArray);

    //刷新表格
    [self.myTableView reloadData];
}
/*****************searchMethod*********s*********/



/*****************tableviewmethod*********s*********/
-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
     //如果searchController是活动状态,说明是当前正在搜索
    if (self.searchController.active) {

        return self.searchArray.count;
    }
    else {
        return self.allMovieArray.count;
    }

}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
     static NSString *cellIdentifier = @"cell";
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if (cell == nil) {
       
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellIdentifier];
    }
    if (self.searchController.active) {
        cell.textLabel.text = [self.searchArray objectAtIndex:indexPath.row];
    }
    else {
        cell.textLabel.text = [self.allMovieArray objectAtIndex:indexPath.row];
    }


    return cell;
}

-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{

    DetailViewController *detailVC = [[DetailViewController alloc] init];
    //如果searchController是活动状态,说明是当前正在搜索
    if (self.searchController.active) {

        NSLog(@"选中搜索列表");
        detailVC.movieName = [self.searchArray objectAtIndex:indexPath.row];
    }
    else {

        NSLog(@"选中所有列表");
        detailVC.movieName = [self.allMovieArray objectAtIndex:indexPath.row];
    }
    [self.navigationController pushViewController:detailVC animated:YES];
    [detailVC release];

}


-(CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 60;

}
/*****************tableviewmethod*********s*********/
```

