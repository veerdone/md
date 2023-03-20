# Spring

## Spring是什么

spring是分层的Java Se/EE应用的full-stack轻量级开源框架，以IoC(Inverse of Control：反转控制)和AOP(Aspect OrientedProgramming：面向切面编程)为内核

提供了展现层SpringMVC和持久层Spring JDBCTemplate以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的Java EE企业应用开源框架

## Spring优势

1. 方便解耦，简化开发
2. AOP编程的支持
3. 声明式事务的支持
4. 方便程序的测试
5. 方便集成各种优秀框架
6. 降低JavaEE API的使用难度
7. Java源码是经典学习范例

## Spring开发步骤

1. 导入spring坐标
2. 创建bean
3. 创建applicationContext.xml
4. 配置xml
5. 创建ApplicationContext对象getBean



## Sping配置文件

### Bean标签范围配置

scope：指对象的作用范围

| 取值范围       | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| singleton      | 默认值，单例的                                               |
| prototype      | 多例的                                                       |
| request        | web项目中，Spring创建一个Bean对象，将对象存入request域中     |
| session        | web项目中，Spring创建一个Bean对象，将对象存入session域中     |
| global session | web项目中，应用在Portlet环境，如果没有Protlet环境那么gloalSession相当于session |

1. 当scope的取值为singleton时
   - Bean的实例化个数：1个
   - Bean的实例化时机：当Spring核心文件被加载时，实例化配置的Bean实例
   - Bean的生命周期：
     - 对象创建：当应用加载，创建容器时，对象就被创建了
     - 对象运行：只要容器在，对象一直活着
     - 对象销毁：当应用卸载，销毁容器，对象就被销毁了
2. 当scope的取值为prototype时
   - Bean的实例化个数：多个
   - Bean的实例化时机：当调用getBean()方法时实例化Bean
   - Bean的生命周期：
     - 对象创建：当使用对象时，创建新的对象实例化
     - 对象运行：只要对象在使用中，就一直活着
     - 对象销毁：当对象长时间不用时，被java的垃圾回收器回收了

3. init-method：指定类中的初始化方法名称
4. destroy-method：指定类中销毁方法名称



### Bean实例化三种方法

1. 无参构造方法实例化
2. 工厂静态方法实例化
3. 工厂实例方法实例化



### Bean的依赖注入

依赖注入(Dependency Injectioni)：是Spring框架核心IOC的具体实现

1. set方法注入

   1. 在需要注入对象的类中创建getter方法

   2. P命名空间的注入

      1. 引入p命名空间

         `xmlns:p="http://www.springframework.org/schem/p"`

      2. 通用注入

         `<bean id="xxxx" class="xxxx" p:xxx-ref="注入对象的容器id"/>`

   3. 通用注入

      ```xml
      <bean id="xxx" class="xxx">
      	<property name="xxx" ref="注入对象的容器id" />
      </bean>
      ```

2. 构造方法注入

   1. 在需要注入对象的类中创建有参构造，传入值为注入对象

   2. 通用注入

      ```xml
      <bean id="xxx" class="xxx">
      	<constructor-arg name="xxx" ref="注入对象"/>
      </bean>
      ```


### Bean的依赖注入的数据类型

1. 普通数据类型

   ```xml
   <bean id="xxx" class="实例化的Bean的全类名">
   	<property name="成员变量名" value="xxx"/>
       <property name="成员变量名" value="xxx"/>
   </bean> 
   ```

2. 引用数据类型

3. 集合数据类型

   ```xml
   <bean id="xxx" class="实例化的Bean的全类名">
       <!--list集合-->
       <property name="集合名称">
       	<list>
           	<value></value>
           </list>
       </property>
           <!--map集合-->
       <property name="集合名称">
   		<map>
           	<entry key/ket-ref="" value/value-ref=""/>
           </map>
       </property>
               <!--properties集合-->
       <property name="集合名称">
   		<props>
           	<prop key="xxx">xxx</prop>
           </props>
       </property>
   </bean>
   ```


