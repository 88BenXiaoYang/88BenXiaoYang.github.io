---
title: iOS多线程
date: 2020-04-02 10:41:41
tags:
---

### 知识点回顾

#### 多线程是什么

#### 线程和进程的区别

#### 多线程有什么用

#### 串行/并行

### `iOS`开发中几种多线程方案

在`iOS`中目前有`4`套多线程方案，分别是`Pthreads`、`NSThread`、`GCD`、`NSOperation & NSOperationQueue`。

多线程周边产品，如线程同步、延时执行、单例模式

#### `Pthreads`

* 概念
  * `POSIX`线程(`POSIX threads`)，简称`Pthreads`，是线程的`POSIX`标准。该标准定义了创建和操作线程的一整套`API`。在类`Unix`操作系统(`Unix`、`Linux`、`Mac OS X`等)中，都使用`Pthreads`作为操作系统的线程。
  * 即这是一套在很多操作系统上都通用的多线程`API`，有较强的移植性，因此在`iOS`中也是可以的。
  * 这是基于`C`语言的框架

* 使用
  * 使用过程中需要`C`语言函数，需要程序员手动管理线程的生命周期(即处理线程的各个状态的转换)
  * `iOS`开发中几乎不可能用到

#### `NSThread`

这套方案是经过苹果封装后的，并且完全面向对象的。所以可以直接操控线程对象，非常直观和方便。但是，它的生命周期需要程序员手动管理，因此这套方案也是偶尔用，如`[NSThread currentThread]`获取当前线程类，以知道当前线程的各种属性，用于调试十分方便。

* 使用
  * 创建并启动
     1. 先创建线程类，再启动
         * `OC方法` - `//创建 NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(run:) object:nil]; //启动 [thread start];`
         * `Swift方法` - `//创建 let thread = NSThread(target: self, selector: "run:", object: nil) //启动 thread.start()`
     2. 创建并自动启动
         * `OC方法` - `[NSThread detachNewThreadSelector:@selector(run:) toTarget:self withObject:nil];`
         * `Swift方法` - ` NSThread.detachNewThreadSelector("run:", toTarget: self, withObject: nil)`
     3. 使用`NSObject`的方法创建并自动启动
         * `OC方法` - `[self performSelectorInBackground:@selector(run:) withObject:nil];`
         * `Swift方法` - 因苹果认为`performSelector:`不安全，所以在`Swift`去掉了这个方法
  * 其他方法
  
  ```
  //取消线程
  - (void)cancel;
  //启动线程
  - (void)start;
  //判断某个线程的状态的属性
  @property (readonly, getter=isExecuting) BOOL executing;
  @property (readonly, getter=isFinished) BOOL finished;
  @property (readonly, getter=isCancelled) BOOL cancelled;
  //设置和获取线程名字
  -(void)setName:(NSString *)n;
  -(NSString *)name;
  //获取当前线程信息
  + (NSThread *)currentThread;
  //获取主线程信息
  + (NSThread *)mainThread;
  //使当前线程暂停一段时间，或者暂停到某个时刻
  + (void)sleepForTimeInterval:(NSTimeInterval)time;
  + (void)sleepUntilDate:(NSDate *)date;
  ```
  
  `NSThread`只有在一些非常简单的场景才会使用，因它还不够智能，如不能自动管理线程的生命周期；不能简便地处理多线程中的其他高级概念，如线程组、栅栏。
  
#### `GCD`

`GCD`全称是`Grand Central Dispatch`，它是苹果为多核的并行运算提出的解决方案，所以会自动合理地利用更多的`CPU`内核(比如双核、四核)，最重要的是它会自动管理线程的生命周期(创建线程、调度线程、销毁线程)，完全不需要程序员手动管理，只需要告诉它干什么就行。它使用的也是`C`语言，由于使用了`Block`，使得使用起来更加方便、灵活。

##### 任务和队列

在`GCD`中，加入了两个重要的概念，**任务**和**队列**。

* 任务，即操作，想要干什么，是一段处理代码，在`GCD`中就是一个`Block`，所以添加任务十分方便
  * 任务有两种执行方式，**同步执行(`sync`)**和**异步执行(`async`)**，他们之间的区别可以从**是否会有线程阻塞**和**是否会新开线程**来理解
      * **会不会阻塞当前线程，直到`Block`中的任务执行完毕**
         * 同步执行，它会阻塞当前线程并等待`Block`中的任务执行完毕，然后当前线程才会继续往下执行
         * 异步执行，当前线程会直接往下执行，它不会阻塞当前线程
      * **是否会创建新的线程**
         * 同步执行，只要是同步执行的任务，都会在当前线程执行，不会另开线程
         * 异步执行，只要是异步执行的任务，都会另开线程，在别的线程执行

