

## Normal

- 对于基本数据类型，==比较的是**数值**
- 对于引用类型，==比较的是**地址值**

`public static <E> E requireNonNUll(E obj)` 查看指定引用对象是不是null

---



### this关键字

- 当方法的局部变量和类的成员变量重名时，根据就近原则，会优先使用局部变量。如需访问成员变量，就需要使用this关键字
- `this.成员变量`
- 谁调用的方法，谁就是this
- this的三种用法：
    1. 在本类的成员方法中，访问本类的成员变量
    2. 在本类的成员方法中，访问本类的另一个成员方法
    3. 在本类的构造方法中，访问本类的另一个构造方法
    4. this（）调用也必须是构造方法的一个语句，也只能有一个
    5. super和this两种构造调用，不能同时使用

---



### super关键字

三种用法：

 	1. 在子类的成员方法中，访问父类的成员变量
 	2. 在子类的成员方法中，访问父类的成员方法
 	3. 在子类的构造方法中，访问父类的构造方法

---



### Static关键字

- 一个成员变量使用了static关键字，那么这个变量不再属于对象自己，而是属于所在类，对个对象共享一份数据
- 对于一个成员方法，如果没有static关键字，必须创建对象，通过对象才能使用。
- 如果有了static关键字，那么不需要创建对象，直接通过类名称来进行调用
- 静态只能直接访问静态，不能直接访问非静态（内存中先有静态内容，后有非静态内容）
- 静态方法中不能使用this关键字

#### 静态代码块

- 当第一次用到本类的时候，静态代码块执行唯一的一次

- 静态内容总是优于非静态

- ````java
    public class 类名称{
        static{
    		//静态代码块内容
        }
    }
    ````

- 静态代码块的典型用途，用来一次性对静态成员变量进行赋值

---

### final关键字

- 当final关键字用来修饰一个类的时候，作用：其中的所有成员方法都无法进行覆盖重写

- 格式：

    ```java
    public final class 类名称{
        //...
    }
    ```

- 当final关键字用来修饰一个方法的时候，这个方法就是最终方法，无法覆盖重写。

    对于类、方法来说，abstract关键字和final关键字不能同时使用

    ```java
    修饰符 final 返回值类型 方法名称（参数列表）{
        //方法体
    }
    ```

- 一旦用final关键字修饰局部变量，那么这个变量就不能进行更改

    对于基本数据类型来说，数值不可变

    对于引用数据来说，地址值不可变

- 对于成员变量来说，如果使用final关键字修饰，那么这个变量不可变。对于final的成员变量，要么使用直接赋值，要么使用构造方法赋值。必须保证类当中的所有重载的构造方法，最终都会对final的成员变量进行赋值。

    

---



### 四种权限修饰符

|     路径     | public | default | property | private |
| :----------: | :----: | :-----: | :------: | :-----: |
|   同一个类   |  YES   |   YES   |   YES    |   YES   |
|   同一个包   |  YES   |   YES   |   YES    |   NO    |
|  不同包子类  |  YES   |   YES   |    NO    |   NO    |
| 不同包非子类 |  YES   |   NO    |    NO    |   NO    |

定义一个类时，权限修饰符规则：

外部类：public  / default

成员内部类：public / protected / default / private

局部内部类：什么都不写

---

### 内部类

一个类内部包含另一个类

分类：

1. 成员内部类
2. 局部内部类（包含匿名内部类）

#### 成员内部类：

```java
修饰符 class 外部类{
    修饰符 class 内部类名称{
        //
    }
    //
}
```

使用成员内部类的两种方法

1. 间接方法：在外部类的方法中，使用内部类，然后main调用外部类的方法
2. 直接方式：外部类名称.内部类名称 对象名 = new 外部类名称（）.new 内部类名称（）;

注意：内用外，随意访问，外用内，需要内部类对象

出现重复变量名时，访问外部类成员变量的方法：外部类名称.this.外部类成员变量

如果一个类是定义在方法内的，那这就是一个局部内部类，“局部”就是只有当前所属的方法才能使用他，定义格式：

```java
修饰符 class 外部类名称{
    修饰符 返回值类型 外部类方法名称(参数列表){
        class 局部内部类名称{
            //...
        }
    }
}
```

#### 局部内部类

如果希望访问所在方法的局部变量，那么这个局部变量必须是有效final的，从java8+开始，只要局部变量事实不变，那么final关键字可以省略

#### 匿名内部类

如果接口的实现类（或者是父类的子类）只需要使用唯一的一次。那么这种情况下就可以省略掉该类的定义，改为使用匿名内部类，定义格式：

```java
接口名称 对象名 = new 接口名称（）{
    //覆盖重写所有抽象方法
}
```

匿名内部类，在创建对象的时候，只能使用唯一的一次，如果希望多次创建对象，而且类的内容一样的话，就必须单独定义实现类

匿名对象，在调用方法的时候，只能调用唯一的一次，如果希望同一个对象，调用多次方法，那就必须给对象其给名字

匿名内部类是省略了实现类/子类名称，但是匿名对象省略了对象名称







---



### 构造方法

- 构造方法是专门用来创建对象的方法，当我们通过关键字new创建对象时，其实就是在调用构造方法

- 构造方法的名称必须和所在类名称完全一样，连大小写都一样

- 构造方法不要写返回值类型

- 构造方法不能return一个具体的返回值

- ```java
    public 类名称（参数类型 参数名称）{
        方法体;
    }
    ```

- 如果没有编写任何构造方法，编译器会自动创建一个无参构造方法

- 一旦编写了至少一个构造方法，编译就不再自动创建无参构造

- 构造方法也能够进行重载

---



### 匿名对象

- 匿名对象只能使用唯一的一次，下次再用需要再创建一个新的对象

- ```java
    new 类名称();
    ```

---



### 继承

- 继承主要解决的问题是：共性抽取

- ```java
    public class父类名称{
    	//...
    }
    public class 子类名称 extends 父类名称{
    	//...
    }
    ```

- java是单继承的，一个类的直接父类只能有唯一一个

    - 支持多级继承

        ```java
        class A{}
        class B extends A {}
        class C extends B {}
        ```

    - 一个子类只能有一个直接父类，但父类可以有多个子类

- 在父子类的继承关系中，如果成员变量重名，则创建子类对象时，访问有两种方式：

- 1. 直接通过子类对象访问成员变量，等号左边是谁，就优先用谁，没有则向上找

  2. 间接通过成员方法访问成员变量，方法属于谁，就优先用谁，没有则向上找

- 局部变量直接写成员变量名

    本类的成员变量：this.成员变量

    父类的成员变量：super.成员变量

- 在父子类的继承关系中，创建子类对象，访问成员方法的规则

    创建的对象是谁，就优先用谁，没有则向上找。无论是成员方法还是成员变量，如果没有，都是向上找