### 引入其他配置文件（分模块开发）

`<import resource="xxxx.xml"/>`



## ApplicationContext的实现类

1. ClassPathXmlApplicationContext

   从类的根路径下加载配置文件，推荐这种方式

2. FileSystemXmlApplicationContext

   从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置

3. AnnotationConfigApplicationContext

   当使用注解配置容器对象时，需要使用此类来创建spring容器，用来读取注解

## Spring配置数据源

### 数据源（连接池）的作用

### Spring配置数据源

将DataSource的创建权交由Spring容器去完成 

## Spring注解开发

### Spring原始注解

Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，注解替代xml配置文件可以简化配置，提高开发效率

**Spring原始注解主要是替代<Bean>的配置**

| 注解           | 说明                                           |
| -------------- | ---------------------------------------------- |
| @Component     | 使用在类上用于实例化Bean                       |
| @Controller    | 使用在Web层类上用于实例化Bean                  |
| @Service       | 使用在Service层类上用于实例化Bean              |
| @Repository    | 使用在Dao层类上用于实例化Bean                  |
| @Autowired     | 使用在字段上用于根据类型依赖注入               |
| @Qualifier     | 结合@Autowired一起使用用于根据名称进行依赖注入 |
| @Resource      | 相当于@Autowired+@Qualifier，按照名称进行注入  |
| @Value         | 注入普通属性                                   |
| @Scope         | 标注Bean的作用范围                             |
| @PostConstruct | 使用在方法上标注该方法是、Bean的初始化方法     |
| @PreDestroy    | 使用在方法上标注该方法是Bean的销毁方法         |



使用注解开发时，需要在applicationContext.xml中配置组件扫描，指定哪个包及其子包下的Bean需要进行扫描

`<context:component-scan base-package="xxx"/>`

### Spring新注解

| 注解            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Configuration  | 用于指定当前类是一个Spring配置类，当创建容器时会从该类上加载注解 |
| @ConponentScan  | 用于指定Spring在初始化容器时要扫描的包，作用和Spring的xml配置文件中配置的一样 |
| @Bean           | 使用在方法上，标注该方法的返回值存储到Spring容器中           |
| @PropertySource | 用于加载.properties文件中的配置                              |
| @Import         | 用于导入其他配置类                                           |

## Spring集成Junit

1. 让SpringJunit负责创建Spring容器，但是需要将配置文件的名称告诉他
2. 将需要进行测试Bean直接在测试类中进行测试

### Spring集成Junit步骤

1. 导入spring集成junit的坐标
2. 使用@Runwith注解替换原来的运行期
3. 使用@ContextConfiguration指定配置文件或配置类
4. 使用@Autowired注入需要测试的对象
5. 创建测试方法进行测试



## Spring AOP

### 什么是AOP

AOP为Aspect Oriented Programming的缩写，意思为面向切面编程，是通过预编译方法和运行期动态代理实现程序功能的统一维护的一种技术

### AOP的作用及其优势

作用：在程序运行期间，在不修改源码的情况下对方法进行功能增强

优势：减少重复代码，提高开发效率，并且便于维护

### AOP的底层实现

AOP的底层是通过Spring提供的动态代理技术实现的，在运行期间，Spring通过动态代理技术动态的生成代理对象，代理对象方法执行时进行增强功能的介入，再去调用目标对象的方法，从而完成功能的增强

### AOP的动态代理技术

1. JDK代理：基于接口的动态代理技术
2. cglib代理：基于父类的动态代理技术

### AOP相关概念

