## Junit

### @Test注解

用于单元测试，不需要写主方法即可执行程序

### @Before

初始化方法，用于资源申请，所有测试方法在执行之前都会执行该方法

@After

释放资源方法，在所有测试方法执行后，都会自动执行该方法

## 反射

将类的各个组成部分封装为其他对象，这就是反射机制

好处：

1. 可以在程序运行中操作这些对象
2. 可以解耦，提高程序的可扩展性

### 获取Class对象的三种方式

1. Class.forName(“全类名”)：将字节码文件加载进内存

   多用于配置文件，将类名定义在配置文件中，读取文件，加载类

2. 类名.class：通过类名的属性class获取

   多用于参数的传递

3. 对象.getClass()：getClass()方法在Object类中定义着

   多用于对象的获取字节码方式

同一个字节码文件在一次程序的运行过程中，只会被加载一次，无论通过哪种方式获取的class对象，都是同一个。

### Class对象功能

#### 获取功能：

1. 获取成员变量

   ```java
   Field[] getFields() 获取所有public修饰的成员变量
   Field[] getFields(String name) 获取指定名称的public修饰的成员变量
   Field[] getDeclaredFields() 获取所有的成员变量，不考虑修饰符
   Field[] getDeclaredField(String name) 获取指定名称的成员变量
   Field：成员变量
   操作：
       1. 设置值
       void set(Object obj, Object value)
       2. 获取值
       get(Object ob)
       
       setAccessible() 是否忽略访问权限修饰符的安全
   ```

   

2. 获取构造方法

   ```java
   Constructor<?>[] getConstructors()
   Constructor<?>[] getConstructor(类<?>... parameterTypes)
   Constructor<?>[] getDeclaredConstructors() 
   Constructor<?>[] getDeclaredConstructor(类<?>... parameterTypes)
   
   // 创建对象
   T newInstance(Object... initargs)
   // 使用空参构造方法创建对象可以使用
   Class对象.newInstance()
   ```

3. 获取成员方法

   ```java
   Method[] getMethod()
   Method[] getMethod(String name, 类<?>... parameterTypes)
   Method[] getDeclaredMethods()
   Method[] getDeclaredMethod(String name, 类<?>... parameterTypes) 
   
   // 执行方法
   Object invoke(Object obj, Object... args)
   // 获取方法名称
   String getName()
   ```

4. 获取类名

   ```java
   String getName()
   ```


---

## 注解

概念：说明程序，给计算机看的

JDK中预定义的一些注解：

1. @Override：检测该注解标注的方法的是否继承自父类（接口）的
2. @Deprecated: 该注解标注的内容，表示已过时
3. @SuppressWarnings：压制警告

格式：

```java
元注解
public @interface 注解名称{}
```

本质：注解本质上就是一个接口，该接口默认继承了Anntation接口

### 属性：

接口中可以定义的成员方法

​	要求

1. 属性的返回值类型：基本数据类型、String、枚举、注解、以上类型的数组
2. 定义了属性，在使用时需要给属性赋值
   1. 如果定义属性时，使用default关键字给属性默认初始值，则使用注解时，可以不进行属性的赋值
   2. 如果只有一个属性需要赋值，并且属性的名称是value，则value可以省略，直接写属性值
   3. 数组赋值时，值使用{ }包裹，如果数组中只有一个值，{ }可以省略

### 元注解：

描述注解的注解

1. @Target:描述注解能够作用的位置

   ```java
   ElementType属性取值：
   	TYPE:可以作用在类上
        METHOD：可以作用在方法上
        FIELD：可以作用在成员变量上
   ```

2. @Retention:描述注解被保留的阶段

   ```java
   @Retention(RetentionPolicy.RUNTIME)
   // 当前被描述的注解，会保留到class字节码文件中，并被JVM读取到
   ```

3. @Document:描述注解是否被抽取到API文档中

4. @Inherited:描述注解是否被子类继承

使用（解析）注解：

获取注解中定义的属性值

1. 获取注解定义的位置的对象（Class, Method, Field）
2. 获取指定的注解 `getAnnotation(class)`
3. 调用注解中的抽象方法获取配置的属性值

---

## Mysql

### 操作数据库：

1. C(Create)创建

   ```sql
   创建数据库
   create database 数据库名称;
   创建数据库，判断不存在再创建
   create database if not exists 数据库名称;
   创建数据库，并指定字符集
   create database 数据库名称 character set 字符集名;
   ```

2. R(Retrieve) 查询

   ```sql
   查询所有的数据库的名称
   show databases;
   查询某个数据库的字符集；查询某个数据库的创建语句
   show create database 数据库名;
   ```

3. U(Update) 修改

   ```sql
   修改数据库的字符集
   alter database 数据库名 character set 字符集名称;
   ```

4. D(Delete) 删除

   ```sql
   删除数据库
   drop database 数据库名;
   删除数据库，判断存在删除
   drop database if exists 数据库名称;
   ```
   
5. 使用数据库

   ```SQL
   查询当前正在使用的数据库
   select database();
   使用数据库
   use 数据库名;
   ```

### 操作表