- 继承关系中，父子类构造方法的特点

    + 子类构造方法当中有一个默认的super()调用，所以一定是先调用父类的构造方法，后执行子类的
    + 子类构造可以通过super关键字来调用父类重载构造
    + super的父类调用必须写在子类构造方法的第一个语句。一个子类构造不能调用多次super构造
    + 子类构造方法必须调用父类的构造方法，super只能有一个且须是第一个

---



### 重写（Override）

- 在继承关系中，方法名称一样，参数列表也一样
- @Override:写在方法前面，用来检测是不是有效的正确覆盖重写
- 子类的返回值类型必须小于等于父类方法的返回值类型范围
- 子类方法的权限必须大于等于父类方法的权限修饰符

---



### 抽象

- 如果父类当中的方法不确定如何进行{}方法体的实现，那么这就应该是一个**抽象方法**

- 抽象方法所在的类必须是抽象类，在class前面写上abstract

- 使用抽象类和抽象方法

    抽象类不能直接创建抽象类对象

    必须有一个子类来继承抽象方法

    子类必须覆盖重写抽象父类中所有的抽象方法

    创建子类对象进行使用

- 抽象类可以有构造方法，供子类创建对象时，初始化父类成员使用

- 抽象类中不一定包含抽象方法，但是有抽象方法的类一定是抽象类

---

### 接口

- 接口就是一种公共的规范标准，接口是一种引用数据类型

- ```java
    public interface 接口名称{
        //内容
    }
    ```

- 接口包含的内容

    如果是java7，常量、抽象方法

    如果是java8，常量、抽象方法、默认方法、静态方法

    如果是java9,常量、抽象方法、默认方法、静态方法、私有方法

- 接口中的抽象方法，修饰符必须是固定的两个关键字：public abstract，可以选择性地省略

- 接口不能直接使用，必须有一个实现类来实现接口，创建实现类的对象，进行使用

    ```java
    public class 实现类名称 implements 接口名称{
        ///
    }
    ```

- 接口的实现类必须覆盖重写接口中所有的抽象方法，如果实现类没有覆盖重写接口中的所有抽象方法，那么这个实现类就必须是抽象类

- 接口的默认方法，可以解决接口升级的问题

    ```java
    public default 返回值类型 方法名称（参数列表）{
        //方法体
    }
    ```

- 接口的默认方法，可以通过接口的实现类对象进行直接调用，接口的实现类也可以覆盖重写接口的默认方法

- 接口定义静态方法

    ```java
    public static 返回值类型 方法名称（参数列表）{
        //方法体
    }
    ```

- 不能通过接口的实现类对象调用接口当中的静态方法。通过接口名称直接调用其中的静态方法

- 接口私有方法

    普通私有方法，解决多个默认方法之间重复代码的问题

    ```java
    private 返回值类型 方法名称（参数列表）{
        //方法体
    }
    ```

    静态私有方法，解决多个默认静态方法之间代码重复的问题

    ```java
    private static 返回值类型 方法名称（参数列表）{
        //方法体
    }
    ```

- 接口中定义常量，必须使用`public staitc final`三个关键字进行修饰

    `public static final 数据类型 常量名称 = 数据值`

    接口当中的常量，可以省略public static final

    接口当中的常量，必须赋值

    接口中常量的名称，必须完全大写

- 接口不能有静态代码块和构造方法

- 一个类可以同时实现多个接口，如果实现类所实现的多个接口中，有重复的抽象方法，那么只需覆盖重写一次即可

- 如果实现类实现的多个接口中，存在重复的默认方法，那么实现类一定要对冲突的默认方法进行覆盖重写

- 一个类的如果直接弗雷当中的方法和接口的默认方法产生了冲突，优先使用父类当中的方法

- 接口和接口之间是多继承的，如果多个父接口的默认方法重复，那么接口必须进行默认方法的覆盖重写

---

### 多态

- 代码当中体现多态性，其实就是一句话：父类引用子类

- ```java
    父类名称 对象名 = new 子类名称();
    接口名称 对象名 = new 实现类名称();
    ```

- 成员方法的访问规则：

    看new的是谁，就优先用谁，没有则向上找。编译看左边，运行看右边

- 对于成员变量，等号左边是谁，优先用谁，没有则向上找。编译看左边，运行还看左边

- 多态的好处

    无论右边new的时候换成哪个子类对象，等号左边调用方法都不会变化
    
- 对象的向上转型，其实就是多态

    `父类对象 对象名 =  new 子类名();` 右侧创建一个子类对象，把它当作父类来看待使用。向上转型的一定是安全的，从小范围转向了大范围

- 对象的向下转型

    `子类名称 对象名 = （子类名称） 父类对象名`。将父类对象，还原成为本来的子类对象

- 对象 instanceof 类名称

    将得到一个布尔值，判断前面的对象能不能当作后面类型的实例



---



## ArrayList

长度可以随意改变

主要方法

```java
public add(E e) 向集合末尾添加元素
public get(int index) 获取集合中指定位置的元素
public remove(int index) 删除集合中指定位置的元素
public size() 获取集合的长度，返回集合中元素的个数
```

---



## String

字符串的特点：

1. 字符串的内容永不可变
2. 字符串不可改变，所以可以共享使用。达到节省内存的目地
3. 字符串相当与char[]字符数组，底层原理是byte[]字节数组
4. 创建字符串时，写上双引号，也相当于创建了字符串对象
5. 通过双引号创建的字符串都存储在字符串常量池中（字符串常量池存在于堆当中）

- 方法

```java
public boolean equals(Object object)比较两个字符串的内容是否相同。如果比较双方一个常量一个变量，建议把常量字符串写在前面
public boolean equalsIgnoreCase(String string)比较两个字符串的内容是否相同，忽略英文字母的大小写
public int length()获取字符串的长度，即字符串含有的字符个数
public String concat(String str)将当前字符串和参数字符串拼接返回一个新的字符串
public char charAt(int index) 获取指定索引位置的单个字符
public int indexOf(String str)查找参数字符串在本字符串中首次出现的位置,如果没有返回-1
public String substring(int index)从参数位置一直到字符串末尾，返回新字符串
public String substring(int begin,int end) 从begin开始截取到end结束
public char[] toCharArray()将当前字符串拆分成字符数组作为返回值
public byte[] getBytes()获得当前字符串底层的字节数组
public String replace(CharSequence oldString, CharSequence newString)将出现的老字符串替换成新的字符串，返回替换后的新字符串
public String[] split(String regex)安装参数规则，将字符串切分成若干分
```

---



## Arrays

- java.util.Arrays是一个与数组相关的工具类，里面提供大量静态方法，用来实现数组常见的操作