* 队列，用于存放任务。有两种队列，**串行队列**和**并行队列**
  * 串行队列，放到串行队列的任务，`GCD`会`FIFO`(先进先出)地取出一个，执行一个，然后取下一个，这样一个一个的执行
  * 并行队列，放到并行队列中的任务，`GCD`也会`FIFO`的取出来，不同的是，它取出来一个任务就会放到别的线程，然后再取出一个任务又放到另一个线程。这样由于取的动作很快，看起来，所有的任务都是一起执行的。需要注意，`GCD`会根据系统资源控制并行的数量，所以如果任务很多，`GCD`并不会让所有任务同时执行。

任务在同步和异步，及在串行和并行队列的环境下的执行情况如下表

| | 同步执行 | 异步执行 |
| --- | --- | --- |
| 串行队列 | 在当前线程，任务一个一个执行 | 在其他线程，任务一个一个执行 |
| 并行队列 | 在当前线程，任务一个一个执行 | `GCD`会开很多线程，任务会被分配到已创建的线程中，一起执行 |

##### 创建队列

* 主队列：这是一个特殊的串行队列。主队列用于刷新`UI`，任何需要刷新`UI`的工作都要在主队列(对应主线程)中执行，所以一般耗时的任务都要放到别的线程执行

```
获取主队列的代码：

//OBJECTIVE-C
dispatch_queue_t queue = dispatch_get_main_queue();
 
//SWIFT
let queue = dispatch_get_main_queue()
```

* 自己创建的队列：可以创建串行队列，也可以创建并行队列。创建队列的函数，第一个参数，用于`DEBUG`标识，可以为空；第二个参数用来表示创建的队列是串行的还是并行的，传入`DISPATCH_QUEUE_SERIAL`或`NULL`表示创建的是串行队列，传入`DISPATCH_QUEUE_CONCURRENT`表示创建的是并行队列

```
自己创建队列的代码：(创建串行队列为例)

//OBJECTIVE-C
dispatch_queue_t queue = dispatch_queue_create("tk.bourne.testQueue", NULL);

//SWIFT
let queue = dispatch_queue_create("tk.bourne.testQueue", nil)
```

* 全局并行队列：`GCD`维护的唯一一个并行队列，只要是并行任务一般都加入到这个队列

```
获取全局并行队列的代码：

//OBJECTIVE-C
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);

//SWIFT
let queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)
```

##### 创建任务

* 同步任务(`SYNC`)：不会另开线程

```
创建代码：

//OC
dispatch_sync(, ^{
  //code here
  NSLog(@"%@", [NSThread currentThread]);
});

//Swift
dispatch_sync(, { () -> Void in
  //code here
  println(NSThread.currentThread())
})
```

* 异步任务(`ASYNC`)：会另开线程

```
创建代码：

//OC
dispatch_async(, ^{
  //code here
  NSLog(@"%@", [NSThread currentThread]);
});

//Swift
dispatch_async(, { () -> Void in
  //code here
  println(NSThread.currentThread())
})
```

