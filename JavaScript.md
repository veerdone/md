# JavaScript

## 基础语法

1. 注释：

    1. // 单行注释
    2. /* */  多行注释

2. 数据类型：

    1. 原始数据类型（基本数据类型）
        1. number：数字。整数、小数、Nan（一个不是数字的数字类型）
        2. string：字符串。
        3. boolean：true和false
        4. null：一个对象为空的占位符
        5. undefined：未定义。如果一个变量没有给初始化值，则会被默认赋值为undefined
    2. 引用数据类型：对象

3. 变量

    1. 变量：一小块存储数据的内存空间
    2. Javascript是弱类型语言
        1. 强类型：在开辟变量存储空间时，定义了空间将来存储的数据的数据类型。只能存储固定类型的数据
        2. 弱类型：在开辟变量存储空间时，不定义空间将来的存储数据类型，可以存放任意类型的数据
    3. 语法：
        1. var 变量名 = 初始值;

4. 运算符：

    1. 一元运算符：只有一个运算数的运算符
        1. `++	--	+(正号)	-(负号)`
        2. ++(--)在前，先自增（自减）在运算
        3. ++(--)在后，先运算，在自加（自减）
    2. 算数运算符
        1. ` +	-	*	/	%`
    3. 赋值运算符
        1. `=	+=	 -=`
    4. 比较运算符
        1. `>	<	>=	<=	==	===(全等于)`
        2. 比较方式：
            1. 类型相同，直接比较
                1. 如果比较的是字母，按照字典顺序，逐一比较，知道得出大小为止
            2. 类型不同，先进行类型转换，再比较
            3. === 全等于。在比较之前，先判断类型，如果类型不一样，直接返回false
    5. 逻辑运算符
        1. `&&	||	！
    6. 三元运算符
        1. `?	:`

    

    

    其他类型转number

    1. 在js中，如果运算数不是运算符要求的类型，js引擎会**自动**的将运算数进行**类型转换**
    2. string转number：按照字面值转换，如果字面值不是数字，则转为NaN
    3. boolean转number：true转为1，false转为0

    其他类型转boolean

    1. number：0或NaN为假，其他为真
    2. string：除了空字符串，其他都是true
    3. null和undefined：都为false
    4. 对象：所有对象都为true

    

## 对象

### 基本对象

#### Function：函数对象

1. 创建

    1. ```javascript
        var 方法名称 = new Function(形式参数列表, 方法体); //了解即可
        function 方法名(形式参数列表){
            // 方法体
        }
        var 方法名 = function(形式参数列表){
            // 方法体
        }
        ```
        
        

2. 方法

    1. length：形参的个数 

3. 特点

    1. 方法定义时，形参的类型不用写
    2. 方法是一个对象，如果定义名称相同的方法，会覆盖
    3. 在js中，方法的调用只与方法的名称有关，与参数列表无关
    4. 在方法声明中有一个隐藏的内置对象（数组），arguments，封装所有的实际参数

4. 调用
    1. 方法名称(实际参数列表)

#### Array：数组对象

1. 创建

    ```javascript
    var arr = new Array(元素列表);
    var arr = new Array(默认长度);
    var arr = [元素列表];
    ```

    

2. 方法

    ```js
    join(参数)：将数组中的元素按照指定的分隔符拼接为字符串
    concat()：连接两个或更多的数组，并返回结果
    pop()：删除并返回数组的最后一个元素
    push()：向数组的某位添加一个或更多元素，并返回新的长度
    reverse()：颠倒数组中的元素的顺序
    shift()：删除并返回数组的第一个元素
    slice()：从某个已有的数组返回选定的元素
    sort()：对数组的元素进行排序
    splice()：删除元素，并向数组添加新元素
    toSource()：返回该对象的源代码
    toString()：将数组转换为字符串，并返回结果
    toLocaleString()：将数组转换为本地数组，并返回结果
    unshift()：向数组的开头添加一个或更多元素，并返回新的长度
    valueOf()：返回数组对象的原始值
    ```

    

3. 属性

    1. length：数组的长度

4. 特点

    1. js中，数组元素的类型是可变的
    2. js中，数组的长度是可变的

#### Boolean

#### Date

1. 创建

    ```js
    var date = new Date()
    ```

    

2. 方法

    ```js
    toLocaleString()：返回当前date对象对应的时间本地字符串格式
    getTime()：获取毫秒值，返回当前日期对象所描述的时间到1970年1月1日零时所经历的毫秒值
    ```
    
    

#### Math

1. 创建

    1. 特点：不用创建，直接使用

2. 属性

    1. PI：返回圆周率

3. 方法

    ```js
    random()：返回0~1之间的随机数
    ceil()：对数进行向上取整
    floor()：对数进行向下取整
    round()：对数进行四舍五入
    ```

    

#### Number

#### String

#### RegExp：正则表达式

1. 正则表达式：定义字符串的组成规则

    1. \d：单个数字字符 [ 0~9 ]
    2. \w：单个字符 [ a~zA~Z0~9 ]

2. 量词符号

    1. *：表示出现0此或多次
    2. ?：表示出现0此或1次
    3. +：表示1次或多次
    4. {m,n}：表示 m<= 数量 <=n
        1. {，n}：表示最多n次
        2. {m,}：表示最少m次
    5. 开始结束符号
        1. ^：开始
        2. $：结束

3. 正则对象：

    1. 创建：

        ```js
        var reg = new RegExp("正则表达式");
        var reg = /正则表达式/;
        ```

    2. 方法

        ```js
        test(参数)：验证指定的字符串是否符合正则定义的规范
        ```
        
        

#### Global

1. 特点：全局对象，这个Global中封装的方法不需要对象就可以直接使用

2. 方法

    ```js
    encodeURI()：url编码
    decodeURI()：url解码
    encodeURIComponent()：url编码，编码的字符更多
    decodeURIComponent()：url解码
    parseInt():将字符串转为数字,注意判断每一个字符是否是数字，直到不是数字为止，将前边数字部分转为number
    isNaN()：判断一个值是否是NaN
    eval()：将JavaScript的字符串转为脚本执行
    ```
    
    

### DOM

概念：文档对象模型，将标记语言文档的各个组成部分，封装为对象。可以用这些对象，对标记语言文档进行CRUD的动态操作

三个不同部分：

1. 核心DOM：针对任何结构化文档的标准模型
    1. Document：文档对象
    2. Element：元素对象
    3. Attribute：属性对象
    4. Text：文本对象
    5. Comment：注释对象
    6. Node：节点对象，其他5个的父对象
2. XML DOM：针对XML文档的标准模型
3. HTML DOM：针对HTML文档的标准模型
    1. 标签体的设置和获取：innerHTML
    2. 使用html元素对象的属性
    3. 控制样式
        1. 使用元素的style属性来设置
        2. 提前定义好类选择器的样式，通过元素的className属性来设置样式

获取页面标签(元素)对象 Element

```js
document.getElementById("id值")：通过元素的id获取元素对象
```

操作Element对象：

	修改属性
	修改标签体内容

#### Document：文档对象

1. 创建（获取）：在html dom模型中可以使用window对象来获取

    ```js
    window.document()
    document()
    ```

    

2. 方法

    ```js
    获取Element对象
    getElementById() 根据id属性获取元素对象
    getElementsByTagName() 根据元素名称获取元素对象们，返回值是一个数组
    getElementsByClassName() 根据Class属性值获取元素对象们，返回值是一个数组
    getElementsByName() 根据name属性值获取元素对象们，返回值是一个数组
    创建其他DOM对象
    createAttribute(name)
    createComment()
    createElement()
    createTextNode()
    ```

    

#### Element：元素对象

1. 获取/创建：通过document来获取和创建

2. 方法

    ```js
    removeAttribute() 删除属性
    setAttribute() 设置属性
    ```

    

#### Node：节点对象

1. 特点：所有dom对象都可以被认为是一个节点

2. 方法

    ```js
    CURD dom树：
    appendChild() 向节点的子节点列表的结尾添加新的子节点
    removeChild() 删除（并返回）当前节点的指定子节点
    replaceChild() 用新节点替换一个子节点
    ```

3. 属性

    ```js
    parentNode 返回节点的父节点
    ```

    

### 事件

概念：某些组件被执行了某些操作后，触发某些代码的执行

事件：某些操作。如：单击、双击、键盘按下、鼠标移动

事件源：组件。如：按钮、文本输入框。。。

监听器：代码

注册监听：将事件、事件源、监听器结合在一起，当事件源上发生了某个事件，则触发执行某个监听器代码

常见的事件：

1. 单击事件：
    1. onclick：单击事件
    2. ondblclick：双击事件
2. 焦点事件：
    1. onblur：失去焦点
    2. onfocus：获得焦点
3. 加载事件：
    1. onload：一张页面或一幅图像完成加载
4. 鼠标事件：
    1. onmousedown：鼠标按钮被按下
    2. onmousemove：鼠标被移开
    3. onmouseout：鼠标从某元素移开
    4. onmouseup：鼠标按键被松开
5. 键盘事件：
    1. onkeydown：某个键盘按键被按下
    2. onkeyup：某个键盘按键被松开
    3. onkeypress：某个键盘按键被按下并松开
6. 选择和改变
    1. onchange：域的内容被改变
    2. onselect：文本被选中
7. 表单时间
    1. onsubmit：确认按钮被提交
        1. 可以阻止表单的提交
            1. 方法返回false则表单被阻止提交
    2. onreset：重置按钮被点击

### BOM

概念：浏览器对象模型，将浏览器的各个部分封装为对象

组成：

1. Window：窗口对象 

    1. 特点：Window对象不需要创建可以直接使用，Window引用可以省略

    2. 方法

        ```js
        // 与弹出框有关的方法：
        alert()：显示带有一段消息和一个确认按钮的警告框
        confirm()：显示带有一段消息以及确认按钮和取消按钮的对话框
        	如果点击确认，返回true，如果点击取消，返回false
        prompt()：显示可提示用户输入的对话框
        	获取用户输入的值
        // 与打开关闭有关的方法：
        close()：关闭浏览器窗口
        open()：打开一个新的浏览器窗口
        // 与定时器有关的方法
        setTimeout()：在指定的毫秒数后调用函数或计算表达式
        	参数：
            	js代码或者方法对象
                毫秒值
             返回值：唯一标识，y
        clearTimeout()：取消由setTimeout()方法设置的timeout
        setInterval()：按照指定的周期(以毫秒值)来调用函数或计算表达式
        clearInterval()：取消由setInterval()设置的timeout
        ```

        
        
    3. 获取其他BOM对象

        1. history
        2. location
        3. Naviagton
        4. Screen

    4. 获取DOM对象

        1. document

2. Navigator：浏览器对象

3. Screen：显示器屏幕对象

4. History：历史记录对象

5. Location：地址栏对象

    1. 方法

        ```js
        reload()：重新加载当前文档
        ```

    2. 属性：

        ```js
        href()：设置或返回完整的URL
        ```

        

# JQuery

概念：一个JavaScript的框架，简化js开发

JQuery和js对象方法不通用，两者需要互相转换

- js --> JQuery：$(js对象)
- JQuery --> js：jq对象[索引] 或者 jq对象.get(索引)

## 获取JQuery对象

```js	
var name = $("id选择器或class选择器或者标签名");
// 获取标签体内容
name.html();
//修改标签体内容
name.hmlt("context");
```



## 选择器

1. 分类

   1. 基本选择器

      1. 标签选择器（元素选择器）

         `$("html标签名") 获得所有匹配标签名称的元素`

      2. id选择器

         `$("#id的属性值") 获得与指定id属性值相匹配的元素`

      3. 类选择器

         `$(".class的属性值") 获取与指定的class属性值匹配的元素`
         
      4. 并集选择器

         `$("选择器1,选择器2...") 获取多个选择器选中的所有元素`

   2. 层级选择器

      1. 后代选择器

         `$("A B") 选择A元素内部的所有B元素`

      2. 子选择器

         `$("A > B") 选择A元素内部的所有B子元素`

   3. 属性选择器

      1. 属性名称选择器

         `$("A[属性名]") 包含指定属性的选择器`

      2. 属性选择器

         `$("A[属性名='值']" 包含指定属性等于指定值的选择器)`

      3. 复合属性选择器

         `$("A[属性名='值'][]...") 包含多个属性条件的选择器`

   4. 过滤选择器

      1. 首元素选择器

         `:first 获取选择的元素的第一个元素`

      2. 尾元素选择器

         `:last 获取选择的元素中的最后一个元素`

      3. 非元素选择器

         `:not(selector) 不包含指定内容的元素`

      4. 偶数选择器

         `:even 偶数，从0开始计数`

      5. 奇数选择器

         `:odd 奇数，从0开始计数`

      6. 等于索引选择器

         `:eq(index) 指定索引的元素`

      7. 大于索引选择器

         `:gt(index) 大于指定索引的元素`

      8. 小于索引选择器、

         `:lt(index) 小于指定索引的元素`

      9. 标题选择器

         `:header(h1~h6) 获取标题元素`

   5. 表单过滤选择器

      1. 可用元素选择器

         `:enabled 获得可用元素`

      2. 不可用元素选择器

         `:disabled 获得不可用元素`

      3. 选中选择器

         `:checked 获取单选/复选框选中的元素`
   
      4. 选中选择器
   
         `:selected 获取下拉框选中的元素`

## DOM操作

### 内容操作

1. html() 获取/设置元素的标签体内容
2. text() 获取/设置元素的标签体文本内容
3. val() 获取/设置元素的value属性值

### 属性操作

1. 通用属性操作
   1. attr() 获取/设置元素的属性
   2. removeAttr() 删除属性
   3. prop() 获取/设置元素的属性
   4. removeProp() 删除属性
      1. attr()和prop()的区别：如果操作固有属性，建议使用prop
      2. 如果操作的是自定义元素，则建议使用attr
2. 对class属性操作
   1. addClass() 添加class属性值
   2. removeClass() 删除class属性值
   3. toggleClass() 切换class属性值
      1. toggleClass("name") 判断如果元素对象上存在class="name",则将class="name"删除，如果不存在，则添加
3. CRUD操作
   1. append() 父元素将子元素追加到末尾
      - 对象1.append(对象2) 将对象2添加到对象1元素的内部，并且在末尾
   2. prepend() 父元素将子元素追加到开头
      - 对象1.prepend(对象2) 将对象2添加到对象1元素的内部，并且在开头
   3. appendTo() 
      - 对象1.append(对象2) 将对象1添加到对象2内部，并且在末尾
   4. perpendTo()
      - 对象1.append(对象2) 将对象1添加到对象2内部，并且在开头
   5. afer() 添加元素到元素后边
      - 对象1.after(对象2) 将对象2添加到对象1后边，对象1和对象2是兄弟关系
   6. before() 添加元素到元素前边
      - 对象1.after(对象2) 将对象2添加到对象1前边，对象1和对象2是兄弟关系
   7. insertAfter()
      - 对象1.insertAfter(对象2) 将对象1添加到对象2后边
   8. insertBefore()
      - 对象1.insertBefore(对象2) 将对象1添加到对象2前边
   9. remove() 移除元素
   10. empty() 清空元素的所有后代元素

## 动画

- 三中方式显示和隐藏元素

  - 参数：
    - speed:动画速度，三个预定义的值("slow","normal","fast")，或表示动画时长的毫秒值
    - easing:用来指定切换效果。默认是"swing"，可用参数"linear"
    - fn:在动画完成时执行的函数，每个元素执行一次

  1. 默认显示和隐藏方式
     1. show([seppd,[easing],[fn])
     2. hide([seppd,[easing],[fn])
     3. toggle([seppd,[easing],[fn])
  2. 滑动显示和隐藏方式
     1. slideDown([seppd,[easing],[fn])
     2. slideUp([seppd,[easing],[fn])
     3. slideToggle([seppd,[easing],[fn])
  3. 淡入淡出显示和隐藏方式
     1. fadeIn([seppd,[easing],[fn])
     2. fadeOut([seppd,[easing],[fn])
     3. fadeToggle([seppd,[easing],[fn])

## 遍历

1. jq对象.each(callback)
2. $.each(object,[callback])
3. for..of

## 事件绑定

1. jQuery标准的绑定方式
   1. jq对象.事件方式(回调函数)
2. on绑定事件/off解除绑定
   1. jq对象.on("事件名称",回调函数)
   2. jq对象.off("事件名称")
3. 事件切换：toggle
   1. jq对象.toggle(fn1,fn2...)

## 插件

增强jQuery的功能

1. 实现方式
   1. $.fn.extend(object) 增强通过jQuery获取的对象的功能
   2. $.extend(object) 增强jQuery对象自身的功能

# AJAX

概念：ASynchronous JavaScript And XML

异步和同步：客户端和服务端相互通信的基础上

- 同步：客户端必须等待服务器的响应。在等待的期间客户端不能做其他操作
- 异步：客户端不需要等待服务器端的响应。在服务器请求的过程中，客户端可以进行其他的操作

Ajax是一种在无需重新加载真个网页的情况下，能够更新部分网页的技术



实现方式：

1. 原生的js实现方式（了解）
2. JQuery实现方式
   1. $.ajax()
      - $.ajax({键值对})
   2. $.get ()
   3. $.post()







# JSON

概念：Javascript Object Notation	JavaScript对象表示法

json多用于存储和交换文本信息的语法

语法：

1. 基本规则

   1. 数据在名称/值对中：json数据是由键值对构成的
   2. 键用引号（单双都行）引起来。也可以不用
   3. 值的取值类型
      1. 数字（整数或浮点数）
      2. 字符串（双引号）
      3. 逻辑值（true或false）
      4. 数组（方括号）
      5. 对象（花括号）
   4. 数据由逗号分隔：多个键值对由逗号分隔
   5. 花括号保存对象：使用{ }定义json格式
   6. 方括号保存数组：[ ]

2. 获取数据

   1. json对象.键名
   2. json对象["键名"]
   3. 数组对象[索引]

3. JSON数据转java对象

4. java对象转json

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











