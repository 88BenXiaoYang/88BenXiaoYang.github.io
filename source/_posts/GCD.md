---
title: GCD
date: 2019-05-30 11:04:50
tags: 多线程编程
categories: iOS
---

### 背景

* `CPU`命令列
  * 通常情况下，程序的代码行基本上是按从上到下的顺序执行的。应用程序源代码通过编译器转换为`CPU`命令列。汇集`CPU`命令列和数据，应用程序安装到设备上，操作系统在用户的指示下启动应用程序。操作系统首先便将包含在应用程序中的`CPU`命令列配置到内存中，`CPU`从应用程序指定的地址开始，一个一个地执行`CPU`命令列。
   
* 一个`CPU`执行的`CPU`命令列为一条无分叉路径，即为线程
* 多线程的执行，即多路径`CPU`命令列的执行，涉及到上下文切换，切换时每条路径的状态，会保存到各自路径专用的内存块中。路径状态，包括如`CPU`的寄存器等信息

* 由于使用多线程的程序可以在某个线程和其他线程之间反复多次进行上下文切换，因此看上去就好像`1`个`CPU`核能够并列地执行多个线程一样。在具有多个`CPU`核的情况下，就不是“看上去像”了，而是真的提供了多个`CPU`核并行执行多个线程的技术

* 多线程编程下易发生的问题：
  * 数据竞争：多个线程更新相同的资源，会导致数据的不一致
  * 死锁：停止等待事件的线程会导致多个线程相互持续等待
  * 大量内存消耗：使用太多线程会消耗大量内存

* 尽管多线程编程存在各种问题，但从保证应用程序的响应性能考虑，应该使用多线程编程
   
##### 多线程编程在`iOS`中的应用

应用程序在启动时，通过最先执行的线程，即“主线程”，来描绘用户界面、处理触摸屏幕的事件等。如果在主线程中进行长时间的处理，如数据库访问或`AR`画像的识别，就会妨碍主线程的执行(产生了阻塞)。在`OS X`和`iOS`的应用程序中，会妨碍主线程中被称为`RunLoop`的主循环的执行，从而导致不能更新用户界面、应用程序的画面长时间停滞等问题(界面交互体现就是产生了卡顿)。为了解决上述卡顿的问题，需要将长时间进行处理的操作，不放在主线程中执行，而放在其他线程中执行。

**使用多线程编程，在执行长时间的处理时仍可保证用户界面的响应性能**

##### `GCD`的引入给多线程编程带来的意义

* 多线程编程的实质
  * 使应用程序能并行的执行多路径(**多个**)`CPU`命令列(一个`CPU`命令列，即为一个**线程**)，并行的执行多个任务，并使多个线程间能相互通信(将耗时操作添加到非主线程中进行执行，待耗时操作处理完成后，将处理结果传回到主线程中执行对应的操作，这一过程便是线程间相互通信的体现)
  * 线程间的相互通信，在涉及网络数据获取或耗时操作的过程中，一般会使用异步任务+并行队列进行数据获取或耗时操作处理，当获取到网络数据或耗时操作处理完成后，需要在主线程中刷新界面显示数据，此时就涉及到线程间的通信。通信方向：子线程到主线程

Tips:

```
子线程执行耗时操作，主线程刷新UI

每个应用程序只有一个主线程，即只有一个主队列，所以将任务添加到主队列，就是回到主线程了
```

* `GCD`是**异步执行任务**的技术之一
  * `GCD`将应用程序中记述的线程管理用的代码在系统级中实现(即，在系统级中实现应用程序中线程管理的代码)。**线程管理的代码在系统级实现**
  * 通过`GCD`，开发者只需定义将要执行的任务并添加到适当的调度队列(`Dispatch Queue`)中，`GCD`就能生成必要的线程并计划执行任务(即，`GCD`能管理线程的生成(引入`GCD`后，线程的生成，不需要开发者参与管理，开发者只需将任务添加到`GCD`中，由`GCD`负责线程的管理，在多线程编程的过程中，开发者只需面向`GCD`编程开发即可)。任务是在线程上执行的)
  * `GCD`中线程管理是作为系统的一部分来实现的，因此可统一管理，也可执行任务，对多线程的编程、线程管理是更有效率的，`GCD`的引入正是为了实现这一目的。`GCD`用简洁的记述方法，实现了极为复杂繁琐的多线程编程。通过`GCD`提供的系统级线程管理提高执行效率

