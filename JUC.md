# JUC

## 简介

JUC就是java.util.concurrent工具包的简称。是处理线程的工具包

## 进程和线程

### 进程(Process)

进程是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础

### 线程(Thread)

线程是操作系统能够进行运算调度的最小单位，被包含在进程中，是进程的实际运作单位，一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每个线程并行执行不同的任务

## 并行和并发

> 微观串行，宏观并行

一般把线程轮流使用CPU的做法称为并发，concurrent

多核CPU下，每个核都可以调度运行线程，这时候线程可以是并行的 parallel

## 异步调用

从方法调用的角度来将，如果

- 需要等待结果返回，才能继续运行就是同步
- 不需要等待结果返回，就能继续运行就是异步

同步在多线程中还有另外一个意思，让多个线程步调一致





# Java线程

## 创建和运行线程

### 一、直接使用Thread

```java
Thread t = new Thread() {
    public void run() {
        // 执行的代码
    }
};
t.start();
```

### 二、使用Runnable配合Thread

把**线程**和**任务** 分开

- Thread代表线程
- Runnable是可运行的任务

```java
Runnable runnable = new Runnable() {
    public void run() {
        // 执行的代码
    }
};
Thread t = new Thread(runnable);
t.start();
```

### 三、FutureTask配合Thread

FutureTask能够接收Callable类型的参数，用来处理有返回结果的情况

```java
FutureTask<Integer> task = new FutureTask<>(() -> {
    // 执行的代码
    return // 返回值
});

Thread t = new Thread(task);
t.start();
Integer result = task.get();
System.out.println(r)
```



## 原理

### 栈与栈帧

JVM中由堆、栈、方法区组成，每个线程启动后，虚拟机就会为其分配一块栈内存

- 每个栈由多个栈帧组成，对应着每次方法调用时所占用的内存
- 每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法



### 线程上下文切换（Thread Context Switch）

因为一些原因导致cpu不再执行当前的线程，转而执行另一个线程的代码

- 线程的cpu时间片用完
- 垃圾回收
- 有更高优先级的线程需要运行
- 线程自己调用了sleep、yield、wait、join、park、synchronized、lock等方法

当Context Switch发生时，需要由操作系统保存当前的状态，并恢复另一个线程的状态，Java中对应的概念就是程序计数器，它的作用是记住下一条jvm指令的执行地址，是线程私有的

- 状态包括程序计数器、虚拟机栈中每个栈帧的信息，如局部变量、操作数栈、返回地址等
- Context Switch频繁发生会影响性能



## 常见方法

| 方法名             | 功能说明                                                   | 注意                                                         |
| ------------------ | ---------------------------------------------------------- | ------------------------------------------------------------ |
| start()            | 启动一个新线程，在新的线程运行run方法中的代码              | start方法只是让线程进入就绪，里面的代码不一定立刻运行（CPU的时间片还没有分给他）。每个线程对象的start方法只能调用一次，如果调用多次会出现IllegalThreadStateExceptioin |
| run()              | 新线程启动后会调用的方法                                   | 如果在构造Thread对象时传递了Runnable参数，则线程启动后会调用Runnable中的run方法，否则默认不执行任何操作。但可以创建Thread的子类对象，来覆盖默认行为 |
| join()             | 等待线程运行结束                                           |                                                              |
| join(long n)       | 等待线程运行结束，最多等待n毫秒                            |                                                              |
| getId()            | 获取线程长整型的id                                         | id唯一                                                       |
| getName()          | 获取线程名                                                 |                                                              |
| setName(String n)  | 修改线程名                                                 |                                                              |
| getPriority()      | 获取线程优先级                                             |                                                              |
| setPriority(int n) | 修改线程优先级                                             | java中规定线程优先级是1~10的整数，较大的优先级能提高该线程被CPU调度的几率 |
| getState()         | 获取线程状态                                               | java中线程状态用六个enum表示，分别是：NEW、RUNNABLE、BLICKED、WAITING、TIMED_WAITING、TERMINATED |
| isInterrupted()    | 判断是否被打断                                             | 不会清除打断标记                                             |
| isAlive()          | 线程是否存活（还没有运行完毕）                             |                                                              |
| interrupt()        | 打断线程                                                   | 如果被打断线程正在sleep、wait、join会导致被打断的线程抛出InterruptedException，并清除打断标记，如果打断的正在运行的线程，则会设置打断标记，park的线程被打断，也会设置打断标记 |
| interrupted()      | 判断当前线程是否被打断                                     | 会清除打断标记                                               |
| currentThread()    | 获取当前正在执行的线程                                     |                                                              |
| sleep(long n)      | 让当前执行的线程休眠n毫秒，休眠时让出cpu的时间片给其他线程 |                                                              |
| yield()            | 提示线程调度器让出当前线程对cpu的使用                      | 主要是为了测试和调试                                         |

### sleep