- ```java
    public static String toString(数组) 将参数数组变成字符串
    public static void sort(数组) 按照默认升序对数组进行排序
    ```

---



## Math

- java.util.Math类是数学相关的工具类，里面提供了大量的静态方法，完成和数学运算相关的操作

- ```java
    public static double abs(double num)获取绝对值
    public static double ceil(double num)向上取整
    public static double floor(double num)向下取整
    public static long round(double num)四舍五入
    Math.PI 代表近似的园周率
    ```

---



 ## Objetct类

Object类是层次结构的跟类，每个类都使用Object作为父类

String toString() 返回对象的字符串表示，默认返回的是对象的地址值

boolea equals(Object obj) 其他某个对象和此对象是否相等，默认比较的是两个对象的地址值

Objects类的equals方法，对两个对象进行比较。防止空指针异常

---

## Date类

表示日期和时间的类，表示特定的瞬间，精确到毫秒

Data类的空参数构造方法，获取当前系统的日期和时间

Date类的带参数构造方法，把毫秒值转换为日期

Date类的成员方法：

```java
Long getTime() 把日期转换为毫秒(相当与System.currentTimeMillis())
    返回自1970年1月1日 00：00：00 以来Date对象表示的毫秒值
```

### DateFormat类

作用：格式化（日期-> 文本）、解析（文本-> 日期）

成员方法：

```java
String format(Date date)按照指定的模式，把Date日期格式化为符合模式的字符串
Date parse(String source) 把符合模式的字符串，解析为Date日期
```

DateFormat是抽象类，无法直接创建对象使用，可以使用DateFormat的子类 SimpleDateFormat类

`String pattern:传递指定的模式` 模式区分大小写

| y    | 年   |
| ---- | ---- |
| M    | 月   |
| d    | 日   |
| H    | 时   |
| m    | 分   |
| s    | 秒   |

写对应模式，会把模式替换为对应的日期和时间

SimpleDateFormat的format方法，把日期格式化为文本：

创建SimpleDateFormat对象，构造方法中传入指定的模式

调用SimpleDateFormat对象中的format方法，按照构造方法中指定的模式，把Date日期转换为符合模式的字符串

SimpleDateFormat的parse方法，把文本解析为日期：

创建SimpleDateFormat对象，构造方法中传递指定的参数

调用SimpleDateForamt对象中的parse方法，把符合构造方法中模式的字符串，解析为Date日期

---



### Calendar类

Calendar是一个抽象类，提供了很多操作日历字段的方法

Calendar无法之间创建对象使用，里边有一个静态方法叫getInstance()，该方法返回了Calendar类的子类对象

`static Calendar getInstance()`使用默认时区和语言环境获得一个日历

常用方法：

```java
public int get(int field)返回给定日历字段的值
public void set(int field, int value)将给定的日历字段设置为给定值
public abstract void add(int field, int amount)根据日历的规则，为给定的日历字段添加或减去指定的时间量
public Date getTime()返回一个表示Calendar时间值的Date对象
```

int field 日历类的字段，可以使用Calendar类的静态成员变量获取

---

## System类

```java
public static long currentTimeMillis()返回以毫秒为单位的当前时间
public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)将数组中指定的数据拷贝到另一个数组中
//src 源数组   srcPos 源数组中的起始位置    dest 目标数组   destPos 目标数组的起始位置   length 要复制的数组元素的数量
```

---

## StringBuild类

字符串缓冲区，可以提高字符串的操作效率，底层也是一个数组，但是没有被final修饰，可以改变长度

构造方法：

```java
public StringBuilder()构造一个空的StringBuilder容器
public StringBuilder(String str)构造一个StringBuilder容器，并将字符串添加进去
```

常用方法：

```java
public StringBuilder append(.....)添加任意类型数据的字符串形式，并返回当前对象本身
public String toString()将当前StringBuilder对象转换为String对象
```

---

## 包装类

作用：基本数据类型使用起来非常方便，但是没有对应的方法来操作这些数据，所以使用一个类，把基本数据类型的数据包装起来，这个类就叫做包装类。

| 基本数据类型 | 对应的包装类 |
| ------------ | ------------ |
| byte         | Byte         |
| short        | Short        |
| int          | Integer      |
| long         | Long         |
| float        | Float        |
| double       | Double       |
| char         | Character    |
| boolean      | Boolean      |

### Integer

装箱：把基本类型的数据，包装到包装类中

构造方法：

```java
Integer(int value) 构造一个新分配的Integer对象，表示指定的int值
Integer(String s) 构造一个新分配的Integer对象，表示String参数所指示的int值
```

静态方法：

```java
static Integer valueOf(int i) 返回一个指定的int值的Integer实例
static Integer valueOf(String s)返回保存指定的String的值的Integer对象
```

拆箱：在包装类中取出基本数据类型的数据

成员方法：

```java
int intValue() 以int类型返回该Integer的值
```

从java5（JDK1.5）开始，基本数据类型与包装类的装箱和拆箱动作就可以自动完成了

### String

基本数据类型与字符串之间的转换

基本数据类型-->字符串：

	1. 基本数据类型的值+""
	2. 使用包装类中的静态方法 `static String toString(int i)` 返回一个指定整数的String对象
	3. 使用String类中的静态方法 `static String valueOf(int i)` 返回int参数的字符串表达形式

字符串-->基本数据类型

使用包装类的静态方法parseXX("字符串")

 1. Integer类：`static int paeseInt(String s)`
 2. Double类：`static double paseDouble(String s)`

---

## Collection集合

集合：集合是java提供的一种容器，可以用来存储多个数据

集合和数组的区别：

	1. 数组的长度是固定的，集合的长度是可变的
	2. 数组存储的同一类型的元素，可以存储基本数据类型。集合存储的是对象，而对象的类型可以不一样

### List接口

1. 有序的集合（存储和取出元素的顺序相同）
2. 允许存储重复的元素
3. 有索引，可以使用普通的for循环遍历

### Set接口

1. 不允许存储重复的元素
2. 没有索引
3. TreeSet和HashSet是无序集合，LinkedHashSet是有序集合

Collection常用方法：

```java
public boolean add(E e) 把给定的对象添加到当前集合中
public void clean() 清空集合中所有的元素
public boolean remove(E e) 把给定的对象从集合中删除
public boolean contains(E e) 判断当前集合是否包含给定的对象
public boolean isEmpty(E e) 判断当前集合是否为空
public int size() 返回集合中元素的个数
public Object[] toArray() 把集合中的元素，存储到数组中
```

### Iterator接口

迭代器（对集合进行遍历）

```java
boolean hasNext() 判断集合中有没有下一个元素，有则返回true
E next() 取出集合的下一个元素
```

Iterator迭代器是一个接口，无法直接使用，需要使用Iterator接口的实现类对象

