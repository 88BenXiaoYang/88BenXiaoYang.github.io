---
title: iOS
date: 2019-01-09 09:19:02
tags: iOS
categories: 移动端
---

iOS开发过程中的知识点整理

* 导航图/启动图尺寸：

```
640*960     
640*1136
750*1334    
1242*2208
1125*2436
```

* icon尺寸：

```
29*29，57*57，
58*58，80*80，
87*87，114*114，
120*120，180*180
```

* 获取UITableView中指定Cell的位置

```
//获取指定indexPath的cell的rect
//是UITableViewCell相对于tableView的rect
CGRect cellRect = [tableView rectForRowAtIndexPath:indexPath];

//将cell的frame转换为tableView所在父视图上的frame
//是UITableViewCell相对于tableView父视图的rect
CGRect rectInSuperview = [tableView convertRect:cellRect toView:[tableView superview]];
```