1. 调用sleep会让当前线程从Running进入Timed Waiting状态
2. 其他线程可以使用interrupt方法打断正在睡眠的线程，这时sleep方法会抛出InterruptedException
3. 睡眠状态结束的线程未必会立刻得到执行
4. 建议用TimeUnit的sleep替代Thread的sleep来获得更好的可读性

### yield

1. 调用yield会让当前线程从Running进入Runnable状态，然后调度执行其他优先级的线程，如果这时没有同优先级的线程，那么不能保证让当前线程暂停的效果
2. 具体的实现依赖于操作系统的任务调度器

### 线程优先级

- 线程优先级会提示调度器优先调度该线程，但它仅仅是一个提示，调度器可以忽略他
- 如果cpu较忙，那么优先级较高的线程会获得更多的时间片，但cpu空闲时，优先级几乎没作用

## 防止CPU占用100%

### sleep实现

在没有利用cpu来计算时，不要让while(true)空转浪费cpu，这是可以使用yield或sleep来让出cpu的使用权给其他程序

- 使用wait或条件变量达到类似效果
- 不同的是，后两种需要加锁，并且需要相应的唤醒操作。一般适用于需要同步的场景
- sleep适用于无需锁同步的场景



## 主线程和守护线程

默认情况下，Java进程需要等待所有线程都运行结束，才会结束。有一种特殊的线程叫做守护线程，只要其它非守护线程运行结束了，即使守护线程的代码没有执行完，也会强制结束



## 五种状态

![image-20211030134012218](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211030134012218.png)

- 初始状态：仅是在语言层面创建了线程对象，还未与操作系统线程关联
- 可运行状态：（就绪状态）指该线程已经被创建（与操作系统线程关联），可以由CPU调度执行
- 运行状态：指获取了CPU时间片运行中的状态
  - 当CPU时间片用完，会从运行状态转换至可运行状态，会导致线程的上下文切换
- 阻塞状态：
  - 如果调用了阻塞API，如BIO写文件，这是该线程实际不会用到CPU，会导致线程上下文切换，进入阻塞状态
  - 等BIO操作完毕，会由操作系统唤醒阻塞的线程，切换至可运行状态

##  六种状态

是从Java API层面来描述的

根据Thread.State枚举，分为六种状态

![image-20211030140604948](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211030140604948.png)

- NEW线程刚被创建，但还没有调用start()方法
- RUNNABLE当调用了start()方法后，Java API层面的RUNNABLE状态涵盖了操作系统层面的可运行状态、运行状态和阻塞状态(由BIO导致的线程阻塞，在java是无法区分的，仍然认为是可运行)



# 共享模型之管程

## 共享带来的问题

### 临界区Critical Seetioin

- 一个程序运行多个线程本身是没有问题的
- 问题出在多个线程访问共享资源
  - 多个线程读共享资源其实也没有问题
  - 在多个线程对共享资源读写操作时发生指令交错，就会出现问题
- 一段代码块内如果存在对共享资源的多线程读写操作，称这段代码块为临界区

### 竞态条件Race Condition

多个线程在临界区内执行，由于代码的执行序列不同而导致结果无法预测，称之为发生了竞态条件



## synchronized解决方案

为了避免临界区的竞态条件发生，有多种手段可以达到目的

- 阻塞式的解决方案：synchronized，Lock
- 非阻塞式的解决方案：原子变量

synchronized俗称对象锁，采用互斥的方式让同一时刻至多只有一个线程能持有对象锁，其他线程再想获取这个对象锁时就会阻塞住，这样就能保证拥有锁的线程可以安全的执行临界区内代码，不再担心线程上下文切换

>注意
>
>虽然java中互斥和同步可以采用synchronized关键字来完成，但还是有区别的：
>
>- 互斥锁是保证临界区的竞态条件发生，同一时刻只能有一个线程执行临界区代码
>- 同步是由线程执行的先后、顺序不同、需要一个线程等待其他线程运行到某个点
>

synchronized实际使用**对象锁**保证了**临界区内代码的原子性**，临界区内的代码对外是不可分割的，不会被线程切换所打断

```java
class Test{
    public synchronized void test(){
        
    }
}
// 等价于
class Test{
    public void test(){
        synchronized(this){
            
        }
    }
}
```

```java
class Test{
    public synchronized static void test(){
        
    }
}
// 等价于
class Test{
    public static void test(){
        synchronized(Test.class){
            
        }
    }
}
```



## 变量的线程安全分析

### 成员变量和静态变量是否线程安全

- 如果它们没有共享，则线程安全
- 如果它们共享了，根据它们的状态是否能够改变，分为两种情况
  - 如果只有读操作，则线程安全
  - 如果有写操作，则这段代码是临界区，需要考虑线程安全

### 局部变量是否线程安全

- 局部变量是线程安全的
- 但局部变量引用的对象则未必
  - 如果该对象没有逃离方法的作用范围，他是线程安全的
  - 如果该对象逃离方法的作用范围，需要考虑线程安全