##### `GCD`的引入，对`iOS`多线程编程的影响

* 引入`GCD`之前
  * `Cocoa`框架提供了`NSObject`类的`performSelectorInBackground:withObject`实例方法和`performSelectorOnMainThread`实例方法等简单的多线程编程技术
  * 除了使用`performSelector`系方法进行多线程编程，还可使用`NSThread`类进行多线程编程，但较之`performSelector`系方法，使用`NSThread`实现多线程编程较繁琐
     * `NSThread`简介
         * 是多线程编程的技术之一，是面向对象操作线程的技术，可直接操作线程对象
         * 需要手动管理线程的生命周期
         * 需要使用线程同步的方法(如：临界区对象)，解决数据竞争的问题。实现线程同步的过程中对数据的加锁会有一定的开销。线程同步对数据的加锁方式有：`NSLock/NSConditionLock/NSRecursiveLock/@synchronized`
         * 实例化线程需要手动启动`(-start)`才能运行
         * 使用类方法创建线程：`+detachNewThreadWithBlock``+detachNewThreadSelector:toTarget:withObject:`。创建后就可执行，不需要手动启动。类方法的操作对象是针对当前线程而言
         * 可以设置线程的调度优先级，调度优先级取值范围为`0.0～1.0`，默认是`0.5`，值越大优先级越高
  * 无论使用`NSObject`类的`performSelector`系方法，还是使用`NSThread`实现多线程编程，所需的源代码量较之使用`GCD`实现的方式繁琐不少

* 引入`GCD`后
  * `GCD`将多线程编程的具体实现封装于`API`的实现中，通过操作`GCD`对外提供的`API`，使多线程编程易于实现。因此使用`GCD`大大简化了偏于复杂的多线程编程的源代码，同时要使用`GCD`实现多线程编程，需熟悉`GCD`对外提供的接口
  * `GCD`搭配`block`使用，可以进一步简化应用程序源代码

* 综上，`GCD`的引入，大大简化了`iOS`多线程编程的源代码，使`iOS`中的多线程编程便于实现和管理

### `GCD`的`API`

##### 苹果官方对`GCD`的说明

* 开发者要做的只是**定义想执行的任务(通常用block语法来定义)**并追加(用`GCD`提供的`API`)到适当的`Dispatch Queue`中
* 上述说明的源代码表示如下

```
dispatch_async(queue, ^{
//想执行的任务(用block来定义)
});

源代码使用Block语法“定义想执行的任务”，通过dispatch_async函数追加任务到赋值给变量queue的"Dispatch Queue"中
```

* 仅如上述源代码所示，就可使指定的`Block`在另一线程中执行

##### `Dispatch Queue`

* 是执行处理任务的等待队列
* `Dispatch Queue`按照追加的顺序(先进先出`FIFO`)执行处理(任务)
* `dispatch_queue_t`是表示`Dispatch Queue`的类型
* 种类

类型 | 名称 | 特点 | 同时执行的处理个数
--- | --- | --- | ---
串行 | Serial Dispatch Queue | 等待现在执行中的处理结束 | 同时执行的处理数只能有1个
并行 | Concurrent Dispatch Queue | 不等待现在执行中的处理结束 | 可并行执行多个处理，并行处理数量取决于当前系统的状态

* 系统状态
  * 即`iOS`和`OS X`基于`Dispatch Queue`中的处理数(任务数)、`CPU`核数以及`CPU`负荷等当前系统的状态来决定`Concurrent Dispatch Queue`中并行执行的处理数。“并行执行”，就是使用多个线程同时执行多个处理

* `Dispatch Queue`种类与线程的关系

类型 | 名称 | 线程数
--- | --- | ---
串行 | Serial Dispatch Queue | 使用一个线程
并行 | Concurrent Dispatch Queue | 使用多个线程

* `iOS`和`OS X`的核心
  * `XNU`内核决定应当使用的线程数，并只生成所需的线程执行处理。当处理结束，应当执行的处理数减少时，`XNU`内核会结束不再需要的线程
  * `XNU`内核仅使用`Concurrent Dispatch Queue`便可完美地管理并行执行多个处理(任务)的线程