1. Target（目标对象）：代理的目标对象
2. Proxy（代理）：一个类被AOP织入增强后，就产生一个结果代理类
3. Joinpoint（连接点）：所谓连接点是指那些被拦截到的点。在Spring中，这些点指的是方法，Spring只支持方法类型的连接点。**说人话就是连接点是指那些被拦截的方法**。
4. Pointcut（切入点）：切入点是指我们要对哪些Joinpoint进行拦截的定义。
5. Advice（通知/增强）：通知是指拦截到Joinpoint之后要做的事情就是通知
6. Aspect（切面）：是切入点和通知的结合
7. Waeving（织入）：是指增强应用到目标对象来创建新的代理对象的过程。Spring采用动态织入，而Aspect采用编译期织入和类装载器织入

### AOP开发明确的事项

1. 需要编写的内容

   1. 编写核心业务代码（目标类的目标方法）
   2. 编写切面类，切面类中有通知（增强功能方法）
   3. 在配置文件中，配置织入关系，即将哪些通知与哪些连接点进行结合

2. AOP技术实现的内容

   Spring框架监控切入方法的执行，一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类型，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行

3. AOP底层使用哪些代理方式

   在Spring中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式

### 基于XML的AOP开发

1. 导入AOP相关坐标
2. 创建目标接口和目标类（内部有切点）
3. 创建切面类（内部有增强方法）
4. 将目标类和切面类的对象创建权交给Spring
5. 在xml文件中配置织入关系

#### 切点表达式

语法： `execution([修饰符] 返回值类型 包名.类名.方法名(参数))`

- 访问修饰符可以省略
- 返回值类型、包名、类名、方法名可以使用*代表任意
- 包名与类名之间一个点 . 代表当前包下的类，两个 .. 代表当前包及其子包下的类
- 参数列表可以使用 .. 代表任意个数，任意类型的参数列表

#### 通知类型

| 名称         | 标签                   | 说明                                                         |
| ------------ | ---------------------- | ------------------------------------------------------------ |
| 前置通知     | \<aop:brfore>          | 用于配置前置通知，指定增强的方法在切入点方法之前执行         |
| 后置通知     | \<aop:after-returning> | 用于配置后置通知，指定增强的方法在切入点方法之后执行         |
| 环绕通知     | \<aop:around>          | 用于配置环绕通知，指定增强的方法在切入点方法之前和之后都执行 |
| 异常抛出通知 | \<aop:throwing>        | 用于配置异常抛出通知，指定增强的方法在出现异常时执行         |
| 最终通知     | \<aop:after>           | 用于配置最终通知，无论增强方法执行是否有异常都会执行         |

#### 切点吧表达式的抽取

当多个增强的切点表达式相同时，可以将切点表达式进行抽取，在增强中使用pointcut-ref属性替代pointcut属性来引用抽取后的切点表达式

### 基于注解的AOP开发

步骤：

1. 创建目标接口和目标类（内部有切点）
2. 创建切面类（内部有增强方法）
3. 将目标类和切面类的对象创建交给Spring
4. 在切面类中使用注解配置织入关系
5. 在配置文件中开启组件扫描和AOP的自动代理



| 名称         | 注解            | 说明                                                         |
| ------------ | --------------- | ------------------------------------------------------------ |
| 前置通知     | @Before         | 用于配置前置通知，指定增强的方法在切入点方法之前执行         |
| 后置通知     | @AfterReturning | 用于配置后置通知，指定增强的方法在切入点方法之后执行         |
| 环绕通知     | @Around         | 用于配置环绕通知，指定增强的方法在切入点方法之前和之后都执行 |
| 异常抛出通知 | @AfterThrowing  | 用于配置异常抛出通知，指定增强的方法在出现异常时执行         |
| 最终通知     | @After          | 用于配置最终通知，无论增强方法执行是否有异常都会执行         |



## Spring JdbcTemplate



### 基于XML的声明式事务控制

1. 什么是声明式事务控制

   Spring的声明式事务就是**采用声明的方式来处理事务**。这里的事务，指在配置文件中声明，用在Spring配置文件中声明式的处理事务来代理代码的处理事务