1. 创建

   ```sql
   创建表
   create table 表名(
   列名1 数据类型,
   列名2 数据类型,
       ...
   列名n 数据类型
   );
   复制表
   create table 表名 like 被复制表名;
   ```

2. 查询

   ```sql
   查询某个数据库中所有的表名称
   show tables;
   查询表结构
   desc 表名称
   ```

3. 删除

    ```sql
    删除表
    drop table 表名;
    删除表，判断存在删除
    drop table if exists 表名;
    ```

4. 修改

    ```sql
    修改表名
    alter table 表名 rename to 新表名;
    修改表的字符集
    alter table 表名 character 字符集名;
    添加一列
    alter table 表名 add 列名 数据类型;
    添加列名称 类型
    alter table 表名 change 列名 新列名 数据类型;
    alter table 表名 modify 列名 新数据类型;
    删除列
    alter table 表名 drop 列名;
    ```

### 操作表中数据

1. 添加数据

    ```sql
    insert into 表名(列名1,列名2,列名3...列名n) values(值1,值2...值n);
    ```

2. 删除数据

    ```sql
    delete from 表名 [where 条件];
    truncate table 表名;  # 删除表，然后在创建一个一摸一样的的空表
    ```

3. 修改数据

    ```sql
    update 表名 set 列名1=值1,列名2=值2...列名n=值n [where 条件];
    ```

### 查询表中数据

```sql
语法：
select 字段列表 from 表名列表 where 条件列表 group by 分组字段 having 分组之后的条件 order by 排序 limit 分页限定;

distinct 去重关键字
ifnull(列名, 替换值) # 将列中出现的null替换为指定的值

排序查询
order by # 默认升序， desc降序

条件查询
运算符 >、 <、 <=、 >=、 =、 <> # <> 表示不等于
between...and
in
like  # 占位符 _表示单个字符 %表示0或多个字符
is null
and 或 &&
or 或 ||
not 或 !

聚合函数 #将一列数据作为一个整体，进行纵向计算。所有聚合函数会排除null数据
count 计算个数
max 计算最大值
min 计算最小值
sum 求和
avg 计算平均值

分组查询
group by 分组字段 having 限定条件;
/*
where和having的区别：
where在分组之前进行限定，having在分组之后进行限定
where后不可以跟聚合函数，having可以进行聚合函数的判断
*/

分页查询
limit 开始的索引,每页查询的条数; # 开始索引 = (当前的页码-1) * 每页显示的条数
```

### 约束

概念：对表中的数据进行限定，保证数据的正确性、有效性和完整性。

分类：

1. 主键约束 primary key

    1. 含义：非空且唯一
    2. 一张表只能有一个字段为为主键
    3. 主键就是表中记录的唯一标识

    ```sql
    创建表时添加非空约束
    列名 数据类型 primary key
    创建表完成后，添加主键
    alter table 表名 modify 列名 数据类型 primary key;
    删除主键
    alter table 表名 drop primary key;
    
    如果某一列是数值类型的，使用auto_increment可以来完成自动增长
    列名 数据类型 auto_increment
    删除auto_increment
    alter table 表名 modify 列名 s
    ```

    

2. 非空约束 not null

    ```sql
    创建表时添加非空约束
    列名 数据类型 not null
    创建表完成后，添加非空约束
    alter table 表名 modify 列名 数据类型 not null;
    删除列的非空约束
    alter table 表名 modify 列名 数据类型;
    ```

3. 唯一约束 unique

    ```sql
    创建表时添加唯一约束
    列名 数据类型 unique
    创建表完成后，添加唯一约束
    alter table 表名 modify 列名 数据类型 unique;
    删除列的唯一约束
    alter table 表名 drop index 列名;
    ```

4. 外键约束 foreign key

    让表与表产生关系，从而保证数据的正确性

    ```sql
    创建表是添加外键
    外键列 数据类型;
    constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
    删除外键
    alter table 表名 drop foreign key 外键名称;
    创建表后添加外键
    alter table 表名 add constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
    添加级联更新和级联删除
    constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称) on update cascade on delete cascade;
    ```

数据库的设计

1. 多表之间的关系

    1. 分类：

        1. 一对一（了解）
            1. 如：人和身份证
        2. 一对多（多对一）
            1. 如：部门和员工
        3. 多对多
            1. 如：学生和课程

    2. 实现关系：

        1. 一对多（多对一）

            实现方式：在多的一方建立外键，指向一的一方的主键

        2. 多对多

            实现方式：需要借助第三张中间表，中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键

2. 数据库设计的范式

    1. 分类
        1. 第一范式(1NF)：每一列都是不可分割的原子数据项
        2. 第二范式(2NF)：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）
        3. 第三范式(3NF)：在2NF的基础上，任何非主属性不依赖于其他非主属性（在2NF基础上消除传递依赖）
    2. 概念
        1. 函数依赖：A-->B，如果通过A属性（属性组）的值，可以确定唯一B属性的值，则称B依赖于A
        2. 完全依赖：A-->B，如果A是一个属性组，则B属性值的确定需要依赖于A属性组中所有的属性值
        3. 部分函数依赖：A-->B，如果A是一个属性组，则B属性值的确定只需要依赖于A属性组中某一些值即可
        4. 传递函数依赖：A-->B,B-->C，如果通过A属性（属性组）的值，可以确定唯一B属性的值，再通过B属性（属性组）的值可以确定唯一C属性的值，则称C传递函数依赖A
        5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性（属性组）为该表的码