* 获取`Dispatch Queue`的两种方法
  * 方法一：通过`GCD`的`API`生成`Dispatch Queue`，通过`dispatch_queue_create`函数可生成`Dispatch Queue`
     * 使用`dispatch_queue_create`函数，将第二个参数指定为`NULL`，生成的是`Serial Dispatch Queue`
     * `使用dispatch_queue_create`函数，将第二个参数指定为`DISPATCH_QUEUE_CONCURRENT`，生成的是`Concurrent Dispatch Queue`
     * 注意
         1. 使用`dispatch_queue_create`函数可生成任意多个`Dispatch Queue`
         2. 多个`Serial Dispatch Queue`可并行执行
         3. 一旦生成`Serial Dispatch Queue`并追加处理，系统对于一个`Serial Dispatch Queue`就只生成并使用一个线程。如果过多使用多线程，就会消耗大量内存，引起大量的上下文切换，大幅度降低系统的响应性能。因此因避免过多生成`Serial Dispatch Queue`
     * `Serial Dispatch Queue`的用途
         * 可解决多线程编程问题中的**数据竞争**。如多个线程更新相同资源或在访问数据库/文件时，存在数据竞争的问题，可使用`Serial Dispatch Queue`来解决数据竞争的问题
  * 方法二：获取系统标准提供的`Dispatch Queue`。这类`Dispatch Queue`不用特意生成，系统给我们提供的，有`Main Dispatch Queue`和`Global Dispatch Queue`
     * `Main Dispatch Queue`
         1. 追加到`Main Dispatch Queue`的处理在主线程的`RunLoop`中执行
         2. `Main Dispatch Queue`是在主线程中执行的`Dispatch Queue`。因主线程只有`1`个，所以`Main Dispatch Queue`是`Serial Dispatch Queue`
     * `Global Dispatch Queue`
         1. `Global Dispatch Queue`是所有应用程序都能够使用的`Concurrent Dispatch Queue`
         2. 没有必要通过`dispatch_queue_create`函数逐个生成`Concurrent Dispatch Queue`，只要获取`Global Dispatch Queue`使用即可
         3. `Global Dispatch Queue`有`4`个执行优先级：高优先级、默认优先级、低优先级、后台优先级
     * 对于`Main Dispatch Queue`和`Global Dispatch Queue`执行`dispatch_retain`和`dispatch_release`函数不会引起任何变化，也不会有任何问题。这也是获取并使用`Global Dispatch Queue`比生成、使用、释放`Concurrent Dispatch Queue`更轻松的原因

  * 对于`Concurrent Dispatch Queue`来说，不管生成多少，由于`XNU`内核只使用有效管理的线程，因此不会发生`Serial Dispatch Queue`那样，因大量生成`Serial Dispatch Queue`而消耗大量内存，降低系统响应性能的情况
  * 生成的`Dispatch Queue`必须由程序员负责释放，因`Dispatch Queue`并没有像`Block`那样具有作为`OC`对象来处理的技术
  * 在`dispatch_async`函数中追加`Block`到`Concurrent Dispatch Queue`，`Block`内部会通过`dispatch_retain`和`dispatch_release`函数对`Dispatch Queue`持有和释放
  * `Dispatch Queue`也像`OC`的引用计数式内存管理一样，需要通过`dispatch_retain`和`dispatch_release`函数的引用计数来管理内存

##### `dispatch_set_target_queue`

* 变更生成的`Dispatch Queue`的执行优先级要使用`dispatch_set_target_queue`函数
* 指定要变更执行优先级的`Dispatch Queue`为`dispatch_set_target_queue`函数的第一个参数，指定与要使用的执行优先级相同优先级的`Global Dispatch Queue`为第二个参数，即将第一个参数的执行优先级指定为与第二个参数相同的执行优先级，第二个参数为目标
* 注：`dispatch_set_target_queue`第一个参数如果指定系统提供的`Main Dispatch Queue`和`Global Dispatch Queue`则不知道会出现什么状况，因此第一个参数均不可指定`Main`和`Global`为参数
* 不仅可以变更`Dispatch Queue`的执行优先级，还可以作成`Dispatch Queue`的执行阶层。如，在多个`Serial Dispatch Queue`中用`dispatch_set_target_queue`函数指定目标为某一个`Serial Dispatch Queue`，那么原先本应并行执行的多个`Serial Dispatch Queue`，在目标`Serial Dispatch Queue`上只能同时执行一个处理

##### `dispatch_after`

* 想在指定时间后执行处理的情况，可使用`dispatch_after`函数来实现
* 注
  * `dispatch_after`函数并不是在指定时间后执行处理，而只是在指定时间追加处理到`Dispatch Queue`