Collection接口有一个方法，叫iterator（），这个方法返回的是迭代器的实现类对象

### 增强for

用来遍历集合和数组

格式：

```java
for(集合/数组的数据类型 变量名 ： 集合名/数组名){
    //...
}
```

### 泛型

当我们不知道使用什么数据类型的时候，可以使用泛型

使用泛型的好处：

	1. 避免了类型转换异常
	2. 把运行期异常，提升到了编译器异常

弊端：

​	泛型是什么类型，就只能存储什么类型

定义含有泛型的类

```java
修饰符 class 类名称<E> {
    // ...
}
```

定义含有泛型的接口：

```java
修饰符 interface 接口名<代表泛型的变量>{}
```

含有泛型的接口，有两种使用方法：

​	1. 定义接口的实现类，实现接口，指定接口的泛型

泛型通配符：不知道使用什么类型来接收的时候，可以使用？表示未知通配符

受限泛型：

 1. 泛型的上限：`类型名称 < ? entends 类> 对象名称`

    意义：只能接收该类型及其子类

 2. 泛型的下限： `类型名称 < ? super 类> 对象名称`

    意义：只能接收该类型及其父类类型

### List集合

List接口的特点：

	1. 有序的集合，存储元素和取出元素的顺序是一致的
	2. 有索引。包含了一些带索引的方法
	3. 允许存储重复的元素

List接口带索引的方法：

```java
public void add(int index,E element) 将指定的元素，添加到该集合的指定位置上
public E get(int index) 返回集合中指定位置的元素
public E remove(int index) 移除列表中指定位置的元素，返回的是移除的元素
public E set(int index, E element) 用指定元素替换指定位置的元素，返回值是更新前的元素
```

ArrayList集合数据存储的结构是数组结构，元素增删慢，查找快。在日常开发中使用最多的功能为查询数据，遍历数据

#### LinkedList

LinkedList集合数据存储的结构是链表结构。增删快，查找慢。

LinkedList集合是双向链表。里面包含了大量操作首尾元素的方法，使用LinkList集合特有的方法，不能使用多态

特有方法：

```java
public void addFirst(E e) 将指定元素插入此列表的开头
public void addLast(E e) 将指定元素添加到此列表的结尾
public void push(E e) 将元素推入此列表所表示的堆栈
public E getFirst() 返回此列表的第一个元素
public E getLast() 返回此列表的最后一个元素
public E removeFirst() 移除并返回次列表的第一个元素
public E removeLast() 移除并返回此列表的最后一个元素
public boolean isEmpty() 如果列表不包含元素，返回true
```

### Set集合

Set接口特点：

	1. 不允许存储重复的元素
	2. 没有索引，没有带索引的方法

#### HashSet

特点：

	1. 不允许存储重复的元素
	2. 没有索引，没有带索引的方法
	3. 是一个无序的集合，存储元素和取出元素的顺序不一样
	4. 底层是一个哈希表结构（查询快）

HashSet集合存储数据的结构（哈希表）：
jdk1.8之前：哈希表=数组+链表

jdk1.8之后：哈希表=数组+链表

​					哈希表=数组+红黑树（提高查询的速度）

#### LinkHashSet集合

底层是一个哈希表（数组+链表/红黑树）+链表：多了一条链表（记录元素的存储顺序），保证元素的有序

特点：

	1. 不能存储重复的元素
	2. 是一个有序的集合

#### 可变参数

当方法的参数列表数据类型已经确，但参数的个数不确定，就可以使用可变参数

使用格式：定义方法时使用

​	`修饰符 返回值类型 方法名（数据类型...变量名）{}`

可变参数的原理：

​	可变参数底层就就是一个数组，根据传递参数个数不同，会创建不同长度的数组，来存储这些参数，传递的参数个，可以是0个（不传递）1、2、3...多个

注意事项：

	1. 一个方法的参数列表只能有一个可变参数
	2. 如果方法的参数有多个，那么可变参数必须写在参数列表的末尾

#### Collections工具类

常用方法：

```java
public static <E> boolean addAll(Collection<E> c, E...elements)：往集合中添加一些元素
public static void shuffle(List<?> list) 打乱顺序，打乱集合顺序
public static void sort(List<E> list) 将集合中元素按照默认规格排序
public static <E> void sort(List<E> list, Comparator<? super T>) 将集合按照指定规则排序
```

### Map集合

特点：

	1. Map集合是一个双列集合，一个元素包含两个值（一个key，一个value）
	2. Map集合中的元素，key和value的数据类型可以相同，也可以不同
	3. Map集合中的元素，key不允许重复的，value是可以重复的
	4. Map集合中的元素，key和value是一一对应的

常用方法：

```java
public V put(K key, V value) 把指定的键与指定的值添加到Map集合中
    存储键值对的时候，key不重复，返回值是null
    存储键值对的时候，key重复，会使用新的value替换map中重复的value，返回被替换的value值
public V remove(Object key) 把指定的键所对应的键值对元素，在Map集合中删除，返回被删除元素的值
public V get(Object key) 根据指定的键，在Map集合中获取对应的值
boolean containsKey(Object key) 判断集合中是否包含指定的键
keySet() 把Map集合中所有的key去出来，存储到一个Set集合中
Map.Entry<K, V> 在Map接口中有一个内部接口Entry，作用：当Map集合一创建，就会在Map集合中创建一个Enpry对象，用来记录键与值（键值对对象，键与值的映射关系）
Set<Map.Enpty<K, V>> entrySet() 把Map集合内部的多个Enpty对象去出来存储到一个Set集合中
```



#### HashMap集合

特点：

 	1. HashMap集合底层是哈希表，查询速度快
 	2. HashMap集合是一个无序的集合，存储元素和取出元素的顺序有可能不一致

#### LinkedHashMap集合

特点：

	1. LinkHashMap集合底层是哈希表+链表
	2. 是一个有序的集合，存储元素和取出元素的顺序是一致的

#### Hashtable集合

底层是哈希表，是一个线程安全的集合，是单线程集合，速度慢

Hashtable集合，不能存储空值（null）

Hashtable集合已经被取代了，其子类Properties依旧活跃在历史舞台，是一个唯一和IO流相结合的集合

---



## 异常

Throwable体系

```java
1. error：严重错误Error，无法通过处理的错误，只能事先避免
2. Exceprtion：表示异常，异常产生后可以通过代码的方式纠正，是程序继续运行，是必须处理的
```

如果父类抛出了多个异常，子类重写父类该方法时，抛出和父类相同的异常或异常的子类或者不抛出异常

如果父类方法没有抛出异常，子类重写父类方法时，也不能抛出异常

### Exception

编译器异常，进行编译（写代码）java程序出现的问题

​	RuntimeException：运行期异常，java程序运行过程出现的问题