* 示例解析
  * 示例一
  
  ```
  以下代码在主线程调用，结果是什么？
  
  NSLog("AAA - %@", NSThread.currentThread())
dispatch_sync(dispatch_get_main_queue(), { () -> Void in 
        NSLog("sync - %@", NSThread.currentThread())
})
NSLog("BBB - %@", NSThread.currentThread())
答案：只会打印第一句：AAA，然后主线程就卡住了，此时如果界面上有按钮，会发现按钮点击不了
解析：同步任务会阻塞当前线程，然后把Block中的任务放到指定的队列中执行，只有等到Block中的任务完
成后才会让当前线程继续往下执行。上面代码的执行步骤是：打印完第一句后，`dispatch_sync`立即阻塞
当前的主线程，然后把Block中的任务放到`main_queue`中。`main_queue`中的任务会被取出来放到主
线程中执行。但主线程这时已经被阻塞了，所以Block中的任务就不能完成，Block中的任务不完成，
`dispatch_sync`就会一直阻塞主线程，产生了死锁现象，最终导致主线程一直卡死。
 ```
  
  * 示例二

  ```
  以下代码会产生什么结果？
  
  let queue = dispatch_queue_create("myQueue", DISPATCH_QUEUE_SERIAL)
   NSLog("AAA - %@", NSThread.currentThread())
    dispatch_async(queue, { () -> Void in
        NSLog("syncBBB - %@", NSThread.currentThread())
        dispatch_sync(queue, { () -> Void in
             NSLog("sync - %@", NSThread.currentThread())
        })
        NSLog("syncCCC - %@", NSThread.currentThread())
   })
  NSLog("DDD - %@", NSThread.currentThread())
  答案：AAA - 、syncBBB -、DDD -打印出来了，sync -、syncCCC -没有打印出来
  解析：
  1、使用DISPATCH_QUEUE_SERIAL这个参数，创建了一个串行队列
  2、打印AAA -这句
  3、dispatch_async异步执行，所以当前线程不会被阻塞，这种情况下有了两条线程，一条当前线程继续
  往下执行，打印出DDD -这句，另一条线程执行Block中的内容，打印syncBBB -这句。因为这两条线程
  是并行的，所以打印的先后顺序无所谓。
  4、重点！dispatch_sync同步执行，于是它所在的线程会被阻塞，一直等到sync里的任务执行完才会继
  续往下执行。sync把自己Block中的任务放到queue中，queue是一个串行队列，一次执行一个任务，所
  以sync的Block必须等到前一个任务执行完毕，但此时queue正在执行的任务就是被sync阻塞了的那个，
  于是发生了死锁，所以sync所在的线程被卡死了，因此剩下的两句log不会被执行自然就不会被打印了。
  ```
  
##### 队列组

队列组可以将很多队列添加到一个组里，这样做的好处是，当这个组里所有的任务都执行完了，队列组会通过一个方法通知我们。

```
队列组使用的代码：

//OC
//1.创建队列组
dispatch_group_t group = dispatch_group_create();
//2.创建队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
//3.多次使用队列组的方法执行任务, 只有异步方法
//3.1.执行3次循环
dispatch_group_async(group, queue, ^{
    for (NSInteger i = 0; i < 3; i++) {
        NSLog(@"group-01 - %@", [NSThread currentThread]);
    }
});
//3.2.主队列执行8次循环
dispatch_group_async(group, dispatch_get_main_queue(), ^{
    for (NSInteger i = 0; i < 8; i++) {
        NSLog(@"group-02 - %@", [NSThread currentThread]);
    }
});
//3.3.执行5次循环
dispatch_group_async(group, queue, ^{
    for (NSInteger i = 0; i < 5; i++) {
        NSLog(@"group-03 - %@", [NSThread currentThread]);
    }
});
//4.都完成后会自动通知
dispatch_group_notify(group, dispatch_get_main_queue(), ^{
    NSLog(@"完成 - %@", [NSThread currentThread]);
});
```

##### `dispatch_barrier_`的使用

* `func dispatch_barrier_async(_ queue: dispatch_queue_t, _ block: dispatch_block_t);`
  * 这个方法重点是传入的`queue`，当传入的`queue`是通过`DISPATCH_QUEUE_CONCURRENT`参数自己创建的`queue`时，`dispatch_barrier_async`这个方法会阻塞这个`queue`(是阻塞传入的自定义创建的`queue`，而不是阻塞当前线程)，一直等到这个`queue`中排在它前面的任务都执行完成后才会开始执行自己，自己执行完毕后，才会取消阻塞，使这个`queue`中排在它后面的任务继续执行
  * 如果传入的不是自定义创建的`queue`，那么它就和`dispatch_async`的功能一样了

* `func dispatch_barrier_sync(_ queue: dispatch_queue_t, _ block: dispatch_block_t);`
  * 这个方法重点也是传入的`queue`,当传入的`queue`是通过`DISPATCH_QUEUE_CONCURRENT`参数自己创建的`queue`时，`dispatch_barrier_sync`这个方法会阻塞这个`queue`，同时还会阻塞当前线程，一直等到这个`queue`中排在它前面的任务都执行完成后才会开始执行自己，自己执行完毕后，才会取消阻塞，使这个`queue`中排在它后面的任务继续执行
  * 如果传入的不是自定义创建的`queue`，那么它就和`dispatch_sync`的功能一样了