* 参数
  * 第一个参数
     * 指定时间用的`dispatch_time_t`类型的值
     * 该值使用`dispatch_time`或`dispatch_walltime`函数生成
         * `dispatch_time`
             1. `dispatch_time`函数获取第一个参数值指定的时间，到第二个参数指定的毫微秒单位时间后的时间
             2. 数值和`NSEC_PER_SEC`的乘积得到单位为毫微秒的数值
             3. 使用`NSEC_PER_MSEC`则可以以毫秒为单位计算
             4. 通常用于计算相对时间
         * `dispatch_walltime`
             1. 由`POSIX`中使用的`struct timespec`类型的时间得到`dispatch_time_t`类型的值
             2. 用于计算绝对时间
  * 第二个参数
     * 指定要追加处理的`Dispatch Queue` 
  * 第三个参数
     * 指定记述要执行处理的`Block`

##### `Dispatch Group`

* 使用场景：在追加到`Dispatch Queue`中的多个处理全部结束后想执行结束处理
  * 针对上述场景，使用`Serial Dispatch Queue`时，只要将想执行的处理全部追加到该队列中，并在最后追加结束处理即可实现
  * 但使用`Concurrent Dispatch Queue`时或同时使用多个`Dispatch Queue`时，使用`Dispatch Group`进行处理

* 使用`Dispatch Group`的原因：无论向什么样的`Dispatch Queue`中追加处理，使用`Dispatch Group`都可监视这些处理执行的结束。一旦检测到所有处理执行结束，就可将结束的处理追加到`Dispatch Queue`中

* 在`Dispatch Group`中也可以使用`dispatch_group_wait`函数仅等待全部处理执行结束
  * 第二个参数值
     1. `DISPATCH_TIME_FOREVER`，意味着永久等待。只要属于`Dispatch Group`的处理尚未执行结束，就会一直等待，中途不能取消
     2. `DISPATCH_TIME_NOW`，则不用任何等待即可判定属于`Dispatch Group`的处理是否执行结束

##### `dispatch_barrier_async`

* 使用场景：在访问数据库或文件时，避免数据竞争，高效率地进行访问
* `dispatch_barrier_async`函数同`dispatch_queue_create`函数生成的`Concurrent Dispatch Queue`一起使用
* 控制流程：`dispatch_barrier_async`函数会等待追加到`Concurrent Dispatch Queue`上的并行执行的处理全部结束之后，再将指定的处理追加到该`Concurrent Dispatch Queue`中，然后在由`dispatch_barrier_async`函数追加的处理执行完毕后，`Concurrent Dispatch Queue`才恢复为一般的动作，追加到该`Concurrent Dispatch Queue`的处理又开始并行执行
* 使用`Concurrent Dispatch Queue`和`dispatch_barrier_async`函数可实现高效率的数据库访问和文件访问

##### `dispatch_sync`

* `dispatch_async`函数的`async`意味着“非同步”`(asynchronous)`，就是将指定的`Block`“非同步”地追加到指定的`Dispatch Queue`中。`dispatch_async`函数不做任何等待
* `dispatch_sync`函数的`sync`意味着“同步”`(synchronous)`，就是将指定的`Block`“同步”追加到指定的`Dispatch Queue`中。在追加的`Block`执行结束之前，`dispatch_sync`函数会一直等待。“等待”意味着当前线程停止
* 使用`dispatch_sync`容易引起死锁问题，因此在使用同步等待处理执行的API时需谨慎

##### `dispatch_apply`

* `dispatch_apply`函数是`dispatch_sync`函数和`Dispatch Group`的关联`API`
* `dispatch_apply`函数功能：按指定的次数将指定的`Block`追加到指定的`Dispatch Queue`中，并等待全部处理执行结束
* `dispatch_apply`函数与`dispatch_sync`函数相同，会等待处理执行结束

##### `dispatch_suspend/dispatch_resume`

* `dispatch_suspend`函数挂起指定的`Dispatch Queue`
* `dispatch_resume`函数恢复指定的`Dispatch Queue`
* `dispatch_suspend/dispatch_resume`函数对已经执行的处理没有影响。挂起后，追加到`Dispatch Queue`中但尚未执行的处理在此之后停止处理，而恢复则使得这些处理能够继续执行

##### `Dispatch Semaphore`