### 数据库的备份和还原

语法

```sql
备份：mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
还原：
登录数据库
创建数据库
使用数据库
执行文件。source 保存备份的路径
```

### 多表查询

分类：

1. 内连接查询

    1. 隐式内连接：使用where条件消除无用数据
    2. 显示内连接：`select 字段列表 from 表名1 [inner] join 表名2 on 条件`

2. 外连接查询

    ```sql
    左外连接
    select 字段列表 from 表1 left [auter] join 表2 on 条件;
    查询的是左表所有的数据以及其交集部分
    
    右外连接
    select 字段列表 from 表1 right [auter] join 表2 on 条件;
    查询的是右表的所有数据以及其交集部分
    ```

3. 子查询

    概念：查询中嵌套查询，称嵌套查询为子查询

    子查询不同情况

    1. 子查询的结果是单行单列的
    2. 子查询的结果是多行单列的
    3. 子查询的结果是多行多列的

### 事务

1. 事务的基本介绍
    1. 概念：如果一个包含多个步骤的操作 ，被事务管理，那么这些操作要么同时成功，要么同时失败
    2. 操作：
        1. 开启事务 start transaction
        2. 回滚 roollback
        3. 提交 commit
    3. mysql数据库中，事务（增删改）默认会自动提交
2. 事务的四大特征：
    1. 原子性：是不可分割的最小单位，要么同时成功，要么同时失败
    2.  持久性：当事务提交或回滚后，数据库会持久化的保持数据
    3. 隔离性：多个事务之间。相互独立。
    4. 一致性：事务操作前后，数据总量不变
3. 事务的隔离级别
    1. 概念：多个事务之间隔离的，相互独立的，如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决
    2. 存在问题：
        1. 脏读：一个事务，读取到另一个事务中没有提交的数据
        2. 不可重复读（虚读）：在同一个事务中，两此读取到的数据不一样
        3. 幻读：一个事务操作（DML）数据表中所有记录，另一个事务添加一条数据，则第一个事务查询不到自己的修改
    3. 隔离级别：
        1. read uncommitted ：读未提交
            1. 产生的问题：脏读、不可重复读、幻读
        2. read committed ：读已提交
            1. 产生的问题：不可重复读、幻读
        3. repeatable read ：可重复读
            1. 产生的问题：幻读
        4. serializable: 串行化
            1. 可以解决所有问题
        5. 隔离级别从小到大安全性越来越高，但效率越来越低
        6. 数据库查看隔离级别：`select @@tx_isolation`
        7. 数据库设置隔离级别： `set global transaction isolation level 级别字符串`

### 管理用户

1. 管理用户

    1. 添加用户

        ```sql
        create user '用户名'@'主机名' identified by '密码';
        ```

    2. 删除用户

        ```sql
        drop user '用户名'@'主机名';
        ```

    3. 修改用户密码

        ```sql
        update user set password = password('新密码') where user = '用户名';
        set password for '用户名'@'主机名' = password('新密码');
        ```

    4. 查询用户

        ```sql
        切换到mysql数据库
        select * from user;
        ```

2. 权限管理

    1. 查询权限

        ```sql
        show grants for '用户名'@'主机名';
        ```

        

    2. 授予权限

        ```sql
        grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
        ```

        

    3. 撤销权限

        ```sql
        revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
        ```

    ---

    

## JDBC

概念：Java DataBase Connectivity Java 数据库连接， Java语言操作数据库

本质：官方定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

步骤：

1. 导入jar包
2. 注册驱动
3. 获取数据库连接对象Connection
4. 定义sql
5. 获取执行sql语句的对象Statement
6. 执行sql，接受返回结果
7. 处理结果
8. 释放资源

### 各个对象：

1. DriverManager：驱动管理对象

    ```java
    // 获取数据库连接
    static Connection getConnection(String url, String user, String password)
        url:指定连接的路径
            jdbc:mysql://ip地址:端口号/数据库名称
    ```

2. Connection：数据库连接对象

    ```java
    // 获取执行sql的对象
    Statement createStatement()
    PreparedStatement prepareStatement(String sql)
    // 管理事务
    开启事务: void setAutoCommit(boolean autoCommit) 方法参数设置为false，及开启事务
    提交事务: commit()
    回滚事务: roolback()
    ```

3. Statement：执行sql的对象

    ```java
    执行sql
    boolean execute(String sql): 可以执行任意的sql
    int executeUpdate(String sql): 执行DML(insert、update、delete)、DDL语句
        // int 返回影响的行数，通过这个影响判断DML语句是否执行成功
    ResultSet executeQuery(String sql): 执行DQL(select)语句
    ```

    

