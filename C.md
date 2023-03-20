# C

## 变量

定义格式：数据类型 名称 = 值;

## 指针

定义格式：指针类型 *名称 = &变量

```c
printf("ptr = %p", ptr) // 输出的是指针指向的地址值
printf("*ptr = %d", *ptr) // 输出的是指针指向的值
printf("&ptr = %p", &ptr) // 输出的是指针变量的地址值
```

## 常量

两种定义方式

1. #define 常量名 值
2. const 常量类型 常量名 = 值

const和#define的区别：

1. const定义的常数带类型，#define不带类型
2. const是在编译、运行的时候起作用，而#define是在编译的预处理阶段起作用
3. #define只是简单的替换，没有类型检查，简单的字符串替换会导致边界效应
4. const常量可以进行调试，#define是不能进行调试的，主要是预编译阶段就已经替换掉了，调试的时候就没有他了
5. const不能重定义，不可以定义两个一样的，而#define通过undef取消某个符号的定义，再重新定义
6. #define可以配合#ifdef、#ifndef、#endif来使用，可以让代码更加灵活

## 枚举

```c
enum 枚举名{
    枚举变量
}
// 使用枚举
enum 枚举变量名 名称;

// 一次性枚举
enum 枚举名{
    枚举变量
} 名称;
```

## 头文件

头文件是拓展名为.h的文件，包含了函数声明和宏定义

include <> 和 include "" 的区别

1. include<> 引用的是编译器的类库路径里面的头文件，用于引用系统头文件
2. include"" 引用的是程序所在的相对路径中的头文件，如果在程序目录没有找到引用的头文件，则到编译器的类库路径下找，用于引用用户头文件
3. 引用系统头文件，两种形式都可以，include<> 效率高，引用用户头文件，只能使用include""

注意事项：

1. 一个#include命令只能包含一个头文件，多个头文件需要多个#include命令
2. 同一个头文件如果被多次引入，多次引入的效果和一次引入的效果相同，因为头文件在代码层面有防止重复引入的机制
3. 在一个被包含的文件(.c)中又可以包含另一个头文件(.h)
4. 不管是标准头文件，还是自定义头文件，都只能包含变量和函数的声明，不能包含定义。否则在多次引入时会引起重复定义错误

## 变量作用域

初始化局部变量和全局变量

1. 局部变量，系统不会对其默认初始化，必须对局部变量初始化后才能使用，否则，程序运行可能会异常退出

2. 全局变量，系统会自动对其初始化

   | 数据类型 | 初始化默认值 |
   | -------- | ------------ |
   | int      | 0            |
   | char     | '\0'         |
   | float    | 0.0          |
   | double   | 0.0          |
   | pointer  | NULL         |

3. 正确的初始化变量是一个良好的编程习惯，否则有时候程序可能会产生意想不到的结果，因为未初始化的变量会导致一些在内存位置中已经可用的垃圾值

4. 要使用其他文件的全局变量，使用extern引入

## static关键字

### 局部变量使用static修饰

1. 局部变量被static修饰后，称为静态局部变量
2. 对应静态局部变量在声明时未赋初值，编译器也会把它初始化为0
3. 静态局部变量存储于进程的静态存储区（全局性质），只会被初始化一次，即使函数返回，它的值也会保持不变

### 全局变量使用static修饰

1. 全局变量被static修饰后，称为静态全局变量
2. 静态全局变量只能在本文件中使用，无法在其他文件中使用
3. 定义不需要与其他文件共享的全局变量时，加上static关键字能够有效地降低程序模块之间的耦合，避免不同文件同名变量的冲突，且不会误使用

### 函数使用static修饰

1. 函数的使用方式和全局变量类似，在函数的返回类型前加上static，就是静态函数
2. 非静态函数可以在另一个文件中通过extern引用
3. 静态函数只能在声明它的文件中可见，其他文件不能引用该函数
4. 不同的文件可以使用相同名字的静态函数，互不影响

## 预处理命令和宏定义

### 预处理命令

1. 使用库函数之前，用#include引入对应的头文件，这种以#号开头的命令称为预处理命令
2. 这些在编译之前对源文件进行简单加工的过程，称为预处理
3. 预处理主要是处理以#开头的命令。与处理命令要放在所有函数之外，而且一般放在源文件的前面
4. 预处理是C语言的一个重要功能，由预处理程序完成，当对一个源文件进行编译时，系统将自动调用预处理程序对源程序中的预处理部分做处理，处理完毕自动进行对源程序的编译
5. C语言提供了多种预处理功能，如宏定义、文件包含、条件编译等

### 宏定义

#define叫做宏定义命令，也是预处理命令的一种，就是用一个标识符来表示一个字符串，如果在后面的代码中出现了该标识符，那么全部替换为指定的字符串

 **宏定义注意事项**

1. 宏定义是用宏名来表示一个字符串，在宏展开时又以该字符串取代宏名，这只是简单的替换。字符串可以含任何字符，可以是常数、表达式、if语句、函数等，预处理程序对它不做任何检查，如有错误，只能在编译已被宏展开后的源程序时发现
2. 宏定义不是说明或语句，在行尾不需要加分号
3. 宏定义必须写在函数之外，其作用域为宏定义命令起到源程序结束，如要终止其作用域可使用#undef命令
4. 可以使用宏定义表示数据类型，方便书写

### 带参数宏定义

1. 在宏定义中的参数称为"形式参数"，在宏调用中的参数称为"实际参数"
2. 对带参数的宏，在展开过程中不仅要进行字符串替换，还要用实参去替换形参
3. 带参宏定义的一般形式为 #define 宏名(形参列表) 字符串，在字符串中可以含有各个形参
4. 带参宏调用的一般形式为：宏名(实参列表)
5. 带参宏定义中，形参之间可以出现空格，但是宏名和形参列表之间不能有空格
6. 在带参宏定义中，不会为形参分配内存，因此不必指明数据类型

### 常见的预处理指令

| 指令     | 说明                                                      |
| -------- | --------------------------------------------------------- |
| #        | 空指令，无任何效果                                        |
| #include | 包含一个源代码文件                                        |
| #define  | 定义宏                                                    |
| #undef   | 取消已定义的宏                                            |
| #if      | 如果给定条件为真，则编译下面代码                          |
| #ifdef   | 如果宏已经定义，则编译下面代码                            |
| #ifndef  | 如果宏没有定义，则编译下面代码                            |
| #elif    | 如果前面的#if给定条件不为真，当前条件为真，则编译下面代码 |
| #endif   | 结束一个#if...#elif条件编译块                             |



## 数组

数组可以存放多个同一类型数据。数组也是一种数据类型，是构造类型。传递是以引用的方式传递（即传递的是地址）

### 字符数组

用来存放字符的数组称为字符数组

字符数组实际上是一系列字符的集合，也就是字符串

在C语言中，字符串实际上是使用null字符('\0')终止的一维数组，因此，一个以null结尾的字符串，包含了组成字符串的字符

'\0'是ASCII码表中的第0个字符，用NULL表示，称为空字符串。该字符既不能显示，也不是控制字符，输出该字符不会有任何效果。它在C语言中仅作为字符串的结束标记