###  常见线程安全类

- String
- Integer
- StringBuffer
- Random
- Vector
- Hashtable
- java.util.concurrent包下的类

这里说它们是线程安全是指，多个线程调用它们同一个实例的某个方法，是线程安全的，也可以理解为

- 它们的每个方法是原子的
- 但注意它们多个方法的组合不是原子的

### 不可变类线程安全性

String、Integer等都是不可变类，因为其内部的状态不可以改变，因此它们的方法都是线程安全的

### Monitor（锁）

Monitor被翻译为监视器或管程

每个java对象都关联一个Monitor对象，如果使用synchronized给对象上锁（重量级）之后，该对象头的Mark Word中就被设置指向Monitor对象的指针

![image-20211031154453948](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211031154453948.png)

- 刚开始Monitor中Owner为null
- 当Thread-2执行到synchronized(obj)就会将Monitor的所有者Owner置为Thread-2，Monitor中只能有一个Owner
- 在Thread-2上锁的过程中，如果Thread-3，Thread-4，Thread-5也来执行synchronized(obj)，就会进入EntryList BLOCKED
- Thread-2执行完同步代码块的内容，然后唤醒EntryList中等待的线程来竞争锁，竞争时是非公平的



## 轻量级锁

轻量级锁的使用场景：如果一个对象虽然有多线程访问，但多线程访问的时间是错开的（也就是没有竞争），那么可以使用轻量级锁来优化

轻量级锁对使用者是透明的，依然使用synchronized

```java
static final Object obj = new Object();
public static void method(){
    synchronized(obj){
		// 同步块A
        method2();
    }
}

public static void method2(){
    synchronized(obj){
        // 同步块B
    }
}
```

- 创建锁记录对象，每个线程的栈帧都会包含一个锁记录的结构，内部可以存储锁定对象的Mark Word

  ![image-20211112095350418](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112095350418.png)

- 让锁记录中Object reference指向锁对象，并尝试用cas替换Object的Mark Word，将Mark Word的值存入锁记录

  ![image-20211112095416209](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112095416209.png)

- 如果cas替换成功，对象头存储了锁记录地址和状态00，表示由该线程给对象加锁

  ![image-20211112100520542](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112100520542.png)

- 如果cas失败，有两种情况

  - 如果是其他线程已经持有了该Object的轻量级锁，这时表示有竞争，进入锁膨胀阶段

  - 如果是自己执行了synchronized锁重入，那么再添加一条Lock Record作为重入的计数

    ![image-20211112100537868](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112100537868.png)

  - 当退出synchronized代码块（解锁时）如果有取值为null的锁记录，表示有重入，这时重置锁记录，表示重入计数减一

    ![image-20211112100932654](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112100932654.png)

  - 当退出synchronized代码块（解锁时）锁记录的值不为null，这时使用cas将Mark Word的值恢复给对象头

    - 成功，则解锁成功
    - 失败，说明轻量级锁进行了锁膨胀或已经升级为重量级锁，进入重量级锁解锁流程

## 锁膨胀

如果在尝试加轻量级锁的过程中，CAS操作无法成功，这时一种情况就是有其他线程为此对象加上了轻量级锁（有竞争），这时需要进行锁膨胀，将轻量级锁变为重量级锁

```java
static Object obj = new Object();
public static void method(){
    synchronized(obj){
        // 同步代码块
    }
}
```

- 当Thread-1进行轻量级加锁时，Thread-0已经对该对象加了轻量级锁

  ![image-20211112102755449](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112102755449.png)

- 这时Thread-1加轻量级锁失败，进入锁膨胀流程

  - 即为Object对象申请Monitor锁，让Object指向重量级锁地址

  - 然后自己进入Monitor的EntryList BLOCKED

    ![image-20211112103745444](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112103745444.png)

- 当Thread-0退出同步代码块解锁时，使用cas将Mark Word的值恢复给对象头，失败。这时会进入到重量级解锁流程，按照Monitor地址找到Monitor对象，设置Owner为null，唤醒EntryList中BLOCKED线程

## 自旋优化

重量级锁竞争的时候，还可以使用自旋来优化，如果当前线程自旋成功（即这时候持锁线程已经退出了同步代码块，释放了锁），这时当前线程就可以避免阻塞

- 在Java6之后自旋锁时自适应的，如果对象刚刚自旋操作成功过，那么认为这次自旋成功的可能性会高，就多自旋几次；反之，则减少自旋的次数甚至不自旋
- 自旋会占用CPU时间，单个CPU自旋是浪费，多核CPU自旋才能发挥优势
- Java7之后不能控制是否开启自旋功能

## 偏向锁

轻量级锁在没有竞争时（就自己这个线程），每次重入仍然需要执行CAS操作

