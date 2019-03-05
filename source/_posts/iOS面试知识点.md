---
title: iOS面试知识点
date: 2019-03-05 11:14:24
tags: iOS
categories: iOS
---

* `Apple`源码 [Apple Github](https://github.com/opensource-apple?tab=overview&from=2018-12-01&to=2018-12-31)、[Apple OpenSource](https://opensource.apple.com/source/)

* `weak`关键字

`weak`关键字的作用弱引用，所引用对象的计数器不会加一，并在引用对象被释放的时候，`weak`修饰的对象会自动被设置为`nil`。

`weak`底层原理：

```
weak 的功能实现通过runtime初始化并维护的。weak是由Runtime维护
的weak表。weak表是一个hash表，用于存储指向某个对象的所有weak指
针，key是指向对象的地址，value是weak指针的地址<这个地址的值是所
指对象指针的地址>数组。

具体步骤：
1，初始化时：runtime会调用objc_initWeak函数，初始化一个新的
weak指针指向对象的地址。
2， 添加引用时：objc_initWeak函数会调用 storeWeak() 函数，
 storeWeak() 的作用是更新指针指向，创建对应的弱引用表。
3，释放时,调用clearDeallocating函数。clearDeallocating函数
首先根据对象地址获取所有weak指针地址的数组，然后遍历这个数组把其中
的数据设为nil，最后把这个entry从weak表中删除，最后清理对象的记录。
```

`runtime`源码中对应的`weak`文件，见[objc4-750](https://opensource.apple.com/source/objc4/objc4-750/runtime/)。

补充：

```
hash表是链表数组的数据结构，操作存储的数据时会用到对应的hash函数和 equals函数。
```

扩展题：

```
两个对象 Person p1 p2
问题：如果两个对象的哈希值相同，p1.hashCode()==p2.hashCode()
两个对象的equals一定返回true吗？p1.equals(p2)一定是true吗？
答案：不一定

如果两个对象的equals方法返回true,p1.equals(p2)==true
两个对象的哈希值一定相同吗？
答案：一定
```
---