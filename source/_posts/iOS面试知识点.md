---
title: iOS面试知识点
date: 2019-03-05 11:14:24
tags: iOS
categories: iOS
---

* `Apple`源码 [Apple Github](https://github.com/opensource-apple?tab=overview&from=2018-12-01&to=2018-12-31)、[Apple OpenSource](https://opensource.apple.com/source/)

##### `weak`

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

##### `KVO`

`KVO`即是`NSKeyValueObserving`，是对象(如：a)用来通知其他对象(如：b)指定的属性(如：name)发生了改变的一种非正式协议。其中：a是被监听的对象，b是监听的对象。

Tips:

```
非正式协议，是`NSObject`的一个分类`Category`，非正式协议的方法
是可选的。分类是OC的语言特性，能够给类对象添加方法而不需要创建子类。

正式协议，正式协议有自己的声明、类型检查语法。一个正式协议声明了类
需要实现的方法列表，可以使用required或者optional关键字指定方法
是否必须实现。正式协议也可以遵守其他协议。子类继承父类遵守的协议。
```

实现`KVO`的过程：

1.添加监听，监听指定的属性：

```
- (void)addObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath options:(NSKeyValueObservingOptions)options context:(nullable void *)context;
```

2.实现监听，收到监听反馈后，实现具体的逻辑处理。在监听的对象里，要实现非正式协议的方法，以对收到的监听反馈进行处理：

```
- (void)observeValueForKeyPath:(nullable NSString *)keyPath ofObject:(nullable id)object change:(nullable NSDictionary<NSKeyValueChangeKey, id> *)change context:(nullable void *)context;
```

3.移除监听：

```
- (void)removeObserver:(NSObject *)observer forKeyPath:(NSString *)keyPath;
```

`KVO`的底层实现：

在添加监听(`addObserver`)之后，实例对象的类对象发生了变化，系统动态添加了一个`NSKVONotifying_被监听的类名`的类(中间类)，如`NSKVONotifying_Animal`，`Animal`是被监听的类。因为改变对象属性的值是通过`setter`方法实现的，所以很明显是系统动态生成的中间类重写了`setter`方法。系统重写`setter`方法的过程中调用了`willChangeValueForKey`和`didChangeValueForKey`方法，`didChangeValueForKey`方法中调用了`observeValueForKeyPath`。

---

##### `Runtime`

`Runtime`运行时，是`OC`动态性的基础。`OC`的动态性体现在动态类型、动态绑定、动态加载。基于`Runtime`，`OC`语言将数据类型的确定由编译时推迟到了运行时。基于`Runtime`，在编译时确定的方法推迟到了运行时，所以能动态产生、修改、删除类、成员变量、方法。`Runtime`是一个`C`语言`API`的库。

常用的`Runtime`接口：

```
对应文件：#import <objc/runtime.h> 

操作成员变量、类、方法：
Ivar * class_copyIvarList : 获得某个类内部的所有成员变量
Method * class_copyMethodList : 获得某个类内部的所有方法
Method class_getInstanceMethod : 获得某个实例方法（对象方法，减号-开头）
Method class_getClassMethod : 获得某个类方法（加号+开头）
method_exchangeImplementations : 交换2个方法的具体实现
```

Tips:

```
消息传递，属于动态调用过程，即在编译阶段不能确定真正所要调用的函
数，只有在真正运行的时候才会根据函数的名称找到对应的函数来调用。
```

---