4. ResultSet：结果集对象，封装查询结果

    ```java
    boolean next() 游标向下一行，判断当前行是否是最后一行m
    getXxx() 获取数据 // Xxx代表数据类型
    ```

    

5. PreparedStatement：执行sql的对象

    sql注入问题：在拼接mysql是，有一些sql的特殊关键字参与字符串的拼接。会造成安全问题

    解决sql注入问题，使用PreparedStatement对象来解决

    预编译sql：参数使用？替代

    给？赋值

    ```java
    setXxx(参数1， 参数2)
        参数1，？的位置
        参数2，？的值
    ```

### JDBC控制事务

```java
开始事务：调用该方法，参数设置为false，及开启事务
setAutoCommit(boolean autoCommit)
提交事务：当所有sql都执行完提交事务
commit()
回滚事务：在catch中回滚事务
rollback()
```

### 数据库连接池

1. 概念：其实就是一个容器（集合），存放数据库连接的容器

    当系统初始化好之后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器

2. 好处：
    1. 节约资源
    2. 高效

3. 实现

    1. 标准接口：DataSource javax.sql包下

        方法：

        1. 获取连接
        2. 归还连接，如果连接对象是从连接池中获取的，那么调用close方法，在不再会关闭连接，而是归还连接

    2. 一般不去实现它，有数据库厂商来实现

        1. C3P0：数据库连接池技术
        2. Druid：数据库连接池实现技术，由阿里巴巴提供

### Spring JDBC

Spring框架对JDBC的简单封装。提供了一个JDBCTemplate对象简化JDBC的开发

步骤：

1. 导入jar包

2. 创建JdbcTemplate对象。依赖于数据源DataSource

3. 调用JdbcTemplate的方法来完成操作

    ```java
    update() 执行DML语句。增、删、改
    queryForMap() 查询结果将结果封装为map集合
    queryForList() 查询结果将结果封装为list集合
    query() 查询结果，将结果封装为JavaBean对象
    queryForObject() 查询结果，将结果封装为对象
    ```


## Servlet

概念：运行在服务器端的小程序

​	servlet就是一个接口，定义了java类被浏览器访问到的规则

### 执行原理：

1. 当服务器接收到客户端浏览器请求时，会解析请求URL路径，获取访问的Servlet的资源路径
2. 查找web.xml文件，是否有对应的<url-pattern> 标签体内容
3. 如果有，则在找到对应的<servlet-class> 全类名
4. tomcat 会将字节码文件加载进内存，并且创建其对象
5. 调用方法

### Servlet中的生命周期

1. 被创建：执行init方法，只执行一次

    1. Servlet什么时候被创建

        1. 默认情况下，第一次被访问时，Servlet被创建

        2. 可以配置执行Servlet的创建时机

            ```xml
            <!--第一次访问时创建,在web.xml中的servlet标签中添加，值为负数-->
            <!--在服务器启动时创建,在web.xml中的servlet标签中添加，值为0或正整数-->
            <load-on-startup> </load-on-startup>
            ```

    2. Servlet的init方法，只执行一次，说明一个Servlet在内存中只能存在一个对象，Servlet是单例的

        1. 多个用户同时访问，可能存在线程安全问题
        2. 解决：尽量不要再Servlet中定义成员变量。即使定义了成员变量，也不要修改值

2. 提供服务：执行service方法，执行多次

    1. 每次访问Servlet时，Servlet方法都会被调用一次

3. 被销毁：执行destroy方法，只执行一次

    1. Servlet被销毁时执行，服务器关闭时，Servlet被销毁。
    2. 只有服务器正常关闭时，才会执行destroy方法

Servlet3.0以上版本支持使用注解配置，可以不需要web.xml

### Servlet体系结构

1. GenericServlet：将Servlet接口中其他的方法做了默认空实现，只将service()方法作为抽象（不常用）
2. HttpServlet：对http协议的封装，简化操作
    1. 定义类继承HttdServlet
    2. 复写doGet/goPost方法

### Servlet相关配置

1. urlpattern：servlet访问路径
    1. 一个Servlet可以定义多个访问路径
    2. 路径定义规则：
        1. /xxx
        2. /xxx/xxx
        3. *.后缀

### Http

概念：Htype Text Transfer Protocol 超文本传输协议

- 传输协议：定义了客户端和服务端通信时，发送数据的格式

特点：

1. 基于TCP/IP的高级协议
2. 默认端口号：80
3. 基于请求/响应模型的：一次请求对应一次响应
4. 无状态的：每次请求之间相互独立，不能交互数据

请求消息数据格式：

1. 请求行

    1. 请求方式 请求url 请求协议/版本

        ```
        Get:
        请求参数在请求行中，在url后
        请求的url长度时有限制的，post则没有
        不太安全
        Post：
        请求参数在请求体中
        ```

2. 请求头

    1. 请求头名称：请求头值

    2. 常见的请求头：

        ```
        User-Agent：浏览器告诉服务器，浏览器的版本信息，在服务器端获取该头信息，解决浏览器的兼容问题
        Referer：告诉服务器，从哪来
        作用：
        防盗链
        统计工作
        ```

        

