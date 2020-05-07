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
非正式协议，是NSObject的一个分类Category，非正式协议的方法
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

##### 从源代码到运行的`APP`

当点击了`build`之后，发生的事：**预处理(生成.i文件) - 编译(生成.s文件) - 汇编(生成.o文件) - 链接**

* 预处理`(Pre-process)`：把宏替换，删除注释，展开头文件，产生`.i`文件
* 编译`(Compliling)`：把之前的`.i`文件转换成汇编语言，产生`.s`文件
* 汇编`(Asembly)`：把汇编语言文件转换为机器码文件，产生`.o`文件
* 链接`(Link)`：对`.o`文件中的对于其他的库的引用的地方进行引用，生成最后的可执行文件(同时也包括多个`.o`文件进行`link`)

---

##### `self.navigationItem.title`与`self.title`有什么区别

* 联系
  * `self.title`是控制器默认`view`的`title`
  * `self.navigationItem.title`是显示在`navigationBar`中间的`title`
  * 修改`self.title`或者`self.navigationItem.title`都可以修改标题
* 区别
  * 标题始终显示`self.navigationItem.title`的值
  * 当`self.navigationItem.title`为空`(null)`时，则显示空白
  * 当`self.title`的值被修改时，`self.navigationItem.title`的值也会跟着修改为`self.title`的值
  * 当`self.navigationItem.title`的值被修改时，`self.title`的值不变，标题也会跟着修改为`self.navigationItem.title`的值
  * `self.title`作用于`tabbar`标题上和`self.navigationItem.title上`；`self.navigationItem.title`作用于`navigationBar`的标题上

---

##### `UINavigationController`的`title`不显示问题

`title`这个属性实际上是属于`UIViewController`而不属于`UINavigationController`。所以，这个属性是从`UIViewController`上面继承过来的。而不是`UINavigationController`上面的名字。由于`UINavigationController`属于容器，所以最少需要一个`RootController`。然后在`RootController`的`viewDidLoad`设置`title`而不是在`UINavigationController`的`subclass`中设置。而且`viewDidLoad`设置的`title`是统一显示的，导航视图控制的`UIViewController`的`title`都是一样的。

---

##### `category`和`extension`的区别

* `extension`(扩展)
  * 在编译期决定，是类的一部分。在编译期和头文件(`.h`)里的`@interface`以及实现文件(`.m`)里的`@implement`一起形成一个完整的类。`extension`伴随类的产生而产生，亦随类的消亡而消亡
  * 一般用来隐藏类的私有信息。必须有类的源码才能为一个类添加`extension`，所以无法为系统的类，如`NSString`添加`extension`

* `category`(分类)
  * 是在运行期决定的

从上面`category`和`extension`的区别来看，`extension`可以添加实例变量，而`category`是无法添加实例变量的，因在运行期，对象的内存布局已经确定，如果添加实例变量就会破坏类的内存布局，这对编译型语言来说是灾难性的。

* `category`的加载过程
  * 把`category`的实例方法、协议以及属性添加到类上
  * 把`category`的类方法添加到类的`metaclass`上

* 在类和`category`中都有`+load`方法时
  * 在类的`+load`方法调用的时候，可以调用`category`中声明的方法，因为附加`category`到类的操作会先于`+load`方法的执行
  * 类和`category`中的`+load`方法，调用顺序为，`+load`的执行顺序是先类，后`category`，而`category`的`+load`执行顺序是根据编译顺序决定的。对于`+load`的执行顺序是这样，但是对于**覆盖**掉的方法，则会先找到最后一个编译的`category`里对应的方法

* 关于使用`category`时，出现类中的方法被`category`中同名的方法**覆盖**掉的理解
  * `category`的方法**没有完全替换掉**原来类已经有的方法，即`category`和原来类都有同名方法`methodA`，那么`category`附加完成之后，类的方法列表里会有两个`methodA`
  * `category`的方法被放到了新方法列表的前面，而原来类的方法被放到了新方法列表的后面，这也就是平常所说的`category`中的方法会**覆盖**掉原来类的同名方法，因为运行时在查找方法的时候是顺着方法列表的顺序查找的，只有找到对应名字的方法，就会返回，不管后面会不会还有一样名字的方法

* 如何调用到原来类中被`category`**覆盖**掉的方法
  * `category`并不是完全替换掉原来类中的同名方法，只是`category`中的方法在方法列表的前面，优先命中了而已，所以只要顺着方法列表找到最后一个对应名字的方法，就可以调用原来类中的方法
  * 实现代码如下

  ```
    // 假设被覆盖的方法名叫 methodA。
    Class currentClass = [OriginalClass class];
    OriginalClass *my = [[OriginalClass alloc] init];

    if (currentClass) {
        unsigned int methodCount;
        Method *methodList = class_copyMethodList(currentClass, &methodCount);
        IMP lastImp = NULL;
        SEL lastSel = NULL;
        for (NSInteger i = 0; i < methodCount; i++) {
            Method method = methodList[i];
            NSString *methodName = [NSString stringWithCString:sel_getName(method_getName(method)) encoding:NSUTF8StringEncoding];
            if ([@"methodA" isEqualToString:methodName]) {
                lastImp = method_getImplementation(method);
                lastSel = method_getName(method);
            }
        }
        typedef void (*fn)(id,SEL);

        if (lastImp != NULL) {
            fn f = (fn) lastImp;
            f(my, lastSel);
        }
        free(methodList);
    }
  ```