#### `NSOperation`和`NSOperationQueue`

`NSOperation`是苹果公司对`GCD`的封装，完全面向对象。`NSOperation`和`NSOperationQueue`分别对应`GCD`中的**任务**和**队列**。

1. 将要执行的任务封装到一个`NSOperation`对象中
2. 将此任务添加到一个`NSOperationQueue`对象中

执行完上述操作后，系统会自动执行任务。具体执行过程，见下面内容：

##### 添加任务

`NSOperation`只是一个抽象类，所以不能封装任务，它有2个子类用于封装任务，分别是`NSInvocationOperation`和`NSBlockOperation`。创建一个`Operation`后，需要调用`start`方法来启动任务，它会默认在**当前队列同步**执行；也可以在中途取消一个任务，只需要调用其`cancel`方法即可。

* `NSInvocationOperation`
  * 需要传入一个方法名
  * 创建代码如下
  	
  ```
  //OC
   //1.创建NSInvocationOperation对象
  NSInvocationOperation *operation = [[NSInvocationOperation alloc] initWithTarget:self selector:@selector(run) object:nil];
  
  //2.开始执行
  [operation start];
  
  因NSInvocationOperation不是类型安全的，所以Swift中没有对应的创建方法
  ```
  
* `NSBlockOperation`

```
//OC
//1.创建NSBlockOperation对象
NSBlockOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
   NSLog(@"%@", [NSThread currentThread]);
}];
//2.开始任务
[operation start];

//Swift
//1.创建NSBlockOperation对象
let operation = NSBlockOperation { () -> Void in
    println(NSThread.currentThread())
}
//2.开始任务
operation.start()
```

创建的`Operation`任务，默认会在当前线程执行。但`NSBlockOperation`有一个方法`addExecutionBlock`，通过这个方法可以给`Operation`添加多个执行`Block`，这样`Operation`中的任务会并发执行，`Operation`会在**主线程**和**其他的多个线程**执行这些任务。

`addExecutionBlock`的使用示例代码如下：

```
//OC
//1.创建NSBlockOperation对象
NSBlockOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
   NSLog(@"%@", [NSThread currentThread]);
}];
//添加多个Block
for (NSInteger i = 0; i < 5; i++) {
    [operation addExecutionBlock:^{
        NSLog(@"第%ld次：%@", i, [NSThread currentThread]);
    }];
}
//2.开始任务
[operation start];

//Swift
//1.创建NSBlockOperation对象
let operation = NSBlockOperation { () -> Void in
    NSLog("%@", NSThread.currentThread())
}
//2.添加多个Block
for i in 0.. Void in
    NSLog("第%ld次 - %@", i, NSThread.currentThread())
 }
}
//2.开始任务
operation.start()
```

注：`addExecutionBlock`方法必须在`start()`方法之前执行，否则会报错。

* 自定义`Operation`

除了`NSInvocationOperation`和`NSBlockOperation`两种`Operation`外，还可以自定义`Operation`。自定义`Operation`需要继承`NSOperation`类，并实现其`main()`方法，因为在调用`start()`方法的时候，内部会调用`main()`方法完成相关逻辑。同时还需要实现`cancel()`在内的各种方法。

##### 创建队列

可以调用一个`NSOperation`对象的`start()`方法来启动这个任务，但是这样做任务默认是同步执行的，就算使用`addExecutionBlock`方法，也会在**当前线程**和**其他线程**中执行，即还是会占用当前线程。这时就要用到队列`NSOperationQueue`了，按类型来说一共有两种类型，分为**主队列**和**其他队列**。只要任务添加到队列，会自动调用任务的`start()`方法。

* 主队列
  * 在`iOS`中，每套多线程方案都会有一个主线程，这是一个特殊的线程，必须串行。所以添加到主队列的任务都会一个接一个地排着队在主线程中处理

  ```
  获取主队列的代码：
  
  //OBJECTIVE-C
NSOperationQueue *queue = [NSOperationQueue mainQueue];
//SWIFT
let queue = NSOperationQueue.mainQueue()
  ```
  