3. 请求空行

    1. 空行，用于分割post请求的请求头和请求体的

4. 请求体

    1. 封装post请求消息的请求参数

### Request

1. request和response对象的原理
    1. request和response对象是由服务器创建的，我们来使用他们
    2. request对象是来获取请求消息，response对象是来设置响应消息
    
2. request功能：

    1. 获取请求消息数据

        1. 获取请求行数据

            ```java
            // 获取请求方式
            String getMethod()
            // 获取虚拟目录
            String getContextPath()
            // 获取Servlet路径
            String getServletPath()
            // 获取get方式请求参数
            String getQueryString()
            // 获取请求的URI
            // URL 统一资源定位符 URI 统一资源标识符
            String getRequestURI()
            StringBuffer getRequestURL()
            // 获取协议和版本
            String getProtocol()
            // 获取客户机的ip地址
            String getRemoteAddr()
            ```

        2. 获取请求头数据

            ```java
            String GetHeader(String name) // 通过请求头名称获取请求头的值
            Enumeration<String> getHeaderNames() // 获取所有请求头名称
            ```

        3. 获取请求体数据

            只有POST请求方式，才有请求体，在请求体中封装了POST请求的请求参数

            ```java
            // 获取流对象
            BufferedReader getReader() // 获取字符输入流，只能操作字符数据
            ServletInputStream getInputStream() // 获取字节输入流，可以操作所有类型数据
            // 从流对象中获取数据
            
            ```

    2. 其他功能

        ```java
        // 获取请求参数通用方式 无论是get还是post请求方式都可以使用下列f
        String getParameter(String name) // 根据参数名称获取参数值
        String[] getParamterValues(String name) // 根据参数名称获取参数值的数据
        Enumeration<String> getParamterValues() // 获取所有请求的参数名称
        Map<String, String[]> getParamterMap() // 获取所有参数的map集合
        request.setCharacterEncoding("utf-8") // 设置request的编码
            
        // 请求转发 一种在服务器内部的资源跳转方式
        RequestDispatcher getRequestDispatcher(String path) // 通过request对象换取请求转发器对象
        forward(ServletRequest request, ServletResponse response) // 使用RequestDispatcher对象来进行转发
        /* 特点：
            1.浏览器地址栏路径不发生改变
            2.只能转发到当前服务器内部资源中
            3.转发是一次请求
        */
            
        // 共享数据
        /*
        域对象：一个有作用范围的对象，可以在范围内共享数据
        request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
        */
        setAttribute(String name,Object obj) // 存储数据
        getAttribute(String name) // 通过键获取值
        removeAttribute(String name) // 通过键移除键值对
            
        // 获取ServletContext
        ServletContext getServletContext()
        ```
        
        

### Response

#### 响应消息：服务器端发送给客户端的数据

数据格式：

1. 响应行
   1. 组成：协议/版本 响应状态码 状态码描述
   2. 响应状态码：服务器告诉客户端浏览器本次请求和响应的一个状态
      1. 状态码都是3位数字
      2. 分类
         1. 1xx：服务器接受客户端消息，但没有接收完成，等待一段时间后，发送1xx状态码
         2. 2xx：成功
         3. 3xx：重定向。302（重定向），304（访问缓存）
         4. 4xx：客户端错误。404（请求路径没有对应的资源），405（请求方式没有对应的doxxx方法）
         5. 5xx：服务器端错误。500（服务器内部出现异常）
2. 响应头
   1. 格式：头名称:值
   2. 常见的响应头：
      1. Content-Type：服务器告诉客户端本次响应体数据格式以及编码格式
      2. Content-disposition：服务器告诉客户端以什么格式打开响应体数据
         1. 值：
            1. in-line：默认值，在当前页面内打开
            2. attachment；fileaname=xxx：以附件形式打开响应体，文件下载
3. 响应空行
4. 响应体

#### Response对象

功能：设置响应消息

1. 设置响应行

   1. 格式：HTTP/1.1 200 ok
   2. 设置状态码：`setStatus(int sc)`

2. 设置响应头：`setHeader(String name,String value)`

   ```java
   // 设置重定向的两种方式
   1. 设置状态码为302，设置响应头
   setStatus(302);
   setHeader("资源路径");
   2.直接重定向
   sendRedirect("资源路径");
    
   /*
    转发的特点：forward
    1.转发地址栏路径不变
    2.转发只能访问当前服务器下的资源
    3.转发是一次请求，能使用request对象来共享数据
    
    重定向的特点：redirect
    1.地址栏发生变化
    2.可以访问其他站点的资源
    3.重定向是两次请求，不能使用request对象来共享数据
   */
   ```

   

3. 设置响应体

   1. 获取输出流
      1. 字节输出流：` ServletOutputStream getOutputStream()`
      2. 字符输出流：`PrintWriter getWriter()`
      3. 乱码问题：设置该流的默认编码和告诉浏览器响应体使用的编码：`setContentType("text/html; charset=utf-8")`
   2. 使用输出流，将数据输出到客户端浏览器

### ServletContext对象

概念：代表整个web应用，可以和程序的容器（服务器）来通信