#### throw关键字

作用：可以使用throw关键字在指定的方法中抛出指定的u异常

使用格式：

​	`throw new xxxException(异常产生的原因);`

注意事项：

  1. throw关键字必须写在方法的内部

  2. throw关键字后边new的对象必须是Exception或者是Exception的子类

  3. throw关键字抛出指定异常对象，就必须处理这个异常对象

     ​	throw关键字后边创建的是runtimeException或者是runtimeException的子类对象，可以不处理，默认交给JVM处理（打印异常对象，中断程序）

     ​	throw关键字后边创建的是编译器异常，我们就必须处理这个异常，要么throws，要么try...catch

### 处理异常的方式：

#### 1.throws关键字

处理异常的第一种方式，交给别人处理

作用：

当方法内部抛出异常对象的时候，那么我们就必须处理这个异常对象

可以使用throws关键字处理异常对象，会把异常对象抛出给方法的调用这处理（自己不处理，交给别人处理）最终交给JVM虚拟机处理

使用格式：在方法中声明

```java
修饰符 返回值类型 方法名（参数列表） throws xxxException,xxxException{
	///	
}
```

注意：

1. throws关键字必须写在方法声明处

2. throws关键字后边声明的异常必须是Exception或Exception的子类

3. 方法内部如果抛出了多个异常对象，throws后边必须也声明多个异常，如果抛出的多个异常对象有子父类关系，那么直接声明父类异常即可

4. 调用一个声明抛出异常的方法，我们就必须处理声明的异常，要么继续throws声明抛出

    要么try...catch处理

#### 2.try...catch

捕获异常：Java中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理

```java
try{
    //可能出现异常的代码
} catch(异常类型 e){
    处理异常的代码
   //记录日志/打印异常信息/继续抛出异常
}
```

多异常使用捕获如何处理：

 1. 多个异常分别处理

 2. 多个异常一次捕获，多次处理

    一个try多个catch注意事项：catch定义的异常处理，如果有子父类关系，那么子类的异常必须写在上边，否则会报错

 3. 多个异常一次捕获一次处理

### Throwable类

```java
public String getMessage() 获取异常的描述信息，原因
public String toString()获取异常的类型和异常描述信息
public void printStackTrace() 打印异常的跟踪栈信息并输入到控制台（包含异常的类型、异常的原因、异常出现的位置）
```

### finally代码块

finally不能单独使用，必须和try一起使用

finally一般用于资源释放（资源回收），无论程序是否出现异常，最后都要资源释放

如果finally有return语句，永远返回finally中的结果，避免该情况

```java
try{
    //可能出现异常的代码
} catch(异常类型 e){
    处理异常的代码
   //记录日志/打印异常信息/继续抛出异常
} finally {
    //
}
```

### 自定义异常类

```java
public class xxxException extends Exception | RuntimeException{
    //添加空参数的构造方法
    //添加一个带异常信息的构造方法
}
```

自定义异常类一般是以Exception结尾，说明该类是一个异常类

自定义异常类，必须继承Exception或RuntimeException

​	继承Exceptoin，那么自定义异常就是一个编译器异常

​	继承RuntimeException，那么自定义异常就是一个运行期异常

## 多线程

并发：指两个或多个事件在同一个时间段内方式

并行：指两个或多个事件在同一时刻发生（同时发生）

进程：是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程。进程是程序的一个执行过程，是系统运行程序的基本单位

线程：线程是进程的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中可以有多个线程，这个应用程序称之为多线程程序

分时调度：所有的线程轮流使用CPU的使用权，平均分配每个线程占用CPU的时间

抢占式调度：优先让优先级高的线程使用CPU，如果线程的优先级相同，那么会随机选择一个，JAVA使用的是抢占式调度

主线程：执行主方法的线程

单线程程序：java程序中只有一个线程，执行从main方法开始，从上到下执行。

### 创建多线程

第一种方式：创建Thread类的子类。实现步骤：

​	1. 创建一个Thread类的子类

​	2. 在Thread类的子类重写Thread类中的run方法，设置线程任务

​	3. 创建Thread类的子类对象

​	4. 调用Thread类中的方法start方法，开启新的线程，执行run方法

​		`void start()`是线程开始执行，java虚拟机调用该线程的run方法

​		多次启动一个线程是非法的，特别是当线程已经结束执行后，不能在重新启动

第二种方式：

实现Runnable接口

	1. 创建一个Runnable接口的实现类
	2. 在实现类中重写Runnable接口的run方法，设置线程任务
	3. 创建Runnable接口的实现类对象
	4. 创建Thread类对象，构造方法中传递Runnable接口的实现类对象
	5. 调用Thread类中的start方法，开启新的线程执行run方法

Thread和Runnable的区别

实现Runnable接口创建多线程的好处：

 1. 避免了单继承的局限性，一个类只能继承一个类，类继承了Thread类就不能继承其他类，实现了Runnable接口，还可以实现其他的类，其他的接口

 2. 增强了程序的扩展性，降低了程序的耦合行（解藕）

    实现Runnable接口的方式，把设置线程任务和开启新线程进行了分离。实现类中，重写了run方法：用来设置线程任务。创建Thread类，调用start方法，来开启新线程。

	3. 

### Thread类

```java
String getName() 获取线程的名称
static Thread currentThread() 获取当前正在执行的线程对象的引用
void setName(String name) 设置线程名称
public static void sleep(long millis) 是当前正在执行的线程以指定的毫秒值数暂停，结束后，程序继续执行
```

### 线程同步技术

#### 1. 同步代码块

synchronized关键字可以用于方法中莫个区块内，表示只对这个区块的资源实行互斥访问

格式：

```java
synchronized(同步锁){
    //需要同步操作的代码块
}
```

同步代码块中的锁对象，可以使用任意的对象。

但是必须保证多个线程使用的锁对象是同一个

锁对象作用：把同步代码块锁住，只让一个线程在同步代码块中执行

#### 2. 使用同步方法

使用步骤：

把访问共享数据的代码抽取出来，放到一个方法中

在方法上添加synchronized修饰符

```java
修饰符 synchronized 返回值类型 方法名（参数列表）{}
```

同步方法也会把方法内部的代码锁住，只让一个线程执行

#### 3.使用Lock锁

Lock中的方法：

```java
void lock() 获取锁
void unlock() 释放锁
```

使用步骤：
在成员位置创建ReentrantLock对象

在可能出现安全问题的代码前调用Lock接口中的方法Lock获取锁

在可能出现安全问题的代码后调用Lock接口中的方法unLock释放锁

### 线程状态