2. 声明式事务处理的作用

   - 事务管理不侵入开发的组件，具体来说，业务逻辑对象就不会意识到正在事务管理之中，事实上也因该如此，因为事务管理属于系统层面的服务，而不是业务逻辑的一部分，如果想要改变事务管理策划，只需要在定义文件中重新配置
   - 不需要事务管理时，只要在设定文件上修改一下，即可移去事务管理，无需改变代码重新编译，方便维护
   - Spring声明式事务控制底层就是AOP

### 基于注解的声明式事务控制

1. 使用@Transactional在需要进行事务控制的类或是方法上修饰，注解可用的属性同xml配置方式
2. 注解使用在类上，那么该类上的所有方法都会使用同一套注解参数配置
3. 使用在方法上，不同的方法采用不同的事务参数配置
4. Xml配置文件文件中要开启事务的注解驱动`<tx:annctation-driven/>`



注解声明式事务控制的配置要点

- 平台事务管理器配置在xml文件中配置
- 事务通知的配置（@Transaction注解配置）
- 事务注解驱动的配置 \<tx:accntation-driven/>





# SpringMVC



## Spring集合web环境

### ApplicationContext应用上下文获取方法

在web项目中，可以使用ServletContextListerner监听web应用的启动，在web应用启动时，就加载Spring的配置文件，创建应用上下文ApplicationContext，将其存储到最大的域servletContext域中，就可以在任意位置从域中获得上下文ApplicationContext对象

### Spring提供获取应用上下文的工具

Spring提供了一个监听器ContextLoaderListener就是对上述功能的封装，该监听器内部加载Spring文件，创建应用上下文对象，并存储到ServletContext域中，提供了一个客户端工具WebApplicationContextUtils供使用者应用上下文对象

## SpringMVC概述

SpringMVC是一种基于java的实现MVC设计模型的请求驱动类型的轻量级Web框架，属于SpringFrameWork的后续产品，已经融合在Spring Web Flow中

通过一套注解，让简单的Java类成为处理请求的控制器，而无需实现任何接口，同时还支持RESTful编程风格的请求

## SpringMVC开发步骤

1. 导入SpringMVC相关坐标
2. 配置SpringMVC核心控制器DispatherServlet
3. 创建Controller类和视图界面
4. 使用注解配置Controller类内业务方法的映射地址
5. 配置SpringMVC核心文件Spring-mvc.xml

## SpringMVC的执行流程

1. 用户发送请求至前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器
3. 处理器映射器找到具体的处理器（可以根据xml配置、注解进行查找），生成处理器对象及处理器拦截器（如果有则生成）一并返回给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter处理器适配器
5. HandlerAdapter经过适配器调用具体的处理器（Controller，也叫后端控制器）
6. Controller执行完成返回ModelAndView
7. HandlerAdapte将controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServlet将ModelAndView返回给DispatcherServlet
9. ViewReslover解析后返回具体View
10. DispatcherServlet根据View进行渲染（即将模型填充至视图）。DispatcherServlet响应用户

![SpringMVC执行流程](C:\Users\yu\Documents\SpringMVC执行流程.png)

## SpringMVC注解解析

### @RequestMapping

作用：用于建立请求URL和处理请求方法之间的对应关系

位置：

- 类上，请求URL的第一级访问目录。此处不写的话，相当于应用的根目录
- 方法上，请求URL的第二级访问目录，与类上的使用@RequestMapping标注的一级目录一起组成访问虚拟路径

属性：

- value：用于指定请求的URL，它和path属性的作用是一样的
- method：用于指定请求的方式
- params：用于指定限制请求参数的条件，支持简单的表达式，要求请求参数的key和value必须和配置的一摸一样

组件扫描

SpringMVC基于Spring容器，在进行SpringMVC操作时，需要将Controller存储到Spring容器中，使用@Controller注解标注的话，需要使用`<context:component-scan base-package="" />`进行组件扫描

