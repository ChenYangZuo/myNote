# Qt开发随手记

## new与变量

new分配的内存在堆（heap）或自由存储区（free store）中；变量存储在栈（stack）中

## 多线程

1.  继承QThread的run方法
2.  继承QObject，将耗时操作放置在槽函数中，使用MoveToThread将对象移动至线程中
3.  继承QRunnable与QObject，使用全局线程池管理线程。线程池简化了创建线程和销毁线程的时间，针对密集型任务有优化作用，针对运行时间较长的任务优化作用不明显。

### 线程间通信

子线程1：

```c++
signals:
	void mysignal1(); // 信号只需要声明，不需要实现
```

子线程2：

```c++
public slots:
	void signalreceiver();

void signalreceiver(){
qDebug() << "Received.";
}
// 槽函数需要声明与实现

connect(thread1, &thread1::mysignal1, thread2, &thread2::signalreceiver);
// 将thread的信号绑定在thread2的槽函数上，实现线程间通信
```

## 原子操作

现代操作系统中，一般都提供了原子操作来实现一些同步操作，所谓原子操作，也就是一个独立而不可分割的操作。在单核环境中，一般的意义下原子操作中线程不会被切换，线程切换要么在原子操作之前，要么在原子操作完成之后。更广泛的意义下原子操作是指一系列必须整体完成的操作步骤，如果任何一步操作没有完成，那么所有完成的步骤都必须回滚，这样就可以保证要么所有操作步骤都未完成，要么所有操作步骤都被完成。

例如在单核系统里，单个的机器指令可以看成是原子操作（如果有编译器优化、乱序执行等情况除外）；在多核系统中，单个的机器指令就不是原子操作，因为多核系统里是多指令流并行运行的，一个核在执行一个指令时，其他核同时执行的指令有可能操作同一块内存区域，从而出现数据竞争现象。多核系统中的原子操作通常使用内存栅障（memory barrier）来实现，即一个CPU核在执行原子操作时，其他CPU核必须停止对内存操作或者不对指定的内存进行操作，这样才能避免数据竞争问题。

## 环境变量

终端执行目录下的文件优先级高于环境变量

## QLabel变形

布局中QLabel显示图片后大小改变的问题

设置label拉伸策略，`setSizePolicy(QSizePolicy::Ignored, QSizePolicy::Ignored);`

## malloc与calloc的区别

函数原型：

```c++
calloc(size_t _NumOfElements,size_t _SizeOfElements);

malloc(size_t _Size);
```

calloc分配n块指定大小的内存；malloc分配指定大小的内存

calloc在分配内存后将**自动初始化内存空间**，将数据清零；malloc在分配内存后不初始化，内存中为垃圾数据

## 函数指针

```c++
void (*handler)(void *) = nullptr;
```

声明一个名为handler的指针，该指针指向一个返回值为void的函数，该函数的参数是`void*`，意为任意类型的指针

## static修饰符
### 用在局部变量前：
1. 该变量在全局数据区(静态区)分配内存（局部变量在栈区分配内存）
2. 没有显式初始化时被自动初始化为0
3. 只初始化一次，可以将值保存至下一次函数调用时，而访问范围限定在函数内，不被其他地方访问到（局部变量在栈区，在函数结束后立即释放内存，所以局部变量在每次函数调用时都会被初始化）

## 信号与槽绑定

QT4风格：

connect(&subw,SIGNAL(mySignal()),this,SLOT(changeSubWindow()));

QT5风格：

connect(thread1, &MyThread::sendSignal, thread2, &MyThread::receiveSignal);

## 布局中QLabel显示图片后大小改变的问题

setSizePolicy(QSizePolicy::Ignored, QSizePolicy::Ignored);

## new加括号与不加括号

加括号：调用默认构造函数

不加括号：不调用构造函数

Example：

```c++
int *p1 = new int[10];   //内存中为垃圾数据
Int *p2 = new int[10]();  //内存中数据被清零
```

## do while(0){}的作用

包裹代码，用于宏定义处

## C++的内联函数

普通函数在调用时，需要将断点处的现场进行保护，跳转至函数内存地址；内联函数在编译时直接将函数语句在调用处展开，省去了跳转的操作，效率更高，但是多次调用内联函数将会多次展开，空间占用提高。

在.h文件中直接实现函数，编译器将默认该函数为内联函数；在函数声明或实现前加inline关键字也可将函数指定为内联函数；编译器发现函数体过长或存在递归时，将不接受内联提议，按照普通的函数规则编译。

## NULL与nullptr

在C语言中，NULL被**宏定义**为`#define NULL ((void *)0)`，即NULL在代替空指针可表示空指针的意思或0，这样就出现了二义性，在一些问题的出现上不易被发现，也不符合程序思想。
为解决NULL代指空指针存在的二义性问题。在C++11推出的nullptr这特一新**关键字**代替空指针，很好解决的上述出现的问题。即nullptr指向即为空指针，不存在二义性

## printf不输出

使用`fflush(stdout)`刷新缓存区


**关联文档：**
上一篇：[[C语言开发·[2]Char的那些事]]
下一篇：无