获取：(两种方法获取的是同一个对象)

1. 通过request对象获取
   1. `request.getServletContext()`
2. 通过Httpservlet获取
   1. `this.getServletContext()`

功能：

1. 获取MIME类型

   1. MIME类型：在互联网通信过程中定义的一种文本数据类型
      1. 格式：大类型/小类型
   2. 获取：`String getMimeType(String file)`

2. 域对象：共享数据

   ```java
   setAttribute(String name, Object value)
   getAttribute(String name)
   removeAttribute(String name)
   /*
   ServletContext对象范围：所有用户所有请求的数据
   */
   ```

3. 获取文件的真实（服务器）路径

   1. 方法：`String getRealPath()`

### 会话技术

1. 会话：浏览器第一次给服务器资源发送请求，会话建立，直到有一方断开为止
2. 功能：在一次会话的范围内的多次请求间，共享数据
3. 方式：
   1. 客户端会话技术：Cookie
   2. 服务端会话技术：Session

### Cookie

1. 概念：客户端会话技术，将数据保存到客户端  

2. 使用步骤：
   1. 创建Cookie对象，绑定数据

      `new Cookie(String name, String value)`

   2. 发送Cookie对象

      `response.addCookie(Cookie cookie)`

   3. 获取Cookie，拿到数据i

      `request.getCookies())`

3. 实现原理

   基于响应头set-cookie和请求头cookie实现的

4. 注意

   1. 默认情况下，当浏览器关闭后，Cookie数据被销毁

   2. 设置Cookie的生命周期

      `setMaxAge(int seconds)`

      1. 正数：将Cookie数据写到硬盘中。持久化存储，还代表cookie的存活时间
      2. 负数：默认值，浏览器关闭后，cookie数据被销毁
      3. 零：删除cookie信息

   3. tomcat 8版本之后，默认支持中文cookie

   4. 默认情况下，cookie不能共享

      `setPath(String path)` 设置cookie的获取范围。默认情况下，范围是当前虚拟目录

      设置为 “/” 则表示当前服务器所有项目都能够共享cookie

      不同服务器间cookie共享

      `setDoamin(String path)` 如果设置一级域名相同，那么多个服务器间cookie可以共享

5. 特点

   1. cookie储存数据在客户端浏览器
   2. 浏览器对于单个cookie的大小有限制以及对同一域名下的总cookie数量也有限制
   3. 作用：
      1. cookie一般用于存储少量不太敏感的数据
      2. 在不登陆的情况下对客户端的身份识别

### Session

1. 概念：服务端会话技术，在一次会话的多次请求间共享数据，将数据保存在服务端的对象中。

2. HttpSession对象：

```java
// 获取Session对象
request.getSession()
// 方法
Object getAttribute(String name)
void setAttribute(String name, Object value)
void removeAttribute(String name)
```

3. 原理
   - Session的实现是依赖与Cookie的
   
4. 注意

   1. 客户端关闭，服务端不关闭，两次获取的Session对象不是同一个，需要相同，则需要

      1. 创建Cookie，键设为JSESSIONID，设置最大存活时间，让cookie持久化保存

   2. 服务器关闭，客户端不关闭，两次获取的Session不是同一个，但是要确保数据不丢失，需要

      1. Session的钝化：在服务器正常关闭之前，将Session对象序列化到硬盘上
      2. Session的活化：在服务器启动后，将Session文件转换为内存中的Session对象

   3. Session被销毁是在

      1. 服务器被关闭后

      2. Session对象调用invalidate()

      3. Session默认失效时间：30分钟

         1. 选择性修改

            ```xml
            <session-config>
            	<session-timeout>数值（单位分钟）</session-timeout>
            </session-config>
            ```

5. 特点：

   1. session用于存储一次会话的多次请求的数据，存在服务端
   2. session可以存储任意数据的类型，任意大小的数据
   3. 与cookie的区别：
      1. session存储数据在服务端，cookie在客户端
      2. session没有数据大小的限制，cookie有
      3. session数据安全，cookie相对于不安全

## JSP

1. 概念：Java  Server Pages：java服务器端页面

   一个特殊的页面，既可以定义html代码，也可以定义java代码

   用于简化书写

2. 原理

   JSP本质上就是一个Servlet

3. JSP脚本：JSP定义Java代码的方式
   1. <%  代码 %> 定义的java代码，在service方法中，service方法中可以定义什么，该脚本就可以定义什么
   2. <%! 代码 %> 定义的java代码，在jsp转换后的java类的成员位置
   3. <%=  代码 %>  定义的java代码会输出到页面上。输出语句可以定义什么，该脚本就可以定义什么