| 线程状态                 | 导致线程状态发生条件                                         |
| ------------------------ | ------------------------------------------------------------ |
| NEW（新建）              | 线程刚创建，但是并未启动。还没有调用start方法                |
| Runnable（可运行的）     | 线程可以在java虚拟机中运行的状态，可能正在运行自己代码，也可能没有，取决与系统处理器 |
| Blocked（锁阻塞）        | 当一个线程试图获取一个对象锁，而该对象被其他的线程持有，则该线程进入Blocked状态，当该线程持有锁时，该线程将变成Runnable状态 |
| Waiting（无限等待）      | 一个线程在等待另一个线程执行一个（唤醒）动作是，该线程进入Waiting状态，进入这个状态不能自动唤醒。必须等待另一个线程调用notify或notifyAll方法才能够唤醒 |
| TimedWaiting（计时等待） | 同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。这一状态将一直保持到超时期满或者收到唤醒通知。带有超时参数的常用方法有Thread.sleep、Object.wait |
| Teminated（被终止）      | 因为run方法正常退出而死亡，或者因为没有捕获异常终止了run方法而死亡 |



### 线程通信

为什么需要处理线程通信：
多个线程并发执行时，在默认条件下CPU是随机切换线程的，当我们需要多个线程来共同完成一件任务，并且我们希望他有规律的执行，那么多线程之间需要一些协调通信，以此来帮我们达到多线程共同操作一份数据

进入TimeWaiting（计时等待）有两种方式：

使用 `sleep(long m)`方法，在毫秒值结束后，线程唤醒进入的哦Runnable/Blocked状态

使用 `wait(long m)`方法，wait方法如果在毫秒值结束后，还还没有被notify唤醒，就会自动醒来

唤醒方法：

​	void notify( ) 唤醒在此对象监视器上等待的单个线程

​	void notifyAll( ) 唤醒在此监视器上等待的所有线程

wait方法和notify方法必须要由同一个锁对象调用

wait方法和notify方法是属于Object类的方法

wait方法和notify方法必须在同步代码块或者同步函数中使用。

### 线程池

就是一个容纳多个线程的容器，其中的线程可以反复使用，省去了频繁创建线程对象的操作，无需重复创建线程而消耗过多的资源

好处：

降低资源消耗

提高响应速度

提高线程的可管理性

```java
static ExecutorService new FixedThreadPools(int nThread) 创建一个可重用固定线程数的线程池
参数：int nThread 创建线程池中包含的线程数量
返回值：返回的是ExecutorService接口的实现类对象，可以使用ExecutorService接口接收
    
ExecutorService接口：
    submit(Runnable task)提交一个Runnable任务用于执行
    void Shutdown() 关闭/销毁线程池
```

线程池的使用步骤：

1.使用线程池的工厂类Executors里边提供的静态方法newFixedThreadPool生产一个指定线程数量的线程池

2.创建一个类，实现Runnable接口，重写run方法，设置线程任务

3.调用ExecutorService中的方法submit，传递线程任务（实现类），开启线程，执行run方法

## Lambda表达式

### 函数式接口

函数式接口在java中是指：有且只有一个抽象方法的接口

格式：

```java
修饰符 interface 接口名称{
	public abstract 返回值类型 方法名称(可选参数信息);
    // 其他非抽象方法内容
}
```

#### @FunctionalInterface

@FunctionalInterface注解，检测此接口是否为函数式接口

### Lambda的使用前提：

使用Lambda必须具有函数式接口

使用Lambda必须具有上下文推断

Lambda的特点：延迟加载

### Lambda由三部分组成：

一些参数

一个箭头

一段代码

Lambda表达式的标准格式：

`(参数类型 参数名称) -> {代码语句}`

### 省略规则：

1.小括号内参数的类型可以省略

如果小括号内有且只有一个参，则小括号可以省略

如果大括号有且只有一个语句，则无论是否有返回值，都可以省略大括号，return关键字和语句分号

### 常用函数式接口：

#### Supplier接口

用来获取泛型参数指定类型的对象数据

Supplier<T>接口被称为生产型接口，指定接口的泛型是什么类型，那么接口中的get方法就会生产什么类型的数据

```java
void get(T t)
```

#### Consumer接口

Consumer<T>接口与Supplier接口相反，不是生产一个数据，而是消费一个数据，具体怎么消费（使用），需要自定义

```java
void accept(T t)
//默认方法
andThen() 需要两个Consumer接口，可以把两个Consumer接口组合到一起，对数据进行消费
Consumer1 andThen(Consumer2).accept() s
```

#### Predicate接口

Predicate接口对某种类型的数据进行判断，结果返回一个boolean值

```java
boolean test(T t) 用来对指定类型数据进行判断
// 默认方法
void and() 表示并且关系，也可用于连接两个判断条件
void or() 表示或者关系，也可用于连接两个判断条件
void negate() 表示取反关系
```

#### Function接口

Function<T R>接口用来根据一个类型的数据得到另一个类型的数据，前者称为前置条件，后者称为后置条件

```java
R appl(T t) 根据类型T的参数获取类型R的结果
//默认方法
andThen() 用来进行组合c
```

### 方法引用：

1. 通过对象名引用成员方法
2. 通过类名引用静态方法
3. 通过super引用父类的成员方法
4. 通过this引用成员方法

---



## File

文件和目录路径名的抽象表示形式。主要用于文件和目录的创建、查找和删除等操作。

### 静态方法：

```java
static String pathSeparator 与系统有关的路径分隔符
static char pathSeparatorChar 与系统有关的路径分隔符

static String separator 与系统有关的默认名称分隔符
static char separator 与系统有关的默认名称分隔符
```
### 构造方法：

```java
File(String pathName) 通过将给定路径字符串转换为抽象路径名来创建一个新File实例
File(String parent, String child) 根据parent路径名字符串和child路径名字符串创建一个新File实例
File(File parent, String child) 根据parent路径名字符串和child路径名字符串创建一个新File实例
```
### 常用方法：

```java
public String getAbsolutePath() 返回此File的绝对路径名字字符串
public String getPath() 将此File转换为路径名字字符串
public String getName() 返回由此File表示的文件或目录的名称，获取的就是构造方法传递路径的结尾部分（文件/文件夹）
public long length() 返回由此File表示的文件的长度，获取的是构造方法指定的文件大小，以字节为单位。文件夹是没有大小的，不能获取文件夹的大小，如果构造方法中给出的路径不存在，返回0
```

### 判断功能方法：

```java
public boolean exists() 此File表示的文件或目录是否真是存在
public boolean isDirectory() 此File表示的是否为目录
public boolean isFile() 此File表示的是否为文件
```

### 创建删除功能：

```java
public boolean createNewFile() 当且仅当具有该名称的文件不存在时，创建一个新的空文件
public boolean delete() 删除由此File表示的文件或目录
    文件夹中有内容，不会删除返回false
public boolean mkdir() 创建此File表示的目录
public boolean mkdirs() 创建由此File表示的多级目录
```