视图解析器配置

SpringMVC有默认组件配置，默认组件都是DispatcherServelt.properties配置文件中配置的，地址是org/springframework/web/servlet/DispatcherServlet.properties，对解析器进行设置

```
REDIRECT_URL_PREFIX = "redirect" 重定向前缀
FORWAWRD_URL_PREFIX = "forward" 转发前缀
prefix = "" 视图名称前缀
suffix = "" 视图名称hou'h
```



## SpringMVC的数据响应方式

1. 页面跳转
   - 直接返回字符串
     - 直接返回字符串：此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转
   - 通过ModelAndView对象返回
2. 回写数据
   - 直接返回字符串
     - 通过SpringMVC框架注入的response对象，使用response.getWriter().print("hello world")回写数据，此时不需要视图跳转，业务方法返回值为void
     - 将需要回写的字符串直接返回，但此时需要通过@ResponseBody注解告知SpringMVC框架，方法返回的字符串不是跳转是直接在http响应体中返回
     
   - 返回对象或集合
   
     - 通过SpringMVC帮助我们对对象或集合进行json字符串的转换并回写，为处理适配器配置消息转换参数，指定使用jackson进行对象或集合的转换
   
       ```xml
       <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
       	<property name="messageConverters">
           	<list>
               	<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
               </list>
           </property>
       </bean>
       ```
   
     - 在方法上添加@ResponseBody就可以返回json格式的字符串，但是配置麻烦，可以使用mvc的注解驱动代替上述配置
     
       ```xml
       <mvc:annotation-driven/>
       ```
     
       在SpringMVC的各个组件中，处理器映射器，处理器适配器，视图解析器被称为SpringMVC的三大组件。使用\<mvc:annotation-driven/>自动加载RequestMappingHandlerMapping和RequestMappingHandlerAdapter，可用在Spring-xml.xml配置文件中使用\<mvc:annotation-driven/>替代注解处理器和适配器的配置
     
       \<mvc:annotation-driven/>默认底层会集成jackson进行对象或集合的json格式字符串的转换

## SpringMVC获得请求数据

SpringMVC可以接收的数据类型

- 基本类型参数
- POJO类型参数
- 数组参数
- 集合类型参数

### 获得基本类型参数

Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配

### 获得POJO类型参数

Controller中的业务方法的POJO参数的 属性名和请求参数的name一致，参数值会自动映射匹配

### 获得集合类型参数

- Controller中的业务方法数组方法名称和请求参数的name一致，参数值会自动映射匹配
- 当使用ajax提交时，可以指定contentType为json格式，那么在方法参数位置使用@RequestBody可以直接接收集合数据而无需使用POJO进行包裹

### 请求数据乱码问题

当post请求时，数据会出现乱码，可以设置过滤器来进行编码的过滤

````xml
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
````

### 参数绑定注解@requestParam

当请求的参数名称与Controller的业务方法参数名称不一致时，就需要通过@requestParam注解显示的绑定

参数

- value：与请求参数名称
- required：此在指定的请求参数是否必须包括，默认是true，提交时如果没有此参数则报错
- defaultValue：当没有指定请求参数时，则使用默认的默认值赋值

### 获得Restful风格的参数

Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件，主要用于客户端和服务器交互的软件，基于这个风格设计的软件可以更简洁、更有层次、更易于实现缓存机制等

Restful风格的请求是使用"url+请求方式"表示一次请求的目的，HTTP协议里面四个表示操作方式的动词如下：

- GET：用于获取资源
- POST：用于新建资源
- PUT：用于更新资源
- DELETE：用于删除资源

### 自定义类型转换器

- SpringMVC默认提供了一些常用的类型转换器，例如客户端提交的字符串转成int型进行参数设置
- 不是所有的数据类型都提供了转换器，没有提供的需要自定义转换器

**自定义转换器的开发步骤**