* 其他队列
  * 因为主队列比较特殊，会单独有一个类方法来获得主队列。那通过初始化产生的队列就是**其他队列**。因为只有两种队列，除了主队列，其他队列就不需要名字了
  * 其他队列的任务会在其他线程并行执行

  ```
  创建其他队列的代码：
  
  //OC
  //1.创建一个其他队列    
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
//2.创建NSBlockOperation对象
NSBlockOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"%@", [NSThread currentThread]);
}];
//3.添加多个Block
for (NSInteger i = 0; i < 5; i++) {
    [operation addExecutionBlock:^{
        NSLog(@"第%ld次：%@", i, [NSThread currentThread]);
    }];
}
//4.队列添加任务
[queue addOperation:operation];
.
.
.
//Swift
//1.创建其他队列
let queue = NSOperationQueue()
//2.创建NSBlockOperation对象
let operation = NSBlockOperation { () -> Void in
    NSLog("%@", NSThread.currentThread())
}
//3.添加多个Block
for i in 0.. Void in
        NSLog("第%ld次 - %@", i, NSThread.currentThread())
    }
}
//4.队列添加任务
queue.addOperation(operation)
  ```
  
* `NSOperationQueue`与`GCD`的比较
  * 队列的比较
     * `NSOperationQueue`中没有串行队列，但`NSOperationQueue`中有一个参数`maxConcurrentOperationCount`最大并发数，用来设置最多可以让多少个任务同时执行。当把该参数设置为`1`时，任务就是串行执行了
  * 添加任务的比价
     * `NSOperationQueue`有一个添加任务的方法`- (void)addOperationWithBlock:(void (^)(void))block;`，和`GCD`类似，通过该方法就可以添加一个任务到队列中了

##### `NSOperation`的使用

* 添加依赖
  * 这是`NSOperation`实用的功能之一。使用场景如，有`3`个任务，`A`-从服务器上下载一张图片；`B`-给这张图片加个水印；`C`-把加水印的图片返回给服务器，这种场景下使用依赖进行处理
  * 实现代码如下

  ```
  //OC
  //1.任务一：下载图片
NSBlockOperation *operation1 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"下载图片 - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];
//2.任务二：打水印
NSBlockOperation *operation2 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"打水印   - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];
//3.任务三：上传图片
NSBlockOperation *operation3 = [NSBlockOperation blockOperationWithBlock:^{
    NSLog(@"上传图片 - %@", [NSThread currentThread]);
    [NSThread sleepForTimeInterval:1.0];
}];
//4.设置依赖
[operation2 addDependency:operation1];      //任务二依赖任务一
[operation3 addDependency:operation2];      //任务三依赖任务二
//5.创建队列并加入任务
NSOperationQueue *queue = [[NSOperationQueue alloc] init];
[queue addOperations:@[operation3, operation2, operation1] waitUntilFinished:NO];
  ```
  
  ```
  //Swift
  //1.任务一：下载图片
let operation1 = NSBlockOperation { () -> Void in
    NSLog("下载图片 - %@", NSThread.currentThread())
    NSThread.sleepForTimeInterval(1.0)
}
//2.任务二：打水印
let operation2 = NSBlockOperation { () -> Void in
    NSLog("打水印   - %@", NSThread.currentThread())
    NSThread.sleepForTimeInterval(1.0)
}
//3.任务三：上传图片
let operation3 = NSBlockOperation { () -> Void in
    NSLog("上传图片 - %@", NSThread.currentThread())
    NSThread.sleepForTimeInterval(1.0)
}
//4.设置依赖
operation2.addDependency(operation1)    //任务二依赖任务一
operation3.addDependency(operation2)    //任务三依赖任务二
//5.创建队列并加入任务
let queue = NSOperationQueue()
queue.addOperations([operation3, operation2, operation1], waitUntilFinished: false)
  ```
  
  * 使用依赖功能时应注意
     * 不能添加相互依赖，不然会产生死锁。如`A`依赖`B`，`B`依赖`A`
     * 可以使用`removeDependency`来解除依赖关系
     * 可以在不同的队列之间依赖，因这个依赖是添加到任务身上的，和队列没关系

##### 其他方法

* `NSOperation`

```
BOOL executing; //判断任务是否正在执行
BOOL finished; //判断任务是否完成
void (^completionBlock)(void); //用来设置完成后需要执行的操作
- (void)cancel; //取消任务
- (void)waitUntilFinished; //阻塞当前线程直到此任务执行完毕
```

* `NSOperationQueue`

```
NSUInteger operationCount; //获取队列的任务数
- (void)cancelAllOperations; //取消队列中所有的任务
- (void)waitUntilAllOperationsAreFinished; //阻塞当前线程直到此队列中的所有任务执行完毕
[queue setSuspended:YES]; // 暂停queue
[queue setSuspended:NO]; // 继续queue
```