Java6中引入了偏向锁来做进一步优化：只有第一次使用CAS将线程ID设置到对象的Mark Word头，之后发现这个线程ID是自己的就代表没有竞争，不用重新CAS，之后只要不发生竞争，这个对象就归该线程所有

```java
static final Object obj = new Object();
pubic static void m1(){
    synchronized(obj){
        // 同步代码块
        m2();
    }
}

public static void m2(){
    synchronized(obj){
        // 同步代码块
        m3();
    }
}

public static void m3(){
    synchronized(obj){
        // 同步代码块
    }
}
```

### 偏向状态

一个创建创建时：

- 如果开启了偏向锁（默认开启），那么对象创建后，markword值为0x05即最后三位为101，这时它的thread、epoch、age都为0
- 偏向锁默认是延迟的，不会在程序启动时立即生效，如果想避免延迟，可以加VM参数 -XX:BiasedLockingStartupDelay=0来禁用延迟
- 如果没有开启偏向锁，那么对象创建后，markdown值为0x01即最后三位为001，这时它的hashcode、age都为0，第一次用到hashcode时才会创建
- 添加VM参数-XX: -UseBiasedLocking可以禁用偏向锁

### 撤销偏向锁-调用对象的hashCode

调用了对象的hashcode，但偏向锁的对象MarkWord中存储的是线程id，如果调用hashcode会导致偏向锁被撤销

- 轻量级锁会在锁记录中记录hashcode
- 重量级锁会在Monitor中记录hashcode



### 撤销偏向锁-其他线程使用对象

当有其他线程使用偏向锁对象时，会将偏向锁升级为轻量级锁

### 撤销偏向锁-调用wait/notify



### 批量重偏向

如果对象虽然被多个线程访问，但是没有竞争，这时偏向了T1的对象仍有机会重新偏向T2，重偏向会重置对象的Thread ID

当撤销偏向锁阈值超过20次后，JVM会在给这些对象加锁时重新偏向至加锁线程



### 批量撤销

当撤销偏向锁阈值超过40次后，jvm会觉得自己偏向错了，根本就不应该偏向，于是整个类的所有对象都会变为不可偏向的，新建的对象也是不可偏向的 





## wait/notify

![image-20211031154453948](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211031154453948.png)

- Owner线程发现条件不满足，调用wait方法，即可进入WaitSet变为WAITING状态
- BLOCKED和WAITING的线程都处于阻塞状态，不占用CPU时间片
- BLOCKED线程会在Owner线程释放锁时唤醒
- WAITING线程会在Owner线程调用notify或notifyAll时唤醒，但唤醒后并不意味着立刻获得锁，仍需进行EntryList重新竞争

### API

- obj.wait() 让进入object监视器的线程到waitSet等待
- obj.notify() 在object上正在waitSet等待的线程中挑一个唤醒
- obj.notifyAll() 让object上正在waitSet等待的线程全部唤醒

它们都是线程之间进行协作的手段，都属于Object对象的方法。必须获得此对象的锁，才能调用这几个方法



wait()方法会释放对象的锁，进入WaitSet等待区，从而让其他线程就有机会获取对象的锁。无限制等待，知道notify为止

wait(long n)有时限的等待，到n毫秒之后结束等待，或是被notify

### sleep和wait的区别

- sleep是Thread方法，wait是Object的方法
- sleep不需要强制和synchronized配合使用，但wait需要和synchronized一起用
- sleep在睡眠的同时，不会释放对象锁，但wait在等待时会释放对象锁

 

# 同步模式之保护性暂停

## 定义

即Guarded Suspension，用在一个线程等待另一个线程的执行结果

- 有一个结果需要从一个线程传递到另一个线程，让它们关联同一个GuardedObject
- 如果有结果不断从一个线程到另一个线程那么可以使用消息队列
- JDK中，join的实现、Future的实现，采用的就是此模式
- 因为要等待另一方的结果、因此归类到同步模式

![image-20211112205034526](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211112205034526.png)

# 异步模式之生产者/消费者

- 与前面的保护性暂停不同，不需要产生结果和消费者线程一一对应
- 消费者队列可以用来平衡生产和消费的线程资源
- 生产者仅负责产生结果数据，不关心数据该如何处理，而消费者专心处理结果数据
- 消息队列是有容量限制的，满时不会再加入数据，空时不会再消耗数据
- JDK中各种阻塞队列，采用的就是这种模式

![image-20211113091920951](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211113091920951.png)





# park和unpark

是LockSupport类中的方法

```java
// 暂停当前线程
LockSupport.park();

// 恢复某个线程的运行
LockSupport.unpark(暂停线程对象)
```



与Object的wait & notify相比

- wait，notify和notifyAll必须配合Object Monitor一起使用，而park、unpark不必
- park & unpark是以线程为单位来阻塞和唤醒线程，而notify只能随机唤醒一个等待线程，notifyAll是唤醒所有等待线程，不那么精确
- park & unpark可以先unpark，而wait & notify不能先notify

![image-20211113100528378](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211113100528378.png)