1. 定义转换器实现Converter接口
2. 在配置文件中声明转换器
3. 在\<annotation-driven>中引用转换器

### 在控制器中使用原生的ServletAPI对象

只需要在控制器的方法参数定义HttpServletRequest和HttpServletResponse对象

### 获取请求头

1. @RequestHeader

   使用@RequestHeader可以获得请求头信息

   属性：

   - value：请求头名称
   - required：是否必须携带此请求头

2. @CookieValue

   使用@CookieValue可以获得指定的Cookie的值

   @CookieValue注解的属性：

   - value：指定cookie的名称
   - required：是否必须携带此cookie

### 文件上传

**文件上传三要素** 

1. 表单项type="file"
2. 表单的提交方式是post
3. 表单的enctype属性是多部分表单形成，乃enctype="multipart/form-data"

**文件上传原理**

1. 当form表单修改为多部份表单时，request.getParameter()将失效
2. enctype="applicaltion/x-www-form-urlencoded"时，form表单的正文内容格式是：key=value&key=value&key=value
3. 当form表单的enctype取值为Mutilpart/form-data时，请求正文内容就变成多部份形式

**单文件上传步骤**

1. 导入fileupload和io坐标

2. 配置文件上传解析器

   ```xml
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <!--上传文件总大小-->
           <property name="maxUploadSize" value="5242800"/>
           <!--上传单个文件的大小-->
           <property name="maxUploadSizePerFile" value="5242800"/>
           <!--上传文件的编码类型-->
           <property name="defaultEncoding" value="UTF-8"/>
       </bean>
   ```

   

3. 编写文件上传代码

**多文件上传**

多文件上传，只需要将页面修改为多个文件上传项，将方法参数MultipartFile类型修改为MultipartFile[]即可



## SpringMVC拦截器

### **拦截器(interceptor)的作用**

SpringMVC的拦截器类似Servlet开发中的过滤器Filter，用于对处理器进行预处理和后处理

将拦截器按一定的顺序联结成一条链，这条链称为拦截器链(interceptor Chain)。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。拦截器也是AOP思想的具体实现

### 拦截器和过滤器的区别