4. 内置对象

   1. 在jsp页面中不需要获取和创建，可以直接使用的对象

   2. 一共有9个内置对象:

      1. request

      2. response

      3. out：字符输出流对象，可以将数据输出到页面上，和response.getWriter()类似

         response.getWriter()和out.write()的区别：

         在tomcat服务器真正给客户端作出响应之前，会先找response缓冲区数据，再找out缓冲区数据。response数据输出永远在out之前
         
      4. pageContext
      
      5. session
      
      6. application
      
      7. page
      
      8. config
      
      9. exception
      
         | 变量名      | 真实类型            | 作用                                        |
         | ----------- | ------------------- | ------------------------------------------- |
         | pageContext | PageContext         | 当前页面共享资源，还可以获取其他8个内置对象 |
         | request     | HttpServletRequest  | 一次请求访问多个资源                        |
         | session     | HttpSession         | 一次会话的多个请求间                        |
         | application | ServletContext      | 所有用户间共享数据                          |
         | response    | HttpServletResponse | 响应对象                                    |
         | page        | Object              | 当前页面（Servlet）的对象                   |
         | out         | JspWriter           | 输出对象，将数据输出到页面上                |
         | config      | ServeltConfig       | Servelt的配置对象                           |
         | exception   | Throwable           | 异常对象                                    |
      
      
   
5. 指令

   - 作用：用于配置JSP页面，导入资源文件

   - 格式：`<%@ 指令名称 属性名1=属性值1 ...%>`

   - 指令名称：

     1. page：配置jsp页面的
        1. contentType
           - 设置响应体的html类型以及字符集
           - 设置当前jsp页面的编码
        2. import：导包
        3. errorPage：错误页面，当前页面发生异常后，会自动跳转指定错误页面
        4. isErrorPage：表示当前页面是否是错误页面
           - true：是，可以使用内置对象exception
           - false：否，默认值。不可以使用内置对象exception
     2. include：页面包含的。导入页面的资源文件
        - `<%@include file="xxx" %>`

     1. taglib：导入资源
        - `<%@ taglib prefix="自定义前缀" uri="" %>`

6. 注释

   1. `<!-- -->` ：只能注释html代码
   2. `<%-- --%>`：可以注释所有

### MVC模式

1. 组成
   1. M：Model，模型
      - 完成具体的业务操作
   2. V：View，视图
      - 展示数据
   3. C：Controller，控制器
      - 获取用户的输入
      - 调用模型
      - 将数据交给视图展示

2. 优缺点

   1. 优点：
      1. 耦合性低，方便维护，可利于分工合作
      2. 重用性高
   2. 缺点：
      1. 使得项目架构变得复杂，对开发人员要求高

   

### EL表达式

- 概念：Expression Language 表达式语言

- 作用：替换和简化jsp页面中java代码的编写

- 语法：`${表达式}`

- 注意：

  - jsp默认支持el表达式，若要忽略el表达式
    - 在page中设置 `isELIgnored="true"` 忽略当前jsp页面所有的表达式
    - `\${表达式}` 忽略当前这个el表达式

- 作用：

  1. 运算

     - 运算符：
       1. 算术运算符：`+	-	*	/(div)	%(mod)`
       2. 比较运算符：`>	<	<=	>=	==	!=`
       3. 逻辑运算符：`&&(and)	||(or)	!(not)`
       4. 空运算符：
          1. `empty` 用于判断字符串、集合、数组对象是否为null并且长度是否为0
          2. `not empty` 用于判断字符串、集合、数组对象不为空并且长度不为0

  2. 获取值

     1. el表达式只能从与域对象中获取值

     2. 语法：

        1. `${域名称.键名}` 从指定域中获取指定键的值

           域名称：
  
           1. pageScope	--> pageContext
           2. requestScope --> request
           3. sessionScope --> session
           4. applicationScope --> application(ServletContext)

        2. `${键名}` 表示依次从最小的域中查找是否有该键对应的值，直到找到为止

        3. 获取对象、List集合、Map集合的值
  
           1. 对象：`${域名称.键名.属性名}`
           2. List集合：`${域名称.键名[索引]}`
           3. Map集合：`${域名称.键名.key名称}`
           
        4. 隐式对象
        
           1. el表达式有11个隐式对象
           2. pageContext：
              1. 获取jsp其他8个内置对象
                 1. `${pageContext.request.contextPath}` 动态获取虚拟目录

### JSTL

1. 概念：JavaServer Pages Tag Library JSP标准标签库

2. 作用：用于简化和替换jsp页面上的java代码

3. 常用的JSTL标签

   1. if：相当于java的if

      ```jsp
      <c:if test=""></c:if>
          属性：
          test 必须属性 接受boolean表达式
          如果表达式为true，则显示if标签的内容，如果为false，则不显示
      ```

      

   2. choose：相当于java的switch

      ```jsp
      <c:choose>
      	<c:when test=""></c:when>
      </c:choose>
      when 标签做判断
      otherwise标签做其他情况的声明
      ```

      

   3. foreach：相当于java的for循环

      ```jsp
      普通for循环
      <c:foreach begin="" end="" var="" step=""></c:foreach>
      属性
      begin：开始值
      end：结束值
      var：临时变量
      step：步长
      varStatus：循环状态对象
      	index：容器中元素的索引
      	count：循环次数
      
      增强for循环
      <c:foreach items="" var=""></c:foreach>
      items：容器对象
      var：容器中元素的临时变量
      ```

      

### 三层架构