1. 当前线程调用Unsafe.park()方法
2. 检查 _counter，本情况为0，这时，获得 _mutex互斥锁
3. 线程进入 _cond条件变量阻塞
4. 设置 _counter=0

![image-20211113101256554](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211113101256554.png)

1. 调用Unsafe.unpark(Thread-0)方法，设置 _counter为1
2. 当前线程调用Unsafe.park()方法
3. 检查 _counter，本情况为1，这时线程无需堵塞，继续运行
4. 设置 _counter为0





# 线程状态切换

假设有线程Thread t

## 情况1 NEW --> RUNNABLE

- 当调用t.start()方式时，有 NEW --> RUNNABLE

## 情况2 RUNNABLE <--> WAITING

t线程用synchronized(obj)获取了对象锁后

- 调用object.wait()方法，t线程从 RUNNABLE --> WAITING
- 调用object.notify(), object.notifyAll(), t.interrupt()时
  - 竞争锁成功，t线程从 WAITING --> RUNNABLE
  - 竞争锁失败，t线程从 WAITING --> BLICKED

## 情况3 RUNNABLE <--> WAITING

- 当前线程调用 t.join()方法时，当前线程从 RUNNABLE --> WAITING
  - 注意是当前线程在 t 线程对象的监视器上等待
- t 线程运行结束，或调用了当前线程的 interrupt()时，当前线程从 WAITING --> RUNNABLE



## 情况4 RUNNABLE <--> WAITING

- 当前线程调用 LockSupport.park()方法会让当前线程从 RUNNABLE --> WAITING
- 调用 LockSupport.unpark(目标线程)或调用了线程的 interrupt()，会让目标线程从 WAITING --> RUNNABLE

## 情况5 RUNNABLE --> TIMED_WAITING

t 线程用synchronized(obj)获取了对象锁后

- 调用obj.wait(long n)方法时，t 线程从RUNNABLE --> TIEMD_WAITING
- t 线程等待时间超过了n毫秒，或调用obj.notify()，obj.notifyAll()，t.interrupt()时
  - 竞争锁成功，t 线程从 TIEMD_WAITING --> RUNNABLE
  - 竞争锁失败，t 线程从 TIMED_WAITING --> BLOCKED



## 情况6 RUNNABLE <--> TIMED_WAITING

- 当前线程调用 t.join(long n)方法时，当前线程从RUNNABLE --> TIEMD_WAITING
  - 注意是当前线程在 t 线程对象的监视器上等待
- 当前线程等待时间超过了 n 毫秒，或t 线程运行结束，或调用了当前线程的 interrupt()时，当前线程从 TIMED_WATING --> RUNNABLE

## 情况7 RUNNABLE <--> TIMED_WAITING

- 当前线程调用Thread.sleep(long n)，当前线程从 RUNNEABLE --> TIMED_WAITING
- 当前线程等待时间超过了n毫秒，当前线程从TIMED_WAITING --> RUNNABLE





## 情况8 RUNNABLE <--> TIEMD_WAITING

- 当前线程调用LockSupport.parkNanos(long nanos)或LockSupport.parkUntil(long millis)时，当前线程从 RUNNABLE --> TIMED_WAITING
- 调用LockSupport.unpark(目标线程)或调用了线程的 interrupt()，或是等待超时，会让目标线程从TIMED_WAITING --> RUNNABLE



## 情况9 RUNNABLE <--> BLOCKED

- t 线程用 synchronized(obj)获取了对象锁时如果竞争失败，从 RUNNABLE --> BLOCKED
- 持obj锁线程的同步代码块执行完毕，会唤醒该对象上所有 BLOCKED的线程重新竞争，如果其中 t线程竞争成功，从 BLOCKED --> RUNNABLE，其他失败的线程仍然 BLOCKED

## 情况10 RUNNABLE <--> TERMINATED

当前线程所有代码运行完毕，进入 TERMINATED





# 多锁

将锁的颗粒度细分

- 好处：可以增强并发度
- 坏处：如果一个线程需要同时获得多把锁，就容易发生死锁

# 活跃性

## 死锁

一个线程需要同时获取多把锁，就容易发生死锁

t1 线程获得A对象锁，接下来想获取B对象的锁

t2 线程获得B对象锁，接下来想获取A对象的锁



## 活锁

活锁出现在两个线程互相改变对方的结束条件，最后谁也无法结束

## 饥饿

一个线程由于优先级太低，始终得不到CPU调度执行，也不能够结束





# ReentrantLock

相较于synchronized有几个特点：

- 可中断
- 可以设置超时时间
- 可以设置为公平锁
- 支持多个条件变量

与synchronized一样，支持可重入



基本语法：

```java
// 获取锁
reentrantLock.lock();
try{
    // 临界区
} finally{
    reentrantLocl.unlock();
}
```



## 可重入