### 目录的遍历

```java
public String[] list() 返回一个String数组，表示该File目录中的所有子文件或目录
public File[] listFiles() 返回一个File数组，表示该File目录中的所有子文件或目录
```

### 递归

指在当前方法内调用自己，使用前提：当调用方法的时候，方法的主体不变，每次调用方法的参数不同，可以使用递归

递归的分类：

1.	直接递归
2.	间接递归

注意事项：

1. 递归一定要有条件限定，保证递归能够停下来，否则会发生栈内存溢出
2. 在递归中虽然有条件限定，但是递归次数不能太多，否则会发生栈内存溢出
3. 构造方法，禁止递归

### 文件过滤器

```java
File[] listFiles(FileFilter filter)
    FileFilter接口：用于抽象路径名（File对象）的过滤器
    作用：用来过滤文本
    抽象方法：
        boolean accept(File pathname) 测试指定抽象路径是否包含在某个路径名列表中
        参数：File pathname 使用ListFiles方法遍历目录，得到的每一个文件对象
File[] listFiles(FilenameFilter filter)
    FilenameFilter接口
    抽象方法：
    	boolean accept(File dir, String name) 测试指定文件是否应该包含在某一个文件列表中
```

---

## IO流

根据数据的流向分为：输入流和输出流

​	输入流：把数据从其他设备上读取到内存中的流

​	输出流：把数据从内存中写出到其他设备的流

根据数据的类型分为字节流和字符流

|        | 输入流                 | 输出流                  |
| ------ | ---------------------- | ----------------------- |
| 字节流 | 字节输入流 InputStream | 字节输出流 OutputStream |
| 字符流 | 字符输入流 Reader      | 字符输出流 Writer       |

### 字节输出流

```java
public void close() 关闭此输出流并释放与此流相关联的任何系统资源
public void flush() 刷新此输出流并强制任何缓冲的输出字节被写出
public void write(byte[] b) 将b.length字节从指定的字符数组写入此输出流
	一次写多个字节：
    	如果写的第一个字节是正数(0-127)，显示的时候会查询ASCII表
    	如果写的第一个字节是负数，那么第一个字节和第二字节，两个字节组成一个中文显示，查询系统默认码表
public void write(byte[] b, int off, int len) 将指定的字节数组写入len字节，从poff开始输出到此输出流
    int off：数组的开始索引
    int len：写几个字节
public abstract void write(int b)  将指定的字节写入此输出流 
```

#### FileOutpuStream

文件字节输出流：把内存中的数据写入到硬盘的文件中

构造方法：

```java
FileOutputStream(String name) 创建一个向具有指定名称的文件中写入数据和的输出文件流
FileOutputStream(File file) 创建一个向指定File对象表示的文件中写入数据的文件输出流
FileOutputStream(String name, boolean append) 创建一个向具有指定名称的文件末尾写入数据和的输出文件流（续写）
FileOutputStream(File file, boolean append)创建一个向指定File对象表示的文件末尾中写入数据的文件输出流（续写）
```

构造方法作用：

1. 创建一个FileOutputStream对象
2. 会根据构造方法中传递的文件/文件路径，创建一个空文件
3. 会把FileOutputStream对象指向创建好的文件

### 字节输入流

```java
public void close() 关闭此输入流并释放与此流相关联的任何资源
public abstract int read() 从输入流读取数据的下一个字节
public void rend(byte[] b) 从输入流中读取一些字节数，并将它们存储到字节数组中
```

#### FileInputStream

构造方法

```java
FileInputStream(File file) 通过打开与实际文件的连接来创建一个FileInputStream，该文件由文件系统中的File对象file命名
FileInputStream(String name) 通过打开与实际文件的连接来创建一个FileInputStream，该文件由文件系统中的路径名name命名
```

### 字符输入流

```java
int read() 读取单个字符并返回
int read(char[] chars) 将字符读入数组
```

#### FileReader

构造方法

```java
FileReader(String fileName)
FileReader(File file)
```

### 字符输出流

```java
void write(int c) 写入单个字符
void write(char[] chars) 写入字符数组
void write(char[] chars, int off, int len) 写入字符数组的某一部分，off表示开始索引，len表示写入长度
void write(String str) 写入字符串
void write(String str, int off, int len) 写入字符串的某一部分
void flush() 刷新该流的缓冲
void close() 关闭此流，但要先刷新
```

flush和close的区别：

flush：刷新缓冲区，流对象可以继续使用

close：先刷新缓冲区，流对象不能继续使用

#### Filewrite

```java
FileWriter(File file)
FileWriter(String fileName)
```

### Properties集合

Properties类表示了一个持久的属性集，是一个双列集合，key和value默认都是字符串。Properties可保存在流中或从流中加载

Properties是一个唯一和IO流相结合的集合，属性列表中每个键及其对对应值都是一个字符串

```java
Object setProperty(String key, String value)把指定的键和值存储到集合中
String getProperty(String key) 通过key找到value值
Set<String> stringPropertyNames() 返回此属性列表中的键集，其中该键及其对应值是字符串
```

可以使用Properties集合中的方法store，把集合中的临时数据，持久化写入到硬盘存储

```java
void store(OutputStream out, String comments)
void store(Writer writer, String comments)
    OutputStream out:字节输出流，不能写入中文
    Writer writer：字符输出流，可以写中文
    String comments：注意，用来解释说明保存的文件是做什么的
```

Properties集合中的方法load，把硬盘中保存的文件（键值对），读取到集合中使用

```java
void load(InputStream inStream)
void load(Reader reader)
    存储键值对的文件中，键与值默认的连接符可以使用=、空格（其他符号）
    存储键值对的文件中，可以使用#进行注释，被注释的键值对不会被读取
    存储键值对的文件中，键与值默认都是字符
```

---

### 缓冲流

缓冲流，也叫高效流，是对4个基本的流的增强

缓冲流的原理，是在创建对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写效率

| 字节缓冲流           | 字符缓冲流     |
| -------------------- | -------------- |
| BufferedInputStream  | BufferedWriter |
| BufferedOutputStream | BufferedReader |



#### 字节缓冲流

构造方法

```java
public BufferedInputStream(InputStream in) 创建一个新的缓冲输入流
public BufferedOutputStream(OutputStream out) 创建一个新的缓冲输出流
```

##### 字节缓冲输出流

```java
BufferedOutputStream(OutputStream out)	创建一个新的缓冲输出流，将数据写入指定的底层输出流
BUfferedOutputStream(OutputStream out, int size) 创建一个新的缓冲输出流，将具有指定缓冲区大小的数据写入指定的底层输出流
```

##### 字节缓冲输入流