### 多线程的使用

#### 线程同步

线程同步，是为了防止多个线程抢夺同一个资源造成的数据安全问题，所采取的一种措施。有以下实现线程同步的方法

* 互斥锁
  * 原理-给需要同步的代码块加一个互斥锁，就可以保证每次只有一个线程访问此代码块
  * 实现代码

  ```
  //OC
  @synchronized(self) {
    //需要执行的代码块
  }

  //Swift
  objc_sync_enter(self)
  //需要执行的代码块
  objc_sync_exit(self)
  ```
  
* 同步执行
  * 原理-使用多线程的知识，把多个线程都要执行的代码块添加到同一个串行队列，通过这种方式实现线程同步的概念
  * 实现代码(使用`GCD`和`NSOperation`的两种方案)

  ```
  //OC
  //GCD
  //需要一个全局变量queue，要让所有线程的这个操作都加到一个queue中
  dispatch_sync(queue, ^{
      NSInteger ticket = lastTicket;
      [NSThread sleepForTimeInterval:0.1];
      NSLog(@"%ld - %@",ticket, [NSThread currentThread]);
      ticket -= 1;
      lastTicket = ticket;
  });
  //NSOperation & NSOperationQueue
  //重点：1. 全局的 NSOperationQueue, 所有的操作添加到同一个queue中
  //       2. 设置 queue 的 maxConcurrentOperationCount 为 1
  //       3. 如果后续操作需要Block中的结果，就需要调用每个操作的waitUntilFinished，阻塞当前线程，一直等到当前操作完成，才允许执行后面的。waitUntilFinished 要在添加到队列之后！
  NSBlockOperation *operation = [NSBlockOperation blockOperationWithBlock:^{
      NSInteger ticket = lastTicket;
      [NSThread sleepForTimeInterval:1];
      NSLog(@"%ld - %@",ticket, [NSThread currentThread]);
      ticket -= 1;
      lastTicket = ticket;
  }];
  [queue addOperation:operation];
  [operation waitUntilFinished];
  //后续要做的事
  ```
  
#### 延迟执行

延迟执行，就是延时一段时间再执行某段代码。有以下实现延迟执行的常用方法

* `perform`
  * 实现代码

  ```
  //OC
  // 3秒后自动调用self的run:方法，并且传递参数：@"abc"
  [self performSelector:@selector(run:) withObject:@"abc" afterDelay:3];
  
  //Swift中无对应方法
  ```
  
* `GCD`
  * 可以使用`GCD`中的`dispatch_after`方法，`OC`和`Swift`中都可以使用
  * 实现代码

  ```
  //OC
  // 创建队列
dispatch_queue_t queue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
// 设置延时，单位秒
double delay = 3; 
dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delay * NSEC_PER_SEC)), queue, ^{
    // 3秒后需要执行的任务
});
  ```

* `NSTimer`
  * 计时器实现延迟执行
  * 实现代码

  ```
  //OC
  [NSTimer scheduledTimerWithTimeInterval:3.0 target:self selector:@selector(run:) userInfo:@"abc" repeats:NO];
  ```
  
#### 单例模式

这里使用`GCD`的`dispatch_once`方法来实现

实现代码

```
//OC
@interface Tool : NSObject
+ (instancetype)sharedTool;
@end
@implementation Tool
static id _instance;
+ (instancetype)sharedTool {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _instance = [[Tool alloc] init];
    });
    return _instance;
}
@end
```

#### 从其他线程回到主线程的方法

在其他线程操作完成后需要回到主线程更新`UI`。有以下实现回到主线程的方法

* `NSThread`

```
//Objective-C
[self performSelectorOnMainThread:@selector(run) withObject:nil waitUntilDone:NO];

//Swift
//swift 取消了 performSelector 方法。
```

* `GCD`

```
//Objective-C
dispatch_async(dispatch_get_main_queue(), ^{
});
//Swift
dispatch_async(dispatch_get_main_queue(), { () -> Void in
})
```

* `NSOperationQueue`

```
//Objective-C
[[NSOperationQueue mainQueue] addOperationWithBlock:^{
}];
//Swift
NSOperationQueue.mainQueue().addOperationWithBlock { () -> Void in
}
```

### 不同多线程方案的使用方法和注意事项