可重入是指同一个线程如果首次获得了这把锁，那么因为它是这把锁的拥有者，因此有权利再次获取这把锁，如果是不可重入锁，那么第二次获得锁时，自己也会被锁挡住



## 条件变量

ReentrantLock的条件变量比synchronized强大之处在于，它是支持多个条件变量的



# 前面内容总结

- 分析多线程访问共享资源时，哪些代码片段属于临界区
- 使用synchronized互斥锁解决临界区的线程安全问题
  - 掌握synchronized锁对象语法
  - 掌握synchronized加载成员方法和静态方法语法
  - 掌握wait/notify同步方法
- 使用lock互斥锁解决临界区的线程安全问题
  - 掌握lock的使用细节：可打断、锁超时、公平锁、条件变量
- 分析变量的线程安全性、掌握常见线程安全类的使用
- 了解线程活跃性的问题：死锁、活锁、饥饿
- 应用：
  - 互斥：使用synchronized或Lock达到共享资源互斥效果
  - 同步：使用wait/notify或Lock的条件变量来达到线程间通信效果
- 原理
  - monitor、synchronized、wait/notify原理
  - synchronized进阶 原理
  - park & unpark原理
- 模式
  - 同步模式之保护性暂停
  - 异步模式之生产者消费者
  - 同步模式之顺序控制

# 共享模型之内存

## Java内存模型

JMM即Java Memory Model，定义了主存、工作内存抽象概念，底层对应着CPU寄存器、缓存、硬件内存、CPU指令优化等

JMM体现在以下几个方面

- 原子性：保证指令不会受到线程上下文切换的影响
- 可见性：保证指令不会受到cpu缓存影响
- 有序性：保证指令不会受cpu指令并行优化的影响

## volatile

volatile 易变关键字

用来修饰成员变量和静态成员变量，可以避免线程从自己的工作缓存中查找变量的值，必须到主存中获取它的值，线程操作volatile变量都是直接操作主存，即一个线程对volatile变量的修改，对另一个线程可见

> volatile仅仅保证了共享变量的可见性，让其他线程能够看到最新值，但不能解决指令交错（不能保证原子性）



## 可见性

可见性保证的是多个线程之间，一个线程对volatile变量的修改对另一个线程可见，不能保证原子性，仅用在一个写线程、多个多线程的情况

> synchronized语句块即可以保证代码块的原子性，也同时保证代码块内变量的可见性。synchronized是属于重量级操作，性能相对更低

## 有序性

JVM会在不影响正确的前提下，可以调整语句的执行顺序

## volatile原理

volatile的底层实现原理是内存屏障，Memory Barrier（Memory Fence）

- 对volatile变量的写指令后会加入写屏障
- 对volatile变量的读指令前会加入读屏障





写屏障：保证在该屏障之前的，对共享变量的改动，都同步到主存当中

读屏障：保证在该屏障之后，对共享变量的读取，加载的是主存中最新数据

# 共享模型之无锁

## CAS与volatile

CAS必须借助volatile才能读取到共享变量的最新值来实现**比较并交换**的效果

## 为什么无锁效率高

- 无锁情况下，即使重试失败，线程始终在高速运行，没有停歇，而synchronized会让线程在没有获得锁的时候，发生上下文切换，进入阻塞
- 无锁情况下，因为线程要保持运行，需要额外CPU的支持。如果没有额外的CPU，虽然不会进入阻塞，但由于没有分到时间片，仍然会进入到可运行状态，还是会导致上下文切换

## CAS特点

CAS适合于线程数少、多核CPU的场景

- CAS是基于乐观锁的思想：最乐观的估计，不怕别的线程来修改共享变量，就算改了也没关系，进行重试
- synchronized是基于悲观锁的思想：最悲观的估计，需要防着其他线程来修改共享变量
- CAS体现的是无锁并发、无阻塞并发
  - 因为没有使用synchronized，所以线程不会陷入阻塞
  - 如果竞争激烈，重试会频繁发生，从而影响效率



# ThreadPoolExecutor

## 线程状态

ThreadPoolExecutor使用 int 的高3位来表示线程池状态，低29位表示线程数量

| 状态名     | 高3位 | 接收新任务 | 处理阻塞队列任务 | 说明                                     |
| ---------- | ----- | :--------: | :--------------: | ---------------------------------------- |
| RUNNING    | 111   |     Y      |        Y         |                                          |
| SHUTDOWN   | 000   |     N      |        Y         | 不会接收新任务，但会处理阻塞队列剩余任务 |
| STOP       | 001   |     N      |        N         | 会中断正在执行的任务，并抛弃阻塞队列任务 |
| TIDYING    | 010   |     -      |        -         | 任务执行完毕，活动线程位0即将进入终结    |
| TERMINATED | 011   |     -      |        -         | 终结状态                                 |

这些信息存储在一个原子变量 ctl 中，目的是将线程池状态与线程个数合二为一，这样就可以用一次 cas 原子操作进行赋值