* `Dispatch Semaphore`是持有计数的信号，该计数是多线程编程中的计数类型信号。从更细粒度上控制并行执行处理更新数据时，会产生数据不一致的情况，进行排他控制的处理
* 在`Dispatch Semaphore`中，使用计数来实现，计数为`0`时等待，计数为`1`或大于`1`时，减去`1`而不等待的功能
* 使用
  * 使用函数`dispatch_semaphore_create()`生成`Dispatch Semaphore`
  * 参数表示计数的初始值
  * `dispatch_semaphore_wait`函数返回`0`时，可安全地执行需要进行排他控制的处理。该处理结束时通过`dispatch_semaphore_signal`函数将`Dispatch Semaphore`的计数值加`1`

* 在没有`Serial Dispatch Queue`和`dispatch_barrier_async`函数那么大粒度且一部分处理需要进行排他控制的情况下，`Dispatch Semaphore`可发挥作用。`Dispatch Semaphore`使用的粒度具体到并行执行中单一的线程上

##### `dispatch_once`

* dispatch_once函数是保证在应用程序执行中只执行一次指定处理的API
* 应用在单例模式中，单例对象的创建

##### `Dispatch I/O`

* 在读取较大文件时，将文件分成合适的大小并使用`Global Dispatch Queue`并列读取，会使读取速度快不少。可以通过使用`Dispatch I/O`，提高文件读取速度
* 现今的输入/输出硬件已经可以做到一次使用多个线程更快地并列读取了。实现这一功能的是`Dispatch I/O`和`Dispatch Data`

### `GCD`实现

##### `Dispatch Queue`

* 实现`GCD`所用到的工具
  * 用于管理追加的`Block`的`C`语言层实现的`FIFO`队列
  * `Atomic`函数中实现的用于排他控制的轻量级信号
  * 用于管理线程的`C`语言层实现的一些容器
* 线程管理中使用`GCD`的实质原因：应用程序中编写的线程管理用的代码要在系统级实现
  * 系统级，即`iOS`和`OS X`的核心`XNU`内核级上实现
  * 在系统级上，保证了实现管理线程的`GCD`的性能
* 使用`GCD`要比使用`pthreads`和`NSThread`这些一般的多线程编程`API`更好
* 用于实现`Dispatch Queue`而使用的软件组件

组件名称 | 提供技术
--- | ---
libdispatch | Dispatch Queue
Libc(pthreads) | pthread_workqueue
XNU内核 | workqueue

* 使用的`GCD`的`API`全部为包含在`libdispatch`库中的`C`语言函数
  * `Dispatch Queue`通过结构体和链表，被实现为`FIFO`队列
  * `FIFO`队列管理是通过`dispatch_async`等函数所追加的`Block`
  * `Block`并不是直接加入`FIFO`队列，而是先加入`Dispatch Continuation`这一`dispatch_continuation_t`类型结构体中，然后再加入`FIFO`队列。该`Dispatch Continuation`用于记忆`Block`所属的`Dispatch Group`和其他一些信息，即执行的上下文

##### `Dispatch Source`

* Dispatch Source是BSD系内核惯有功能kqueue的包装。kqueue是在XNU内核中发生各种事件时，在应用程序编程方执行处理的技术。其CPU负荷非常小，尽量不占用资源。kqueue可以说是应用程序处理XNU内核中发生的各种事件的方法中最优秀的一种
* Dispatch Source可处理以下事件

名称 | 内容
--- | ---
DISPATCH\_SOURCE\_TYPE\_DATA\_ADD | 变量增加
DISPATCH\_SOURCE\_TYPE\_DATA\_OR | 变量 OR
DISPATCH\_SOURCE\_TYPE\_MACH\_SEND | MACH端口发送
DISPATCH\_SOURCE\_TYPE\_MACH\_RECV | MACH端口接收
DISPATCH\_SOURCE\_TYPE\_PROC | 检测到与进程相关的事件
DISPATCH\_SOURCE\_TYPE\_READ | 可读取文件映像
DISPATCH\_SOURCE\_TYPE\_SIGNAL | 接收信号
DISPATCH\_SOURCE\_TYPE\_TIMER | 定时器
DISPATCH\_SOURCE\_TYPE\_VNODE | 文件系统有变更
DISPATCH\_SOURCE\_TYPE\_WRITE | 可写入文件映像

* 事件发生时，在指定的`Dispatch Queue`中可执行事件的处理

