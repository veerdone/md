# JVM

Java Virtual Machine java程序的运行环境(java二进制字节码的运行环境)

好处

- 一次编写，到处运行
- 自动内存管理，垃圾回收功能

![JVM](https://gitee.com/ziYu_74751/picture/raw/master/images/JVM.png)

## 相关VM参数

| 含义             | 参数                                                         |
| ---------------- | ------------------------------------------------------------ |
| 堆初始大小       | -Xms                                                         |
| 堆最大大小       | -Xmx或-XX:MaxHeapSize=size                                   |
| 新生代大小       | -Xmn或(-XX:NewSize=size + -XX:MaxNewSize=size)               |
| 幸存区比例(动态) | -XX:InitialSurvivorRatio=ratio 和 -XX:+UseAdaptiveSizePolicy |
| 幸存区比例       | -XX:SurvivorRatio=ratio                                      |
| 晋升阈值         | -XX:MaxTenuringThreshold=threshold                           |
| 晋升详细         | -XX:+PrintTenuringDistribution                               |
| GC详细           | -XX:+PrintGCDetails -verbose:gc                              |
| FullGC前MinorGC  | -XX: +ScavegeBeforFullGC                                     |



# 内存结构

## 程序计数器

Program Counter Registere程序计数器(寄存器)

作用：记住下一条jvm指定的执行地址

特点：

- 线程私有
- 不会存在内存溢出

## 虚拟机栈

Java Virtual Machine Stacks(Java虚拟机栈)

- 每个线程运行时所需要的内存，称为虚拟机栈
- 每个栈由多个栈帧(Frame)组成，对应着每次方法调用所占用的内存
- 每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法

### 栈内存溢出

- 栈帧过多导致栈内存溢出
- 栈帧过大导致栈内存溢出

## 本地方法栈

作用：为本地方法的调用提供内存空间



## Heap堆

通过new关键字，创建的对象都在堆内存

特点：

- 是线程共享的，堆内存中对象需要考虑线程安全的问题
- 有垃圾回收机制

## 方法区

### 组成

![image-20211016204408281](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211016204408281.png)

![image-20211016204353726](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211016204353726.png)



### 运行时常量池

- 常量池，就是一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等信息
- 运行时常量池，常量池是*.class文件中的，当该类被加载，它的常量池信息就会放入运行时常量池，并把里面的符号地址变为真实地址

### StringTable特性

- 常量池中的字符串仅是符号，第一次用到时才会变为对象
- 利用串池的机制，来避免重复创建字符串对象
- 字符串变量拼接的原理是StringBuilder(1.8)
- 字符串常量拼接的原理是编译器优化
- 可以使用intern方法，主动将串池中还没有的字符串对象放入串池
  - 1.8将这个字符串对象尝试放入串池，如果有则不放入，如果没有则放入，会把串池中的对象返回
  - 1.6将这个字符串对象尝试放入串池，如果有则不放入，如果没有则把此对象复制一份，放入串池，把串池中的对象返回

### StringTable 垃圾回收



## 直接内存

### 定义

Direct Memory

- 常见于NIO操作时，用于数据缓冲区
- 分配回收成本较高，但读写性能高
- 不受JVM内存回收管理

### 分配和回收原理

- 使用了Unsafe对象完成直接内存的分配回收，并且回收需要主动调用freeMemory方法
- ByteBuffer的实现类内部，使用了Cleaner(虚引用)来监测ByteBuffer对象，一旦ByteBuffer对象被垃圾回收。就会由ReferenceHandler线程通过Cleaner的clean方法调用freeMemory来释放直接内存

# 垃圾回收

## 如何判断对象可以回收

### 引用计数法

### 可达性分析算法

- Java虚拟机中的垃圾回收器采用可达性分析来探索所有存活的对象
- 扫描堆中的对象，看是否能够沿着GC Root对象为起点的引用链找到对象，找不到，表示可以回收
  - 引用方式
    - 强引用
    - 软引用
    - 弱引用
    - 虚引用
    - 终结器引用

## 垃圾回收算法

### 标记清除

Mark Sweep

- 速度较快
- 会造成内存碎片

![image-20211017145702829](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017145702829.png)

  ### 标记整理

Mark Compact

- 速度慢
- 没有内存碎片

![image-20211017150100317](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017150100317.png)

### 复制

copy

- 不会有内存碎片
- 需要占用双倍内存空间

![image-20211017150635620](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017150635620.png)

## 分代垃圾回收

![image-20211017154025096](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017154025096.png)

- 对象首先分配在伊甸园区域
- 新生代空间不足，触发minor gc，伊甸园和from存活的对象使用copy复制到to中，存活的对象年龄加1并且交换from to
- minor gc会引发stop the world，暂停其他用户的线程，等垃圾回收结束，用户线程才能恢复运行
- 当对象寿命超过阈值时，会晋升至老年代，最大寿命是15(4bit)

# 垃圾回收器

1. 串行
   1. 单线程
   2. 堆内存较小，适合个人电脑
2. 吞吐量优先
   1. 多线程
   2. 堆内存较大，多核CPU
   3. 让单位时间内，STW的时间最短
3. 响应时间优先
   1. 多线程
   2. 堆内存较大，多核CPU
   3. 尽可能让单次STW的时间最短

## 串行

-XX:+UseSerialGC=Serial+SerialOld

![image-20211017182505575](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017182505575.png)

## 吞吐量优先

-XX:+UseParallelGC~ -XX:+UseParallelOldGC

-XX:+UseAdaptiveSizePolicy

-XX:GCTimeRatio=ration

-XX:MaxGCPauseMillis=ms

-XX:ParallelGCThreads=n

![image-20211017183256103](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017183256103.png)

## 响应时间优先

-XX:+UseConcMarkSweepGC ~ -XX:+UseParNewGC ~ SerialOld

-XX:ParallelGCThreads=n ~ -XX:ConcGCThreads=threads

-XX:CMSInitiatingOccupancyFraction=percent

-XX:+CMSScavengeBeforeRemark

![image-20211017190543986](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211017190543986.png)

## G1

Garbage First

使用场景：

- 同时注重吞吐量和低延迟，默认的暂时目标是200ms
- 超大堆内存，会将堆划分为多个大小相等的Region
- 整体上是标记+整理算法，两个区域之间是复制算法

相关JVM参数

-XX:+UseG1GC

-XX:G1HeapRegionSize=size

-XX:MaxGCPauseMillis=time

![image-20211018080114498](https://gitee.com/ziYu_74751/picture/raw/master/images/image-20211018080114498.png)

### Young Collection

- 会STW

### Young Collection + CM

- 在Young GC是会进行GC Root的初始标记
- 老年代占用堆内存比例达到阈值时，进行并发标记(不会STW)，由下面的JVM参数决定

-XX:InitiatingHeapOccupancyPercent=percent(默认45%)

### Mixed Collection

会对伊甸园、幸存区、老年代进行全方面垃圾回收

- 最终标记(Remark)会STW
- 拷贝存活(Evacuation)会STW

-XX:MaxGCPauseMillis=ms

## Full GC

- SerialGC
  - 新生代内存不足发生的垃圾收集-minor gc
  - 老年代内存不足发生的垃圾收集-full gc
- ParallelGC
  - 新生代内存不足发生的垃圾收集-minor gc
  - 老年代内存不足发生的垃圾收集-full gc
- CMS
  - 老年代内存不足发生的垃圾收集-minor gc
  - 老年代内存不足
- G1
  - 新生代内存不足发生的垃圾收集-minor gc
  - 老年代内存不足







# 类加载和字节码技术

## 类加载阶段

### 加载

- 将类的字节码载入方法区，内部采用C++的instanceKlass描述类，重要的field有：
  - _java_mirror即java的类镜像，作用是把klass暴露给java是使用
  - _super即父类
  - _fields即成员变量
  - _methods即方法
  - _constants即常量池
  - _class_loader即类加载器
  - _vtable虚方法表
  - _itable接口方法表
- 如果这个类还有父类没有加载，先加载父类
- 加载和链接可能是交替运行的

> - instanceKlass这样的元数据是存储在方法区（1.8后的元空间内），但_java_mirror是存储在堆中

### 链接

验证：验证类是否符合JVM规范，安全性检查

准备：为static变量分配空间，设置默认值

- static变量在JDK7之前存储于instanceKlass末尾，从JDK7开始，存储于_java_mirror末尾
- static变量分配空间和赋值是两个步骤，分配空间在准备阶段完成，赋值在初始化阶段完成
- 如果static变量是final的基本类型，那么编译阶段值就确定了，赋值在准备阶段完成
- 如果static变量是final的引用类型，那么赋值也会在初始化阶段完成

解析：将常量池中的符号引用解析为直接引用

### 初始化

<cinit>()v方法

初始化即调用<cinit>()v，虚拟机会保证这个类的构造方法的线程安全

类初始化是**懒惰的**

- main方法所在的类，总会被首先初始化
- 首次访问这个类的静态变量或静态方法时
- 子类初始化，如果父类还没初始化，会引发
- 子类访问父类的静态变量，只会触发父类的初始化
- Class.forName
- new会导致初始化

不会导致类初始化的情况

- 访问类的static final静态变量（基本类型和字符串）不会触发初始化
- 类对象.class不会触发初始化
- 创建该类的数组不会触发初始化
- 类加载器的loadClass方法
- Class.forName的参数2为false时

# 类加载器

| 名称                    | 加载哪些类            | 说明                       |
| ----------------------- | --------------------- | -------------------------- |
| Bootstrap ClassLoader   | JAVA_HOME/jre/lib     | 无法直接访问               |
| Extension ClassLoader   | JAVA_HOME/jre/lib/ext | 上级为Boostrap，显示为null |
| Application ClassLoader | classpath             | 上级为Extension            |
| 自定义类加载            | 自定义                | 上级为Application          |

### 自定义类加载器

什么时候需要

- 想加载非classpath随意路径的类文件
- 都是通过接口来使用实现，希望解耦时，常用在框架设计
- 这些类希望予以隔离，不同应用的同类名都可以加载，不冲突，常见于tomcat容器

步骤

- 继承ClassLoader父类
- 要遵从双亲委派机制，重写findClass方法
  - 不是重写loadClass方法，否则不会走双亲委派机制
- 读取类文件的字节码
- 调用父类的defineClass方法来加载类
- 使用者调用该类加载器的loadClass方法

## 分层编译

JVM将执行状态分成了5个层次

- 0层，解释执行（Interpreter）
- 1层，使用C1即时编译器编译执行（不带profiling）
- 2层，使用C1即时编译器编译执行（带基本的profiling）
- 3层，使用C1即时编译器编译执行（带完全的profiling）
- 4层，使用C2即使编译器编译执行

> profiling是指在运行过程中收集一些程序执行状态的数据，如方法的调用次数，循环的回边次数等

即使编译器（JTI）与解释器的区别

- 解释器是将字节码解释为机器码，下次即使遇到相同的字节码，仍会执行重复的解释
- JIT是将一些字节码编译为机器码，并存入Code Cache，下次遇到相同的代码，直接执行，无需再编译
- 解释器是将字节码解释为针对所有平台都通用的机器码
- JIT会根据平台类型，生成平台特定的机器码

对于占据大部分的不常用的代码，无需耗费时间将其编译为机器码，而是采取解释执行的方法执行，另外一方面，对于仅占据小部分的热点代码，我们则可以将其编译为机器码，以达到理想的运行速度。执行效率上Interpreter< C1 < C2 ，总的目标是发现热点代码，进行优化。称为**逃逸分析**。

## 方法内联

如果发现一个方法是热点方法，并且长度不太长时，会进行内联，所谓的内联就是把方法内代码拷贝，粘贴到调用者的位置



# JMM 内存模型

JMM是Java Memory Model的意思，即java内存模型，定义了一套在多线程读写共享数据时（成员变量、数组）时，对数据的可见性、有序性和原子性的规则和保障



## volatile(易变关键字)

用来修饰成员变量和静态成员变量，可以避免线程从自己的工作缓存中查找变量的值，必须到主存中获取它的值，线程操作volatile变量都是直接操作主存

## 可见性

保证的是多个线程之间，一个线程对volatile变量的修改对另一个线程可见，不能保证原子性，仅用在一个写线程，多个读线程的情况



## happens-before

happens-before规定了哪些写操作对其他线程的读操作可见，它是可见性与有序性的一套规则总结：

- 线程解锁m之前对变量的写，对于接下来对m加锁的其他线程对该变量的读可见
- 线程对volatile变量的写，对接下来其他线程对该变量的读可见
- 线程start前对变量的写，对该线程开始后对该变量的读可见
- 线程结束前对变量的写，对其他线程得知它结束后的读可见
- 线程t1打断t2前对变量的写，对于其他线程得知t2被打断后对变量的读可见
- 对变量默认值的写，对其他线程对该变量的读可见





# CAS与原子类

## CAS

CAS即Compare and Swap，它体现的一种乐观锁的思想

## 乐观锁和悲观锁

synchronized是基于悲观锁的思想，最悲观的估计

## 原子操作类

juc中提供了原子操作类，可以提供线程安全的操作，例如：AtomicInteger、AtomicBoolean，底层采用的是CAS技术+volatile来实现



## 轻量级锁

如果一个对象虽然有多线程访问，但多线程访问的时间是错开的（也就是没有竞争），那么可以使用轻量级锁来优化