## 构造方法

```java
public ThreadPoolExecutor(int corePoolSize,
                         int maximumPoolSize,
                         long keepAliveTime,
                         TimeUnit unit,
                         BlockingQueue<Runnable> workQueue,
                         ThreadFactory threadFactory,
                         RejectedExecutionHandler handler)
    /* corePoolSize 核心线程数目（最多保留的线程数）
       maximumPoolSize 最大线程数目
       keepAliveTime 生存时间-针对救急线程
       unit 时间单位-针对救急线程
       workQueue 阻塞队列
       threadFactory 线程工厂-可以为线程创建时起名字
       handler 拒绝策略
    */
    
```

- 线程池中刚开始没有线程，当一个任务提交给线程池后，线程池会创建一个新线程来执行任务
- 当线程数达到corePoolSize并没有线程空闲，这时再加入任务，新加的任务会被加入workQueue队列排队，直到有空闲的线程
- 如果队列选择了有界队列，那么任务超过了队列大小时，会创建maximumPoolSize - corePoolSize数目的线程来救急
- 如果线程达到maximumPoolSize仍然有新任务这时会执行拒绝策略，拒绝策略jdk提供了4种实现
  - AbortPolicy 让调用者抛出 RejectedExecutionException异常，这时默认策略
  - CallerRunsPolicy 让调用者运行任务
  - DiscardPolicy 放弃本次任务
  - DiscardOldestPolicy 放弃任务队列中最早的任务，本任务取而代之
  - Dubbo 的实现，在抛出 RejectedExecutionException 异常之前记录日志，并 dump 线程栈信息
  - Netty 的实现，创建一个新线程来执行任务
  - ActiveMQ 的实现，带超时等待（60s）尝试放入队列
  - Pinpoint 的实现，使用一个拒绝策略链，逐一尝试拒绝策略链中每种拒绝策略
- 当高峰过去后，超过了corePoolSize的救急线程如果一段时间没有任务做，需要结束节省资源，这个时间由keepAliveTime和 unit 来控制

## newFixedThreadPool

```java
public static ExecutorService newFixedThreadPool(int nThreads){
    return new ThreadPoolExecutor(nThreads, nThreads,
                                 0L, TimeUnit.MILLISECONDS,
                                 new LinkedBlockingQUeue<Runnable>())
}
```

特点

- 核心线程数 == 最大线程数（没有救急线程被创建），因此也无需超时时间
- 阻塞队列是无界的，可以放任意数量的任务

> 适用于任务量已知，相对耗时的任务



## newCacheThreadPool

```java
public static ExecutorService newCacheThreadPool(){
    return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                 60L, TimeUnti.SECONDS,
                                 new SynchronousQueue<Runnable>())
}
```

特点：

- 核心线程数是0，最大线程数是 Integer.MAX_VALUE，救急线程的空闲时间是60s，意味着
  - 全部都是救急线程（60s后可以回收）
  - 救急线程可以无限创建
- 队列采用了SynchronousQueue实现特点是，它没有容量，没有线程来取是放不出去的

> 整个线程池表现为线程数会根据任务量不断增长，没有上限，当任务执行完毕，空闲1分钟后释放线程。
>
> 适合任务数比较密集，但每个任务执行时间较短的情况

## newSingleThreadExecutor

```java
public static ExecutorService newSingleThreadExecutor(){
    return new FinalizableDelegatedExecutorService(new ThreadPoolExecutor(
        															1, 1, 0L,
                                                               TimeUnit.MILLISECONDS, 
                                                       new LinkedBlockingQueue<Runnable>()))
}
```

使用场景：

希望多个任务排队执行。线程数固定为1，任务数多于1时，会放入无界队列排队，任务执行完毕，这唯一的线程也不会被释放

区别：

- 自己创建一个单线程串行执行任务，如果任务执行失败而终止那么没有任何补救措施，而线程池还会新建一个线程，保证池的正常工作
- Executors.newSingleThreadExecutor()线程个数始终为1，不能修改
  - FinalizableDelegatedExecutorService 应用的是装饰器模式，只对外暴漏了 ExecutorService接口，因此不能调用 ThreadPoolExecutor 中特有的方法
- Executors.newFixedThreadPool(1)初始值为1，以后还会修改
  - 对外暴露的是ThreadPoolExecutor 对象，可以强转后调用 setCorePoolSize 等方法进行修改

## 提交任务

```java
// 执行任务
void execute(Runnable command);

// 提交任务task，用返回值 Future 获得任务执行结果
<T> Future<T> submit(Callable<T> task);

// 提交tasks中所有任务
<T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks) throws InterruptedException;
    
// 提交tasks中所有任务，带超时时间
<T> List<Future<T>> invokeAll(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit) throws InterruptedException;

// 提交tasks中所有任务，哪个任务先成功执行完毕，返回此任务执行结果，其他任务取消
<T> T invokeAny(Collection<? extends Callable<T>> tasks) throws InterruptedException, ExecutionException;

// 提交tasks中所有任务，哪个任务先成功执行完毕，返回此任务执行结果，其他任务取消，带超时时间
<T> T invokeAny(Collection<? extends Callable<T>> tasks, long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
```