* 实际上`Dispatch Queue`没有“取消”这一概念。一旦将处理追加到`Dispatch Queue`中，就没有方法可将该处理去除，也没有方法可在执行中取消该处理
* `Dispatch Source`与`Dispatch Queue`不同，是可以取消的。而且取消时必须执行的处理可指定为回调用的`Block`形式。因此使用`Dispatch Source`实现`XNU`内核中发生的事件处理要比直接使用`kqueue`实现更为简单

### `GCD`中各种队列代码的示例

* 异步函数+并发队列

```
//并发队列
dispatch_queue_t queue = dispatch_queue_create("queueName", DISPATCH_QUEUE_CONCURRENT);
//异步函数
dispatch_async(queue, ^{
//要执行任务的代码
});
```

* 同步函数+并发队列

```
//并发队列 - 通过全局调度队列获取并发队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
//同步函数
dispatch_sync(queue, ^{
//要执行任务的代码
});
```

* 异步函数+串行队列

```
//串行队列
dispatch_queue_t queue = dispatch_queue_create("queueName", DISPATCH_QUEUE_SERIAL);
//异步函数
dispatch_async(queue, ^{
//要执行任务的代码
});
```

* 同步函数+串行队列

```
//串行队列
dispatch_queue_t queue = dispatch_queue_create("queueName", DISPATCH_QUEUE_SERIAL);
//同步函数
dispatch_sync(queue, ^{
//要执行任务的代码
});
```

* 异步函数+主队列

```
//主队列
dispatch_queue_t queue = dispatch_get_main_queue();
//异步函数
dispatch_async(queue, ^{
//要执行任务的代码
});
```

* 同步函数+主队列 - **你等我，我等你，最终结果是不执行了**

```
//主队列
dispatch_queue_t queue = dispatch_get_main_queue();
//同步函数
dispatch_sync(queue, ^{
//要执行任务的代码
});
```

* 注意：使用`sync`函数往当前串行队列中添加任务，会卡住当前的串行队列

* 总结

类型 | 全局并发队列 | 手动创建串行队列 | 主队列
--- | --- | --- | ---
同步(sync) | 没有开启新线程，串行执行任务 | 没有开启新线程，串行执行任务 | 没有开启新线程，串行执行任务
异步(async)| 有开启新线程，并发执行任务 | 有开启新线程，串行执行任务 | 没有开启新线程，串行执行任务

### `GCD`的实际应用

##### 温习

* 同步/异步 (侧重描述能不能开启新的线程)
  * 同步不开线程，对应函数`dispatch_sync` (在当前线程中执行任务，不具备开启新线程的能力)
  * 异步，除了主队列，都开线程，开多少条由队列的类型来决定，对应函数`dispatch_async` (在新的线程中执行任务，具备开启新线程的能力)
* 并发/串行 (侧重描述任务的执行方式)
  * 并发，多个任务并发(同时)执行
  * 串行，一个任务执行完毕后，再执行下一个任务
* 全局队列/主队列 
  * 全局队列，同步不开线程，异步开N条线程
  * 主队列，同步会卡死，异步不开线程(因它有主线程)
* `GCD`是苹果公司为多核的并行运算提出的解决方案
  * 这一方案会自动利用更多的`CPU`内核
  * 会自动管理线程的生命周期(创建线程、调度任务、销毁线程)

##### 使用场景

* 异步下载图片
* 线程安全-设置依赖关系
  * 如，异步下载`20`张图片写入一个数组中，为了保证效率所以数组为非线程安全的，如何解决多线程同时访问的问题。
     * 解决方法：通过`GCD`的阻塞(`dispatch_barrier_async`)和异步
     * 注：如使用了`dispatch_barrier_async`，须用创建的并发队列，而不能用全局队列
  * 如，想在dispatch_queue中所有的任务执行完成后再执行某种操作
     * 解决方法
         1. 串行队列中，可以把该操作放到最后一个任务执行完成后继续
         2. 并行队列中，使用`dispatch_group`进行操作(Global Dispatch Queue是并行队列，可以通过dispatch_get_global_queue(0,0)获得到)

* 单例(使用`dispatch_once`)
* 延迟执行(使用`dispatch_after`)
* 其他
  * `dispatch_suspend`函数只能暂停新追加未执行的`block`，已经处于执行中的`block`是无法暂停的，实质是将对应的`dispatch queue`挂起，从而达到暂停的效果

* 任务之间不太互相依赖，而需要更高的并发能力，`GCD`更有优势