```java
BufferedInputStream(InputStream in) 创建一个BufferedInputStream并保存其参数，即输入流in，方便将来使用
BufferedInputStream(InputStream in, int size) 创建具有指定缓冲区大小的BufferedInputStream并保存其参数，及输入流in，方便将来使用
```

#### 字符缓冲流

##### 字节缓冲输出流

``` java
BufferedWriter(Writer out) 创建一个使用默认大小输出缓冲区的缓冲字符输出流
BufferedWriter(Writer out, int size) 创建一个使用给定大小输出缓冲区的新缓冲字符输出流
特有方法
void newLine() 写入一个行分隔符
```

##### 字符缓冲输入流

```java
BufferedReader(Reader in) 创建一个使用默认大小缓冲区的缓冲字符输入流
BufferedReader(Reader in, int size) 创建一个使用指定大小输入缓冲区的缓冲字符输入流
特有方法
String readLine() 读取一个文本和
```

---

### 转换流

#### OutputStreamWriter类

是字符流通向字节流的桥梁，可使用指定的charset将写入流中的字符编码成字节

构造方法

```java
OutputStreamWriter(OutputStream out) 创建使用默认字符编码的OutputStreamWriter
OutputStreamWriter(OutputStream out, String charsetName) 创建使用指定字符集的OUtputStreamWriter
```

#### InputStreamReader类

是字节流向字符的桥梁，使用指定的charset读取字节并将其解码为字符

构造方法

```java
InputStreamReader(InputStream in) 创建一个默认字符集的InputStreamReader
InputStreamReader(InputStream in, String charsetName) 创建使用指定字符集的InputStreamReader
```

### 序列化

序列化和反序列化的时候，需要实现Serializable接口以启用其序列化功能

Serializable接口也叫标记型接口

要进行序列化和反序列化的类必须实现Serializable接口，就会给类添加一个标记

#### ObjectOutputStream

```java
//构造方法
ObjectOutputStream(OutputStream out) 创建写入指定OutputStream的ObjectOutputStream
//特有的成员方法
    void writeObject(Object obj) 将指定的对象写入ObjectOutputStream
```

#### ObjectInputStream

```java
//构造方法
ObjectInputStream(InputStream in) 把文件中保存的对象，以流的形式读取出来使用
//特有的成员方法
    Object readObject() 从ObjectInputStream读取对象
```

#### transient关键字

被static修饰的成员变量不能被序列化，序列化的都是对象

transient是瞬态关键字，被transient关键字修饰的成员变量不能被序列化

#### serialVersionUID

在对象序列化的时候，会给序列化对象生成一个serialVersionUID，此UID会保存在序列化对象的文件中，当反序列化的时候，会把class文件中的serialVersionUID和文本文件中的serialVersionUID进行比较，如不相同，会抛出序列化冲突异常（每当对类的定义进行修改时，就会生成一个新的序列号）

为避免此异常，手动给类添加一个序列号

```java
static final long serialVersionUID = xxL;
```



### 打印流

**PrintStream**：打印流，为其他输出流添加了功能，是它们能够方便的打印各种数据值表示形式

特点：

1. 只负责数据的输出，不负责数据的读取

2. 与其他输出流不同，PrintStream永远不会抛出IOException

3. 特有的方法

   ```java
   void print()
   void println()
   ```

4. 构造方法

   ```java
   PrintStream(File file) 输出的目的地是文件
   PrintStream(OutputStream out) 输出的目的地是一个字节输出流
   PrintStream(String fileName) 输出的目的地是一个文件路径
   ```

5. 注意：

   1. 如果使用继承自父类的方法Write方法写数据，那么查看数据的时候会查询编码表
   2. 如果使用自己特有的方法写数据，那么数据原样输出

6. 使用System.setOut方法改变输出语句的目的地为参数中传递的打印流目的地

   ```java
   static void setOut(PrintStream out) 
   ```

   

---

## Socket类

### Socket

此类表示客户端套接字，套接字指的是两台设备之间通讯的端点

```java
//构造方法
Socket(String host, int port) 创建一个流套接字并将其连接到指定主机的指定端口
    // String host 服务器主机名称/ip 
    // int port 端口号
//成员方法
OutputStream getOutputStream() 返回此套接字的输出流
InputStream getInputStream() 返回此套接字的输入流
void close() 关闭此套接字
void shutdownOutput() 禁用此套接字的输出流
```

注意：

1. 客户端和服务端进行交互，必须使用Socket中提供的网络流，不能使用自己创建的流对象
2. 当我们创建客户端对象Socket的时候，就会去请求服务器和服务器进行3次握手建立连接通路，如果服务器没有启动，就会抛出异常。

### ServerSocket

```java
//构造方法
ServerSocket(int port) 创建绑定到指定端口的服务器套接字
//成员方法
Socket accept() 侦听并接受此套接字的l
```

---

## Stream流

Stream流是一个来自数据源的元素队列

1. 元素是特定类型的数据，形成一个队列。Java中的Stream并不会存储元素，而是按需计算
2. 数据源流的来源，可以是集合、数组等
3. Stream流属于管道流，只能被消费（使用）一次

### 获取Stream流的方式：

1. 所有的Collection集合都可以通过stream默认方法获取流

2. Stream接口中的静态方法of 可以获取数组对应的流

   ```java
   static <T> Stream<T> of (T..values)
   ```

### 常用方法：

分为两类：

1. 延迟方法：返回值类型仍然是Stream接口自身类型的方法，因此支持链式调用（除了终结方法，其他都是延迟方法）
2. 终结方法：返回值类型不再是Stream接口自身类型的方法，因此不再支持链式调用。

#### 逐一处理：forEach（终结方法）

```java
void forEach(Consumer<? super T> action) 
// 该方法接收一个Consumer接口函数，会将每一个流元素交给函数进行处理
```

#### 过滤：filter（延迟方法）

```java
Strem<T> filter(Predicate<? super T> predicate)
// 该接口接收一个Predicate函数式接口参数作为筛选条件
```

#### 映射：map（延迟方法）

```java
<R> Stream<R> map(Function<? super T, ? extends R> mapper) 
// 该接口需要一个Function函数式接口参数，可以将当前流中的T类型
```

#### 统计个数：count（终结方法）

```java
long count()
// 该方法返回一个long值代表元素个数
```

#### 取用前几个：limit（延迟方法）

```java
Stream<T> limit(long maxSize);
// 参数是一个long类型，如果集合当前长度大于参数进行截取，否则不操作
```

#### 跳过前几个：skip（延迟方法）

```java
Stream<T> skip(long n)
// 如果流的当前长度大于n，则跳过前n个，否则将会得到一个长度为0的空流
```

#### 组合：concat（延迟方法）

```java
static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b)
// 如果有两个流，希望合并成一个流，那么可以使用Stream中的静态方法concat()
```