## 关闭线程池

shutdown

```java
/*
线程池状态变为SHUTDOWN
不会接受新任务
但已提交的任务会执行完
此方法不会阻塞调用线程的执行
*/
void shutdown();
```

shutdownNow

```java
/*
线程池状态变为STOP
不会接收新任务
会将队列中的任务返回
并用interrupt的方式中断正在执行的任务
*/
List<Runnable> shutDownNow();
```

## 其他方法

```java
// 不在 RUNNING 状态的线程池，此方法就返回 true
boolean isShutdown();

// 线程池状态是否是 TERMINATED
boolean isTerminated();

// 调用shutdown后，由于调用线程并不会等待所有任务运行结束，因此如果它想在线程池 TERMINATED 后做些事情，可以利用此方法等待
boolean awaitTermination(long timeout, TimeUnit unit) throws InterruptedException;
```



## Fork/Join

## 概念

Fork/Join是 JDK1.7 加入的新的线程池实现，体现的是一种分治思想，适用于能够进行任务拆分的cpu密集性运算

所谓的任务拆分，是将一个大任务拆分为算法上相同的小任务、直至不能拆分可以直接求解。

Fork/Join 在分治的基础上加入了多线程，可以把每个任务的分解和合并交给不同的线程来完成，进一步提升了运算效率

Fork/Join 默认会创建与CPU核心数大小相同的线程池



## 使用

提交给Fork/Join 线程池的任务需要继承 RecursiveTask(有返回值) 或 RecursiveAction(没有返回值)







# AQS原理

全称是AbstractQueuedSynchronizer，是阻塞式锁和相关的同步器工具的框架

特点：

- 用 state 属性来表示资源的状态（分独占模式和共享模式），子类需要定义如何维护这个状态，控制如何获取锁和释放锁
  - getState 获取 state 状态
  - setState 设置 state 状态
  - compareAndSetState 乐观锁机制设置 state 状态
  - 独占模式是只有一个线程能够访问资源，而共享模式可以允许多个线程访问资源
- 提供了基于 FIFO 的等待队列，类似于 Monitor 的 EntyrList
- 条件变量来实现等待、唤醒机制，支持多个条件变量，类似 Monitor 的 WaitSet

子类主要实现的一些方法（默认抛出 UnsupportedOperationExceptioni）

- tryAcquire
- tryRelease
- tryAcquireShared
- tryReleaseShared
- isHeldExclusively

获取锁

```java
// 如果获取锁失败
if(!tryAcquire(arg)) {
    // 入队，可以选择阻塞当前线程
}
```

释放锁

```java
// 如果释放锁成功
if (tryRelease(arg)) {
    // 让阻塞x
}
```





# 读写锁

## ReentrantReadWriteLock

当读操作远远高于写操作时，使用读写锁可以让 读 - 读 可以并发，提高性能



## 注意事项

- 读锁不支持条件变量
- 重入时升级不支持：即持有读锁的情况下去获取写锁，会导致获取写锁永久等待
- 重入时降级支持：即持有写锁的情况下去获取读锁





## StampedLock

进一步优化读性能，特点是在使用读锁、写锁时必须配合 戳 使用

加解读锁

```java
long stamp = lock.readLock();
lock.unlockRead(stamp);
```

加解写锁

```java
long stamp = lock.writeLock();
lock.unlockWrite(stamp);
```

乐观读，StampedLocl支持 tryOptimisticRead() 方法（乐观读），读取完毕后需要再做一次 戳校验 如果校验通过，表示这期间确实没有写操作，数据可以安全使用，如果检验失败，需要重新获取读锁，保证数据安全

```java
long stamp = lock.tryOptimisticRead();
// 校验戳
if(!lock.validate(stamp)){
    // 锁升级
}
```

> 注意
>
> - StampedLock 不支持条件变量
> - StampedLock 不支持可重入



# Semaphore

信号量，用来限制能同时共享资源的线程上限

## Semaphore应用

- 使用Semaphore限流，在访问高峰时，让请求线程阻塞，高峰期过去再释放许可，只适合限制单机线程数量，并且仅是限制线程数，而不是限制资源数
- 用Semaphore实现简单连接池，对比 享元模式 的实现，性能和可读性显然更好







# JUC线程安全集合类

## Blocking、CopyOnWrite、Concurrent

- Blocking大部分实现基于锁，并提供用来阻塞的方法
- CopyOnWrite之类容器修改开销相对较重
- Concurrent类型的容器
  - 内部很多操作使用cas优化，可以提供较高吞吐量
  - 弱一致性
    - 遍历时弱一致性
    - 求大小弱一致性
    - 读取弱一致性