1. 界面层（表示层）：用户看得到的界面。用户可以通过界面上的组合和服务器进行交互
2. 业务逻辑层：处理业务逻辑
3. 数据访问层：操作数据存储文件



### Filter

web中的过滤器：当访问服务器的资源时，过滤器可以将请求拦截下来，完成一些特殊的功能

过滤器的作用：

- 一般用于完成通用的操作。如：登陆验证、统一编码和处理、敏感字符过滤

步骤：

1. 定义类实现接口Filter
2. 重写方法
3. 配置拦截路径

过滤器细节：

1. web.xml配置 
2. 过滤器执行流程
3. 过滤器生命周期方法
   1. init方法：在服务器启动后，会创建Filter对象，然后调用init方法（用于加载资源）
   2. destroy方法：服务器关闭后，Filter对象被销毁。如果服务器正常关闭，则会执行destroy方法（用于释放资源）
   3. doFilter方法：每一次拦截资源时，都会执行
4. 过滤器配置详解
   1. 拦截路径配置：
      1. 具体资源路径
      2. 拦截目录
      3. 后缀名拦截
      4. 拦截所有资源
   2. 拦截方式配置：资源被访问的方式
      1. 注解配置：设置dispatcherTypes属性
         1. REQUEST：默认值。浏览器直接请求资源
         2. FORWARD：转发访问资源
         3. INCLUDE：包含访问资源
         4. ERROR：错误跳转资源
         5. ASYNC：异步访问资源
      2. web.xml配置
         - 设置<dispatcher></dispatcher>
5. 过滤器链（配置多个过滤器）
   - 执行顺序：如果有两个过滤器：过滤器1和过滤器2
     - 过滤器1->过滤器2->资源->过滤器2->过滤器1
   - 过滤器先后顺序问题：
     1. 注解配置：按照类名的字符串比较规则比较，值小的先执行
     2. web.xml配置：<filter-mapping>谁定义在上面，谁先执行

### Listener：监听器

概念：web的三的组件之一

事件监听机制：

- 事件：一件事情
- 事件源：事件发生的地方
- 监听器：一个对象
- 注册监听：将事件、事件源、监听器绑定在一起。当事件源上发生某个事件后，执行监听器代码

ServeltContextListener：监听ServletContext对象的创建和销毁

```java
void contextDestroyed(ServeltContextEvent sce) ServletContext对象被销毁之前会调用该方法
void contextInitialized(ServletContextEvent sce) ServeltContext对象创建后被d
```



## java对象转json

- JSON解析器：jackson

1. JSON转为java对象

2. java对象转为JSON

   转换方法

   ```java
   writeValue(参数1,obj):
   参数1：
       File 将obj对象转换为json字符串，并保存到指定的文件中
       Writer：将obj对象转换为json字符串，并将json数据填充到字符输出流中
       OutputStream：将obj对象转换为json字符串，并将json数据填充到字节输出流中
       
   writeValueAsString(obj) 将对象转换为字符串
   ```

   注解：

   1. @JsonIgnore：排除属性
   2. @JsonFormat：属性值的格式化

## Redis

概念：redis是一款高性能的noSql数据库

###  redis的数据结构

1. redis存储的是：key, value格式的数据，key都是字符串，value有5中不同的数据结构
   1. value的数据结构
      1. 字符串类型 string
      2. 哈希类型 hash ：map格式
      3. 列表类型 list ：Linkedlist格式
      4. 集合类型 set
      5. 有序集合类型 sortedset

### 命令操作

1. 字符串类型 string
   1. 存储：set key value
   2. 获取：get key
   3. 删除：del key
2. 哈希类型 hash
   1. 存储：hset key field value
   2. 获取：hget key field
   3. 获取所有的field：hgetall key
   4. 删除：hdel key field
3. 列表类型 list 可以添加一个元素到列表的头部或尾部
   1. 添加：
      1. lpush key value 将元素添加到列表左边
      2. rpush key value 将元素添加到列表右边
   2. 获取：
      1. lrange key start end 范围获取
   3. 删除：
      1. lpop key 删除列表最左边的元素，并将元素返回
      2. rpop key 删除列表左右边的元素，并将元素返回
4. 集合类型 set 不允许重复元素
   1. 存储：sadd key value
   2. 获取：smembers key 获取set集合中所有元素
   3. 删除：srem key value 删除set集合中某个元素
5. 有序集合 sortedset 不允许重复元素
   1. 存储 zadd key score value
   2. 获取 zrange key start end
   3. 删除 zrem key value
6. 通过命令
   1. keys *： 查询所有的键
   2. type key：获取键对应的value类型
   3. del key：删除指定的key value

### 持久化

1. redis是一个内存数据库，当redis服务器重启，数据会丢失，可以将redis内存中的数据持久化保存到硬盘中
2. redis持久化机制
   1. RDB：默认方式
      1. 在一定的间隔事件中，检测key的变化情况，然后持久化数据
   2. AOF：日志记录的方式，可以记录每一条命令的操作。每一次命令操作，持久化数据
      1. appendfsync always：每一次操作都会进行持久化
      2. appendfsync everysec：每隔一秒操作一次
      3. appendfsync no：不进行持久化
