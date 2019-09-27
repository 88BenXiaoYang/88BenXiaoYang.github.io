---
title: Mac上给移动硬盘分区方法
date: 2019-09-26 17:21:30
tags:
---

### 场景

不兼容：一直在`Windows`上使用的移动硬盘居然在`Mac`上只读
原因：因为`Mac`与`PC`移动硬盘的存储格式不一样

### 格式化选择

* 移动硬盘格式化通常有三种选择
  1. `MS-DOS(FAT)`格式
      * 这种格式，`Mac`和`PC`都能读写，限制是不能存放大于`4GB`的东西。适合在`Mac`和`PC`之间共享文件

  2. `NTFS`格式
      * 这种格式是微软编的，但是在`Mac`上只读。在`Mac`上不能将硬盘格式化为`NTFS`
      * 在`MS-DOS(FAT)`格式下无法存储的`>4GB`的文件可以在这种格式下存储

  3. `Mac OS Extended`格式
      * `Mac`专有的格式，这种格式的硬盘/`U`盘在`PC`上不可见
      * 可以用来做`Time Machine`备份。[Time Machine的使用](https://support.apple.com/zh-cn/HT201250)

### 实际操作

以为`500GB`的移动硬盘分区为例

```
150 GB ”Shared” MS-DOS(FAT) 
100 GB ”Huge Files” NTFS 
250 GB ”Time Machine Disk” Mac OS Extended 

每个分区的大小根据实际使用情况来确定
```

* 操作步骤
  1. 把移动硬盘接到`Mac`上
  2. 打开`Disk Utility`(磁盘工具)这个`app`
  3. 选择你的移动硬盘，点击右侧的`erase`标签
  4. 把移动硬盘擦除为`MS-DOS(FAT)`格式 
  5. 点击`Partition`(分割)标签
  6. 更改`Partition Layout`为`3 partitions`，也就是重新划分为三个分区
  7. 给每个分区起名，最好是能见名知义的名字
  8. 为每个分区选择容量和格式
  9. 点击“应用”，等待
  10. 弹出硬盘，换到一台`Windows PC`上，接入移动硬盘
  11. 右键点击`Huge Files`(`Mac`上分出来的分区)这个硬盘，选择“格式化” 
  12. 把这个分区的格式改成`NTFS`然后等待完成，大约数分钟
  13. 完成！现在这个`500GB`的移动硬盘就有三个分区了，可以应付各种情况。