| 区别     | 过滤器                                                  | 拦截器                                                       |
| -------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 使用范围 | 是servlet规范中的一部分，任何Java Web工程都可以使用     | 是SpringMVC框架自己的，只有使用了SpringMVC框架的工程的才能用 |
| 拦截范围 | 在url-pattern中配置了/*之后，可以对所有要访问的资源拦截 | 只会拦截访问的控制器方法，如果访问的是jsp、html、css、image或者js是不会进行拦截的 |

### 拦截器快速入门

自定义拦截器

1. 创建拦截器类实现HandlerInterceptor接口
2. 配置拦截器
3. 测试拦截器的拦截效果

拦截器方法说明

| 方法名            | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| preHandle()       | 方法将在请求处理之前进行调用，该方法的返回值是Boolean类型的，当他返回为false时，表示请求结束，后续的Interceptor和Controller都不会再执行，当返回值为true时就会继续调用下一个Interceptor的preHandle方法 |
| postHandle()      | 该方法是在当前请求进行处理之后被调用，前提是preHandle方法的返回值为true时才能被调用，且他会在DispatcherServlet进行视图返回渲染之前被调用，所以可以在这个方法中对Controller处理之后的ModelAndView对象进行操作 |
| afterCompletion() | 该方法将在请求结束后，而就是在DispatcherServlet渲染了对应的视图之后执行，前提是preHandle方法的返回值为true时才能被调用 |

## SpringMVC异常处理机制

### 异常处理两种方式

1. 使用SpringMVC提供的简单异常处理器SimpleMappingExceptionResolver
2. 实现Spring的异常接口HandlerExceptionResolver自定义自己的异常处理

### 简单异常处理器

SimpleMappingExceptionResolver

SpringMVC已经定义好了该类型转换器，在使用时可以根据项目情况进行相应异常与视图的映射配置

```xml
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <!--默认错误视图-->
        <property name="defaultErrorView" value="error"/>
        <property name="exceptionMappings">
            <map>
                <!--key表示异常类型，value表示错误视图-->
                <entry key="java.lang.ClassCastException" value="error"/>
            </map>
        </property>
    </bean>
```

### 自定义异常处理步骤

1. 创建异常处理器实现HandlerExceptionResolver
2. 配置遗产处理器
3. 编写异常页面
4. 测试异常跳转





# Mybatis

## 开发步骤

1. 添加Mybatis的坐标
2. 创建User数据表
3. 编写User实体类
4. 编写映射文件UserMapper.xml
5. 编写核心文件QqlMapConfig.xml
6. 编写测试类

## Mybatis的Dao层实现

### 代理开发方式

采用Mybatis的代理开发方式实现DAO层的开发

Mapper接口开发方法只需要编写Mapper接口（相当于Dao接口），由Mybatis框架根据接口定义创建接口的动态代理对象，代理对象的方法体同Dao接口实现类方法

Mapper接口开发需要遵循以下规范：

1. Mapper.xml文件中的namespace与mapper接口的全限定名相同
2. Mapper接口方法名和Mapper.xml中定义的每个statement的id相同
3. Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同
4. Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql的resultType的类型相同

## Mybatis映射文件深入

### 动态sql语句

动态sql之<if>

根据实体类的不同取值，使用不同的sql语句来进行查询，比如在id如果不为空时可以根据id查询，如果username不为空时还要加入用户名作为条件

### 抽取动态sql

使用`<sql id="">sql语句</sql>`

引用抽取sql语句

`<include refid=""/>`

### Mybatis映射文件配置

````
<select> 查询
<insert> 插入
<update> 修改
<delete> 删除
<where> where条件
<if> if判断
<foreach> 循环
<sql> sql片段抽取
````

### typeHandlers标签

无论是Mybatis在预处语句中设置一个参数时，还是从结果集中取出一个值时，都会用类型处理器将获取的值转换为java类型。

| 类型处理器         | Java类型                  | JDBC类型                           |
| ------------------ | ------------------------- | ---------------------------------- |
| BooleanTypeHandler | java.lang.Boolean.boolean | 数据库兼容的BOOLEAN                |
| ByteTypeHandler    | java.lang.Byte.byte       | 数据库兼容的NUMERIC或BYTE          |
| ShortTypeHandler   | java.lang.Short.short     | 数据库兼容的NUMERIC或SHORT INTEGER |
| InterTypeHandler   | java.lang.Integer.int     | 数据库兼容的NUMERIC或INTEGER       |
| LongTypeHandler    | java.lang.Long.long       | 数据库兼容的NUMERIC或LONG INTEGER  |

可以重写类型处理器或创建自己的类型处理器来处理不支持的或非标准的类型。

具体方法：实现org.apache.ibatis.type.TypeHandler接口，或继承一个类org.apache.ibatis.type.BaseTypeHandler，然后可以选择性的将它映射到一个JDBC类型

步骤：

1. 定义转换类继承类BaseTypeHandler<T>
2. 覆盖4个未实现的方法，其中setNonNullParameter为java程序设置数据到数据库的回调方法，getNullableResult为查询时mysql的字符串类型转换成java的Type类型的方法
3. 在Mybatis核心配置文件中进行注册
4. 测试转换是否正确

### plugins标签

Mybatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据

步骤：

1. 导入通用PageHelper的坐标
2. 在Mybatis核心配置文件中配置PageHelper插件
3. 进行测试

## Mybatis注解开发

@Insert：实现新增

@Update：实现更新

@Delete：实现删除

@Select：实现查询

@Result：实现结果集封装

@Results：可以与@Result一起使用，封装多个结果

@One：实现一对一结果集封装

@Many：实现一对多结果集封装





# Dubbo

## 架构图

![dubbo架构图](C:\Users\yu\Documents\dubbo架构图.png)

| 节点      | 角色名称                               |
| --------- | -------------------------------------- |
| Provider  | 暴露服务的服务提供方                   |
| Consumer  | 调用远程服务的服务消费方               |
| Registry  | 服务注册与发现的注册中心               |
| Monitor   | 统计服务的调用次数和调用时间的监控中心 |
| Container | 服务运行容器                           |

虚线是异步访问，实现是同步访问

蓝色实线：在启动时完成的任务

红色虚线（实线）：都是程序运行过程中执行的功能

# SpringBoot

## 简介

SpringBoot提供了一种快速开发Spring项目的方式，而不是对Spring功能上的增强

Spring的缺点：

- 配置繁琐
- 依赖繁琐

SpringBoot功能：

- 自动配置
- 起步依赖：依赖传递
- 辅助功能

## 配置文件

配置文件分类

SpringBoot是基于约定的，所以很多配置都有默认值，想要使用自己的配置替换默认配置的话，可以使用application.properties或者application.yml进行配置

- springboot提供了两种配置文件类型：properties和yml/yaml
- 默认配置文件名称：application
- 在同一级目录下优先级：properties > yml > yaml

## yml

### 基本语法

- 大小写敏感
- 数据值之前必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格
- 缩进的空格数据不重要，只要相同层级的元素左侧对齐
- #表示注释，从这个字符一直到行尾，都会被解析器忽略

### 数据格式

- 对象（map）：键值对格式

  ```yml
  person:
    name: zhagnsan
  # 行内写法
  person: {name: zhangsan}
  ```

- 数组：一组按次序排列的值

  ```yml
  address:
    - beijing
    - shanghai
  # 行内写法
  address: [beijing, shanghai]
  ```

- 纯量：单个的、不可再分的值

  ```yml
  msg1: 'hello \n world' #单个引号忽略转义字符
  msg2: "hello \n world" #双引号识别转义字符
  ```

### yml参数引用

```yml
name: lisi
person:
  name: ${name} #引用上面定义的n
```



## 读取配置

1. @Value

   ```java
   @Value("$(name)") // 表达式内的名称要与配置文件内的名称相同
   private String name;
   ```

2. Environment

   ```java
   //创建Environment对象，用@Autowired注入
   @Autowired
   private Environment environment;
   // 通过Environment的方法 getProperty获取相应的值
   environment.getProperty("配置文件中的名称")
   ```

3. @ConfigurationProperties

   ```java
   // 在实体类中加上@ConfigurationProperties注解，进行属性的自动注入
   @ConfigurationProperties(prefix = "person") // prefix的值和配置文件的对象名相同
   public class Person{
       private String name; // 属性名和配置文件中对象的属性值相同
       private int age;
   }
   ```

   

## Profile

1. profile是用来完成不同环境下，配置动态切换功能的
2. profile配置方式
   1. 多profile文件方式：提供多个配置文件，每个代表一种环境
      1. application-dev.properties/yml 开发环境
      2. application-test.properties/yml 测试环境
      3. application-pro.properties/yml 生产环境
   2. yml多文档方式：
      1. 在yml中使用 --- 分隔不同配置
3. profile激活方式
   1. 配置文件 ： 在配置文件中配置：spring.profiles.active=dev
   2. 虚拟机参数：在VM options指定： -Dspring.profiles.active=dev
   3. 命令行参数：java -jar xxx.jar --spring.profiles.active=dev

内部加载顺序：

springboot程序启动时，会从以下位置加载配置文件：

1. file:./config/：当前项目下的/config目录下
2. file:./：当前项目的根目录
3. classpath:/config/：classpath的/config目录
4. classpath:/：classpath的根目录

加载顺序为上下文的排列顺序，高优先级配置的属性会生效





