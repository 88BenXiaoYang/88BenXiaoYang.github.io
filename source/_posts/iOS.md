---
title: iOS
date: 2019-01-09 09:19:02
tags: iOS
categories: 移动端
---

##### 获取UITableView中指定Cell的位置

```
//获取指定indexPath的cell的rect
//是UITableViewCell相对于tableView的rect
CGRect cellRect = [tableView rectForRowAtIndexPath:indexPath];

//将cell的frame转换为tableView所在父视图上的frame
//是UITableViewCell相对于tableView父视图的rect
CGRect rectInSuperview = [tableView convertRect:cellRect toView:[tableView superview]];
```